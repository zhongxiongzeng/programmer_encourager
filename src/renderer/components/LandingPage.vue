<template>
  <div id="wrapper">
    <div class="op-box">
      <span class="c-tag">{{index + 1}} / {{assets.length}}</span>
      <button class="cus-btn" @click="to('pre')">&#60;&#60;</button>
      <button class="cus-btn" @click="to('next')">>></button>
      <button class="cus-btn circle-btn" @click="handleOp('min')">-</button>
    </div>
  </div>
</template>

<script>
  const { ipcRenderer: ipc } = require('electron')
  export default {
    name: 'landing-page',
    data () {
      return {
        assets: [
          {
            name: 'live2d-widget-model-shizuku',
            value: 'https://unpkg.com/live2d-widget-model-shizuku@1.0.5/assets/shizuku.model.json'
          },
          {
            name: 'live2d-widget-model-haruto',
            value: 'https://unpkg.com/live2d-widget-model-haruto@1.0.5/assets/haruto.model.json'
          },
          {
            name: 'live2d-widget-model-hibiki',
            value: 'https://unpkg.com/live2d-widget-model-hibiki@1.0.5/assets/hibiki.model.json'
          },
          {
            name: 'live2d-widget-model-hijiki',
            value: 'https://unpkg.com/live2d-widget-model-hijiki@1.0.5/assets/hijiki.model.json'
          },
          {
            name: 'live2d-widget-model-izumi',
            value: 'https://unpkg.com/live2d-widget-model-izumi@1.0.5/assets/izumi.model.json'
          },
          {
            name: 'live2d-widget-model-koharu',
            value: 'https://unpkg.com/live2d-widget-model-koharu@1.0.5/assets/koharu.model.json'
          },
          {
            name: 'live2d-widget-model-miku',
            value: 'https://unpkg.com/live2d-widget-model-miku@1.0.5/assets/miku.model.json'
          },
          {
            name: 'live2d-widget-model-tororo',
            value: 'https://unpkg.com/live2d-widget-model-tororo@1.0.5/assets/tororo.model.json'
          },
          {
            name: 'live2d-widget-model-wanko',
            value: 'https://unpkg.com/live2d-widget-model-wanko@1.0.5/assets/wanko.model.json'
          },
          {
            name: 'live2d-widget-model-z16',
            value: 'https://unpkg.com/live2d-widget-model-z16@1.0.5/assets/z16.model.json'
          },
          {
            name: 'live2d-widget-model-haru_01',
            value: 'https://unpkg.com/live2d-widget-model-haru_01@1.0.5/assets/haru_01.model.json'
          }
        ],
        index: 0
      }
    },
    mounted () {
      this.canMove()
      this.init()
    },
    methods: {
      init () {
        if (window.L2Dwidget) {
          window.L2Dwidget.init({
            'model': {
              jsonPath: this.assets[this.index].value,
              scale: 1
            },
            'display': {
              'position': 'right',
              'width': 150,
              'height': 500,
              'hOffset': 0,
              'vOffset': -100
            },
            'mobile': {
              'show': true,
              'scale': 0.5
            },
            'react': {
              'opacityDefault': 0.7,
              'opacityOnHover': 0.2
            }
          })
        } else {
          setTimeout(() => {
            this.init()
          }, 5000)
        }
      },
      canMove () {
        let win = this.$electron.remote.getCurrentWindow()
        let biasX = 0
        let biasY = 0
        let that = this
        document.addEventListener('mousedown', function (e) {
          switch (e.button) {
            case 0:
              biasX = e.x
              biasY = e.y
              document.addEventListener('mousemove', moveEvent)
              break
            case 2:
              that.$electron.ipcRenderer.send('createSuspensionMenu')
              break
          }
        })

        document.addEventListener('mouseup', function () {
          biasX = 0
          biasY = 0
          document.removeEventListener('mousemove', moveEvent)
        })

        function moveEvent (e) {
          win.setPosition(e.screenX - biasX, e.screenY - biasY)
        }
      },
      to (dir) {
        if (dir === 'next') {
          if (this.index < this.assets.length - 1) this.index++
          else this.index = 0
          this.init()
        } else if (dir === 'pre') {
          if (this.index === 0) this.index = this.assets.length - 1
          else this.index--
          this.init()
        }
      },
      handleOp (type) {
        ipc.send(type)
      },
      toAdd () {

      }
    }
  }
</script>

<style lang='scss'>
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    cursor: url('https://images.cnblogs.com/cnblogs_com/mxiaoli/1534839/o_cursor.png'), auto;
  }

  *:hover {
    cursor: url('https://images.cnblogs.com/cnblogs_com/mxiaoli/1534839/o_0000111.png'), auto;
  }

  *, *::before, *::after {
    outline: 0;
  }
  body {
    font-family: 'Source Sans Pro', sans-serif;
  }

  #wrapper {
    height: 100vh;
    padding: 60px 80px;
    width: 100vw;

    .op-box{
      position: absolute;
      right: 30px;
      top: -100px;
      transition: all .3s linear;
      .c-tag{
        background: rgba(0, 0, 0, 0.2);
        color: rgb(255, 255, 255);
        padding: 0 8px;
        height: 20px;
        border-radius: 10px;
        position: relative;
        top: 2px;
      }
      .cus-btn{
        cursor: pointer;
        background: rgba(0, 0, 0, 0.2);
        border: none;
        border-radius: 10px;
        height: 20px;
        border: 2px solid rgb(218, 196, 200);
        padding: 0 8px;
        color: rgb(255, 255, 255);
        transition: all .3s linear;

        &.circle-btn{
          padding: 0;
          width: 20px;
          border: 2px solid rgb(218, 196, 200);
          line-height: 10px;
          border-radius: 50%;
        }
        &:hover{
          background: rgba(0, 0, 0, 0.5);
        }
      }
    }
    &:hover{
      .op-box{
        top: 30px;
      }
    }

  }

</style>
