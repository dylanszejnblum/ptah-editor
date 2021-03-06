<template>
  <div class="control-carousel">
    <base-label>
      Carousel settings
    </base-label>

    <!-- pagination -->
    <div class="b-control">
      <base-switcher
        v-model="pagination"
        :label="$t('c.pagiShow')"/>

      <base-color-picker
        v-if="pagination"
        class="b-optional"
        :label="$t('c.pagiColor')"
        v-model="paginationColor"/>

      <base-range-slider
        v-if="pagination"
        class="b-optional"
        v-model="navigationSize"
        :label="$t('c.pagiSize')"
        step="1"
        min="5"
        max="50">
        {{settingObjectSection.data.mainStyle.swiper.navSize}}
      </base-range-slider>
    </div>

    <!-- navigation -->
    <div class="b-control">
      <base-switcher
        v-model="navigation"
        :label="$t('c.navShow')"/>

      <base-color-picker
        v-if="navigation"
        class="b-optional"
        :label="$t('c.navColor')"
        v-model="navigationColor"/>
    </div>

    <div class="b-control">
      <base-switcher
        v-model="autoplay"
        :label="$t('c.autoplay')"/>
      <base-range-slider
        v-if="autoplay !== false"
        class="b-optional"
        :value="settingObjectSection.data.mainStyle.swiper.autoplay.delay"
        :label="$t('c.transDelay')"
        @change="changeSwiperDelay"
        step="1000"
        min="1000"
        max="10000">
        {{settingObjectSection.data.mainStyle.swiper.autoplay.delay}}
      </base-range-slider>
    </div>

    <div class="b-control">
      <base-range-slider
        v-model="frameWidth"
        :label="$t('c.carouselWidth')"
        step="1"
        min="2"
        max="12">
        {{frameWidth}}
      </base-range-slider>
    </div>

  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'
import * as _ from 'lodash-es'

export default {
  name: 'TheControlCarousel',

  data () {
    return {
    }
  },

  computed: {
    ...mapState('Sidebar', [
      'settingObjectOptions',
      'settingObjectElement',
      'settingObjectSection'
    ]),

    autoplay: {
      get () {
        return !!this.settingObjectOptions.swiper.autoplay
      },

      set (value) {
        this.setAutoplay(value)
      }
    },

    navigation: {
      get () {
        return this.settingObjectOptions.swiper.showNavigation
      },

      set (value) {
        this.updateSettingOptions(_.merge({}, this.settingObjectOptions, {
          swiper: {
            showNavigation: value
          }
        }))
      }
    },

    navigationColor: {
      get () {
        return this.settingObjectOptions.swiper.navColor
      },

      set (value) {
        let color = value.rgba ? `rgba(${Object.values(value.rgba).toString()})` : value

        this.updateSettingOptions(_.merge({}, this.settingObjectOptions, {
          swiper: {
            navColor: color
          }
        }))
      }
    },

    navigationSize: { // TODO: pagination size
      get () {
        return parseInt(this.settingObjectOptions.swiper.navSize)
      },

      set (value) {
        this.updateSettingOptions(_.merge({}, this.settingObjectOptions, {
          swiper: {
            navSize: `${value}px`
          }
        }))
      }
    },

    pagination: {
      get () {
        return this.settingObjectOptions.swiper.showPagination
      },

      set (value) {
        this.updateSettingOptions(_.merge({}, this.settingObjectOptions, {
          swiper: {
            showPagination: value
          }
        }))
      }
    },

    paginationColor: {
      get () {
        return this.settingObjectOptions.swiper.paginationColor
      },

      set (value) {
        let color = value.rgba ? `rgba(${Object.values(value.rgba).toString()})` : value

        this.updateSettingOptions(_.merge({}, this.settingObjectOptions, {
          swiper: {
            paginationColor: color
          }
        }))
      }
    },

    frameWidth: {
      get () {
        return this.settingObjectOptions.swiper.frameWidth
      },

      set (value) {
        this.updateSettingOptions(_.merge({}, this.settingObjectOptions, {
          swiper: {
            frameWidth: value
          }
        }))
      }
    }
  },

  methods: {
    ...mapActions('Sidebar', [
      'updateSettingOptions'
    ]),

    setAutoplay (value) {
      if (value) {
        this.updateSettingOptions(_.merge({}, this.settingObjectOptions, {
          swiper: {
            autoplay: {
              disableOnInteraction: false,
              waitForTransition: false,
              delay: 2000
            }
          }
        }))
      } else {
        this.updateSettingOptions(_.merge({}, this.settingObjectOptions, {
          swiper: {
            autoplay: false
          }
        }))
      }
    },

    changeSwiperDelay (delay) {
      this.updateSettingOptions(
        _.merge({}, this.settingObjectOptions, {
          swiper: {
            autoplay: {
              delay
            }
          }
        })
      )
    }
  }
}
</script>

<style lang="sass" scoped>
.b-control
  border-bottom: 0.2rem dotted rgba(0, 0, 0, 0.15)
  padding-bottom: 2.5rem
  margin-bottom: 2.5rem

.b-optional
  margin-top: 1.5rem
</style>
