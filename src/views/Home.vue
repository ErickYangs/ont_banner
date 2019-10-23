<template>
  <div class="home">
    <div class="nav_bar">
      <div class="logo"></div>
      <div class="main_nav">
        <ul class="nav_box">
          <li id="men_pro" class="active">
            <div class="main_title">Home</div>
          </li>
          <li class="main_men">
            <div class="main_title">Developer</div>
            <div class="sub_title">
              <div class="sub_link">Developer Center</div>
              <div class="sub_link">Bounty Program</div>
              <div class="sub_link">Explorer</div>
            </div>
          </li>
          <li class="main_men">
            <div class="main_title">dApps</div>
            <div class="sub_title">
              <div class="sub_link">dApps List</div>
              <div class="sub_link">Submit a Dapp</div>
            </div>
          </li>
          <li class="main_men">
            <div class="main_title">Wallets</div>
            <div class="sub_title">
              <div class="sub_link">ONTO</div>
            </div>
          </li>
          <li class="main_men">
            <div class="main_title">Trust Ecosystem</div>
            <div class="sub_title">
              <div class="sub_link">Pax</div>
            </div>
          </li>
          <li class="main_men">
            <div class="main_title">About</div>
            <div class="sub_title">
              <div class="sub_link">News</div>
            </div>
          </li>
        </ul>
      </div>
      <div class="lang">
        <div class="lang_box">
          English
          <i class="lang_down"></i>
        </div>
      </div>
      <div class="subtitle_bg" id="menu_hover_bg"></div>
    </div>
    <div class="text_layout">
      <div class="main_ti">TRUST Redefined</div>
      <div class="sun_tit">Ready for All Business</div>
      <div
        class="sun_tit floortb"
      >Ontology is a high-performance public blockchain project & a distributed trust collaboration platform.</div>
      <div class="learn_more">Learn more</div>
    </div>
    <div class="arrow_down"></div>
    <div class="bgCans">
      <canvas id="cvs"></canvas>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import $ from 'jquery'
import * as THREE from 'three'
export default {
  name: 'home',
  mounted() {
    // var $obj = $('.main_nav ul')
    // $obj.find('li').on('mouseenter', function () {
    //   $(this).addClass('active')
    // })
    // $obj.find('li').on('mouseleave', function () {
    //   $(this).removeClass('active')
    // })
    $('.main_men').on('mouseenter', function () {
      $('#menu_hover_bg')
        .stop()
        .animate({ height: '58px' }, 500)
      $(this).siblings().find('.sub_title').stop()
        .fadeOut()
      // $(this).siblings().find('.sub_title').stop()
      //   .animate({ height: '0' }, 500)
      // $(this).find('.sub_title').stop()
      //   .animate({ height: '70px' }, 500)
      $(this).find('.sub_title').stop()
        .fadeIn()

      // $('#men_pro .sub_title')
      //   .stop()
      //   .animate({ height: '70px' }, 500)
    })
    $('.main_men').on('mouseleave', function () {
      $('#menu_hover_bg')
        .stop()
        .animate({ height: '0' }, 500)
      // $(this).find('.sub_title')
      //   .stop()
      //   .animate({ height: '0' }, 500)

      $(this).find('.sub_title')
        .stop()
        .fadeOut()

    })






    function Waves(cvs, options) {
      var self = this,
        paused = true,
        rafid;

      self.isOK = false;

      self.defaults = {
        accuracy: 1.5,
        speed: 0.16,
        offset: 246,
        height: 30,
        color: 0xFFFFFF,
        bgColor: 0xFFFFFF,
        size: 1000,
        segments: 40,
        camera: {
          perspective: 70,
          position: { x: 450, y: 40, z: 0 },
          lookAt: { x: 0, y: 140, z: 50 }
        },
        fog: {
          enabled: true,
          density: 0.005,
        },
        renderCallback: function (s) { }
      };

      self.isPaused = function () {
        return paused;
      };
      self.dots = [];

      function extend(destination, source) {
        for (var property in source) {
          if (typeof source[property] === 'object')
            destination[property] = extend(destination[property], source[property]);
          else
            destination[property] = source[property];
        }
        return destination;
      }

      function addEvent(elem, type, eventHandle) {
        if (elem == null || elem == undefined) return;
        if (elem.addEventListener) {
          elem.addEventListener(type, eventHandle, false);
        } else if (elem.attachEvent) {
          elem.attachEvent("on" + type, eventHandle);
        } else {
          elem["on" + type] = eventHandle;
        }
      };

      function getDevicePixelRatio() {
        var ratio = 1;
        // To account for zoom, change to use deviceXDPI instead of systemXDPI
        if (window.screen.systemXDPI !== undefined && window.screen.logicalXDPI !== undefined && window.screen.systemXDPI > window.screen.logicalXDPI) {
          // Only allow for values > 1
          ratio = window.screen.systemXDPI / window.screen.logicalXDPI;
        }
        else if (window.devicePixelRatio !== undefined) {
          ratio = window.devicePixelRatio;
        }
        return ratio;
      }

      function OnResizeCVS() {
        var pixelRatio = 1 / getDevicePixelRatio();
        self.camera.aspect = window.innerWidth / window.innerHeight;
        self.camera.updateProjectionMatrix();
        self.renderer.setSize(window.innerWidth * pixelRatio * self.options.accuracy, window.innerHeight * pixelRatio * self.options.accuracy, false);
      }

      function init() {
        self.options = extend(self.defaults, options);

        if (!!!window.WebGLRenderingContext) return false;

        try {
          self.renderer = new THREE.WebGLRenderer({ canvas: cvs, antialias: true });
          self.renderer.setClearColor(self.options.bgColor);
          // SCENE
          self.scene = new THREE.Scene();

          if (self.options.fog.enabled)
            self.scene.fog = new THREE.FogExp2(self.options.bgColor, self.options.fog.density);

          // CAMERA
          self.camera = new THREE.PerspectiveCamera(self.options.camera.perspective, window.innerWidth / window.innerHeight, 1, 10000);
          self.camera.position.set(self.options.camera.position.x, self.options.camera.position.y, self.options.camera.position.z);
          self.lookAt = new THREE.Vector3(self.options.camera.lookAt.x, self.options.camera.lookAt.y, self.options.camera.lookAt.z);
          self.camera.lookAt(self.lookAt);
          self.scene.add(self.camera);

          // ON WINDOW RESIZE
          addEvent(window, 'resize', OnResizeCVS);
          OnResizeCVS();

          // PLANE
          var material = new THREE.MeshBasicMaterial({
            color: self.options.color,
            wireframe: true,
          });
          var geometry = new THREE.PlaneGeometry(self.options.size, self.options.size * 2, self.options.segments, self.options.segments * 2);

          self.mesh = new THREE.Mesh(geometry, material);
          self.mesh.rotation.x = Math.PI / 2;
          self.scene.add(self.mesh);

        } catch (e) {
          return false;
        }
        return true;
      }
      self.isOK = init();

      function animate() {
        rafid = requestAnimationFrame(animate);
        waves(Date.now() / 1000);
        self.options.renderCallback(self);
        self.renderer.render(self.scene, self.camera);
      }

      function waves(time) {

        var verts = self.mesh.geometry.vertices;

        for (var i = 0, len = verts.length; i < len; i++) {
          verts[i].z = Math.sin(verts[i].x * Math.PI / self.options.offset + time * self.options.speed)
            * Math.cos(verts[i].y * Math.PI / self.options.offset + time * self.options.speed)
            * self.options.height;
        }

        self.mesh.geometry.verticesNeedUpdate = true;
      }

      self.toggle = function (run) {
        if (!self.isOK) return false;

        if (run === undefined)
          self.toggle(!paused);

        else if (!!run && paused) {
          paused = false;
          animate();
        }
        else if (!!!run) {
          paused = true;
          cancelAnimationFrame(rafid);
        }
        return true;
      }
    }

    (function () {
      console.log(12312)
      var cvs = document.getElementById('cvs');
      var mouseX = 0;

      document.onmousemove = function (e) {
        mouseX = e.clientX;
      };

      var waves = new Waves(cvs, {
        color: 0xBBBBBB,
        renderCallback: function (v) {
          var targetX = mouseX / window.innerWidth * 200 - 100;
          v.lookAt.z += (-targetX - v.lookAt.z) / 100;
          v.camera.lookAt(v.lookAt);
        }
      });

      waves.toggle(true);
    })();
  }
}
</script>

<style lang="less" scoped>
.home {
  height: 100vh;
  position: relative;
  .nav_bar {
    width: 100%;
    height: 70px;
    background: #fff;
    display: flex;
    justify-content: flex-start;
    position: fixed;
    left: 0;
    top: 0;
    box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.04);
    z-index: 99;
    .logo {
      width: 140px;
      height: 70px;
      // float: left;
      background: url(../assets/images/logo_dark.svg) no-repeat center;
      background-size: contain;
      margin: 0 100px 0 20px;
    }
    .lang {
      width: 76px;
      height: 70px;
      // float: right;
      margin-left: auto;
      display: flex;
      justify-content: center;
      align-items: center;
      margin-right: 20px;

      .lang_box {
        width: 76px;
        height: 25px;
        line-height: 25px;
        background: rgba(250, 250, 250, 1);
        border-radius: 13px;
        border: 1px solid rgba(242, 242, 242, 1);
        font-size: 10px;
        font-family: Roboto-Medium;
        color: rgba(0, 0, 0, 1);
        padding: 0 10px;
        position: relative;
        .lang_down {
          position: absolute;
          display: block;
          width: 6px;
          height: 4px;
          background: url(../assets/images/lang.svg) no-repeat center;
          background-size: contain;
          right: 10px;
          top: 50%;
          transform: translateY(-50%);
        }
      }
    }
    .main_nav {
      // width: 100%;
      // max-width: 1280px;
      height: 70px;
      background: green;
      position: relative;
      .nav_box {
        width: 100%;
        // max-width: 1280px;
        // margin: 0 auto;
        height: 70px;
        background: #fff;
        display: flex;
        justify-content: flex-start;
        position: relative;

        li {
          margin-right: 50px;
          height: 70px;
          cursor: pointer;
          .main_title {
            line-height: 70px;
            font-family: Roboto-Regular;
            font-size: 13px;
            position: relative;
            // font-family: Roboto-Medium;
            // font-family: Roboto-Black;
          }
          .main_title::after {
            content: "";
            width: 0;
            height: 4px;
            background: rgba(0, 0, 0, 1);
            position: absolute;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            transition: width 0.5s;
          }
          .sub_title {
            position: absolute;
            left: 0;
            width: 100vh;
            height: 58px;
            top: 70px;
            // background: green;
            color: #000;
            z-index: 9;
            overflow: hidden;
            display: none;
            .sub_link {
              float: left;
              margin-right: 50px;
              line-height: 58px;
              font-size: 13px;
              font-family: Roboto-Regular;
              font-weight: 400;
              color: rgba(0, 0, 0, 0.6);
            }
          }
          &:hover {
            .main_title::after {
              width: 35px;
            }
          }
        }
        li.active {
          .main_title::after {
            width: 35px;
          }
        }
      }
    }
    .subtitle_bg {
      position: absolute;
      width: 100%;
      height: 0px;
      background: rgba(250, 250, 250, 1);
      left: 0;
      top: 70px;
    }
  }
  .text_layout {
    width: 100%;
    height: 100vh;
    position: absolute;
    left: 0;
    top: 0;
    // background: tomato;
    padding-top: 34vh;
    box-sizing: border-box;
    z-index: 4;
    .main_ti {
      font-size: 50px;
      font-family: Roboto-Black;
      font-weight: 900;
      color: rgba(0, 0, 0, 1);
      line-height: 66px;
      text-align: center;
      margin-bottom: 10px;
    }
    .sun_tit {
      font-size: 18px;
      font-family: Roboto-Regular;
      font-weight: 400;
      color: rgba(0, 0, 0, 1);
      line-height: 24px;
      text-align: center;
    }
    .floortb {
      margin: 50px 0;
    }
    .learn_more {
      width: 180px;
      height: 54px;
      border-radius: 2px;
      border: 2px solid rgba(0, 0, 0, 1);
      line-height: 52px;
      font-size: 14px;
      font-family: Roboto-Medium;
      text-align: center;
      margin: 0 auto;
      cursor: pointer;
    }
  }
  .arrow_down {
    position: absolute;
    width: 60px;
    height: 60px;
    background: #000;
    border-radius: 50%;
    left: 50%;
    bottom: 60px;
    transform: translateX(-50%);
    z-index: 4;
  }
  .bgCans {
    width: 100%;
    height: 100%;
    position: absolute;
    left: 0;
    top: 0;
    // background: tomato;
    canvas {
      width: 100%;
      height: 100%;
      pointer-events: none;
      touch-callout: none;
    }
  }
}
</style>

