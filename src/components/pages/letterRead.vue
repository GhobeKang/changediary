<template>
  <f7-page id="letterRead">
    <swiper :options="swiperOption" ref="slider">
      <template v-for="item in letterData">
        <swiper-slide>
          <f7-block class="letter_wrap">
            <f7-block-header class="letter_header">
            <p>To. {{item.target.split('@')[0]}}</p>
            </f7-block-header>
            <f7-block class="letter_content">
            <p v-html="item.contents"></p>
            </f7-block>
            <f7-block-footer class="letter_footer">
            <p>From. {{item.sender.split('@')[0]}}</p>
            </f7-block-footer>
            <f7-link text="New Letter" :reload-all="true" :force="true" @click="transToWrite(item.target)"></f7-link>
          </f7-block>
        </swiper-slide>
      </template>
    </swiper>
  </f7-page>
</template>

<script>
  import '../../assets/js/extra/turn'
  import $ from 'jquery'
  import SwiperSlide from '../../../node_modules/vue-awesome-swiper/src/slide'

  export default {
    components: {SwiperSlide},
    data: function () {
      return {
        letterData: {},
        swiperOption: {
          effect: 'flip',
          grabCursor: true,
          centeredSlides: true,
          width: window.innerWidth,
          height: window.innerHeight - 44
        }
      }
    },
    methods: {
      transToWrite: function (target) {
        window.f7.mainView.router.load({url: '/letter_W/?target=' + target})
      }
    },
    mounted: function () {
      var $$ = window.Dom7
      var _this = this
      var width = window.innerWidth
      var height = window.innerHeight - 44
      $$(document).on('page:init', '.page[id="letterRead"]', function (ev) {
        var page = window.f7.mainView.activePage
        _this.letterData = JSON.parse(page.query.data)
        _this.$nextTick(function () {
          $('#flipbook').turn({
            width: width,
            height: height,
            display: 'single',
            autoCenter: true
          })
        })
//        for (var i in _this.letterData) {
//          var content = '<div><p>' + _this.letterData[i].target + '</p><p>' + _this.letterData[i].contents + '</p><p>' + _this.letterData[i].timestamp + '</p><p>' + _this.letterData[i].sender + '</p></div>'
//          $('#flipbook').turn('addPage', content)
//        }
      })
      $$('.page[id="letterRead"]').on('page:beforeremove', function (ev) {
        $$('.page[id="letterRead"]').remove()
      })
    }
  }
</script>

<style scoped>
  .swiper-container {
    height: 100%;
    top: 0;
  }
  .swiper-slide {
    top: -12px;
  }
</style>
