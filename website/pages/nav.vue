<template>
  <div>
    <h2>{{ langConfig[1] }}</h2>
    <div class="block">
      <p>{{ langConfig[2] }}</p>
    </div>
    <div class="block">
      <h3>{{ langConfig[3] }}</h3>
      <p>{{ langConfig[4] }}</p>
    </div>
    <div class="block">
      <h3>{{ langConfig[5] }}</h3>
      <el-row :gutter="20">
        <el-col :span="9">
          <p>{{ langConfig[6] }}</p>
        </el-col>
        <el-col :span="15" class="nav-demos">
          <img
            src="~examples/assets/images/navbar_1.png"
            alt="{{ langConfig[7] }}"
            @click="enlarge(846, $event)"
          />
          <h5>{{ langConfig[7] }}</h5>
          <p>{{ langConfig[8] }}</p>
          <img
            src="~examples/assets/images/navbar_2.png"
            alt="{{ langConfig[9] }}"
            @click="enlarge(846, $event)"
          />
          <h5>{{ langConfig[9] }}</h5>
          <p>{{ langConfig[10] }}</p>
          <img
            src="~examples/assets/images/navbar_3.png"
            alt="{{ langConfig[11] }}"
            @click="enlarge(846, $event)"
          />
          <h5>{{ langConfig[11] }}</h5>
          <p>{{ langConfig[12] }}</p>
        </el-col>
      </el-row>
    </div>
    <div class="block">
      <h3>{{ langConfig[13] }}</h3>
      <el-row>
        <el-col :span="10">
          <p>{{ langConfig[14] }}</p>
        </el-col>
        <el-col :span="14" class="nav-demos">
          <img
            src="~examples/assets/images/navbar_0.png"
            alt=""
            @click="enlarge(846, $event)"
          />
          <p>{{ langConfig[15] }}</p>
        </el-col>
      </el-row>
    </div>
    <transition name="fade">
      <div v-show="showDialog" class="mask" @click="showDialog = false"></div>
    </transition>
    <transition name="zoom">
      <div
        v-show="showDialog"
        class="dialog-img"
        :style="imgWrapStyle"
        @click="showDialog = false"
      >
        <div class="imgWrap" :style="imgStyle">
          <img :src="imgUrl" alt="" />
        </div>
      </div>
    </transition>
  </div>
</template>
<script>
import pageLang from '../i18n/page.json'
export default {
  data() {
    return {
      imgUrl: '',
      imgBound: {},
      showDialog: false,
      imgStyle: {},
      imgWrapStyle: {},
      lang: this.$route.meta.lang,
    }
  },
  computed: {
    langConfig() {
      return pageLang.filter((config) => config.lang === this.lang)[0].pages.nav
    },
  },
  watch: {
    showDialog(val) {
      document.body.style.overflow = val ? 'hidden' : ''
    },
  },
  methods: {
    enlarge(imgWidth, ev) {
      var imgNode = ev.target
      // var bound = imgNode.getBoundingClientRect();
      var offset = {}
      var doc = document

      offset.left = (doc.body.scrollWidth - imgWidth) / 2
      offset.top = 100

      this.imgUrl = imgNode.src
      this.imgBound = imgNode.getBoundingClientRect()

      this.imgWrapStyle.transformOrigin = `${ev.clientX}px ${ev.clientY}px`
      // this.imgStyle.transformOrigin = `${ev.clientX}px ${ev.clientY}px`;
      this.imgStyle.width = imgWidth + 'px'
      this.showDialog = true
    },
  },
}
</script>
<style lang="scss" scoped>
h3 {
  margin-bottom: 15px;
}
.block {
  margin-bottom: 55px;
}
p {
  margin: 0 0 15px;
}
.nav-demos {
  p {
    margin-bottom: 50px;
  }
  h5 {
    margin: 0;
  }
  img {
    padding: 15px;
    background-color: #f9fafc;
    width: 100%;
    margin-bottom: 15px;
    cursor: pointer;
  }
}
.dialog-img {
  position: fixed;
  overflow: auto;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1000;
  -webkit-overflow-scrolling: touch;
  outline: 0;

  .imgWrap {
    margin: 0 auto;
    position: relative;
    top: 100px;
    padding-bottom: 50px;
  }
  img {
    display: block;
    width: 100%;
  }
}
.mask {
  position: fixed;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  background-color: #373737;
  background-color: rgba(55, 55, 55, 0.6);
  height: 100%;
  z-index: 1000;
}
.zoom-enter-active,
.zoom-leave-active {
  transition: transform 0.3s cubic-bezier(0.78, 0.14, 0.15, 0.86),
    opacity 0.3s cubic-bezier(0.78, 0.14, 0.15, 0.86);
}
.zoom-enter,
.zoom-leave-active {
  transform: scale(0.3);
  opacity: 0;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s cubic-bezier(0.78, 0.14, 0.15, 0.86);
}
.fade-enter,
.fade-leave-active {
  opacity: 0;
}
</style>
