<template>
  <main>
    <!-- <SectionHero /> -->
    <SmoothScroll>
      <!-- <SectionSpace /> -->
      <SectionItemLeft />
      <!-- <SectionItemRight /> -->
      <SectionCarousel />
      <SectionSpace />
    </SmoothScroll>
    <CursorFollower />
  </main>
</template>

<script>
// import SectionHero from './SectionHero'
import SectionSpace from './SectionSpace'
import SectionItemLeft from './SectionItemLeft'
// import SectionItemRight from './SectionItemRight'
import SectionCarousel from './SectionCarousel'
import SmoothScroll from '~/components/SmoothScroll'
import CursorFollower from '~/components/CursorFollower'

export default {
  components: {
    // SectionHero,
    SectionSpace,
    SectionItemLeft,
    // SectionItemRight,
    SmoothScroll,
    SectionCarousel,
    CursorFollower
  },

  data() {
    return {
      scroll: null
    }
  },

  head() {
    return {
      title:
        this.$config.app.title +
        this.$config.app.titleSeparator +
        this.$config.app.titleTemplate,

      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.$config.app.description
        }
      ]
    }
  },

  mounted() {
    this.locomotiveScrollInit()
    // this.setHeroAnimation()
    this.setItemAnimation()
  },

  beforeDestroy() {
    this.scroll.destroy()
  },

  methods: {
    locomotiveScrollInit() {
      this.scroll = new this.$LocomotiveScroll({
        el: document.querySelector('[data-scroll-container]'),
        smooth: true,
        getDirection: true
      })

      // this.scroll.stop()
    },

    setHeroAnimation() {
      const ease = 0.05
      const star = document.querySelector('[data-img-star]')

      const onComplete = () => {
        this.scroll.update()
        this.scroll.start()

        this.scroll.on('scroll', instance => {
          star.style.transform = `rotate(${instance.scroll.y * ease}deg)`
        })
      }

      const tl = this.$gsap.timeline({
        defaults: { duration: 3, ease: 'expo.out' }
      })

      tl.to('[data-svg-title]', {
        y: 0,
        opacity: 1
      }).to(
        '[data-img-star]',
        {
          opacity: 1,
          scale: 1,
          rotation: 360,
          onComplete
        },
        '-=2.5'
      )
    },

    setItemAnimation() {
      this.scroll.on('scroll', instance => {
        if (Object.keys(instance.currentElements).includes('hey')) {
          // const element = instance.currentElements.hey
          console.log(instance.scroll.y)
          instance.currentElements.hey.el.style.left = `${
            (instance.scroll.y - 107) / 2 - 600
          }px`
        }
      })

      this.scroll.on('call', (value, way, obj) => {
        if (value === 'itemAnimation') {
          if (way === 'enter') {
            this.$gsap.to(obj.el, {
              scaleX: 1,
              ease: 'expo.out',
              duration: 3
            })
          }
        }
      })
    }
  }
}
</script>
