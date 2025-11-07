<script>
import ContactForm from './components/ContactForm.vue'

export default {
  name: 'App',
  components: {
    ContactForm
  },
  data() {
    return {
      animatedChars: ['$', '%', '*', '&', '!'],
      glitchText: '$$$$',
      animationInterval: null
    }
  },
  mounted() {
    this.startAnimation()
  },
  beforeUnmount() {
    if (this.animationInterval) {
      clearInterval(this.animationInterval)
    }
  },
  methods: {
    startAnimation() {
      this.animationInterval = setInterval(() => {
        this.glitchText = Array.from({ length: 4 }, () =>
          this.animatedChars[Math.floor(Math.random() * this.animatedChars.length)]
        ).join('')
      }, 400)
    }
  }
}
</script>

<template>
  <div class="app">
    <main class="hero">
      <h1>Your bull<span class="glitch">{{ glitchText }}</span> vibe-coded app is going to break in production.</h1>
      <h2>We can keep that from happening.</h2>
    </main>

    <ContactForm />
  </div>
</template>

<style lang="sass">
*
  margin: 0
  padding: 0
  box-sizing: border-box

body
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif
  background: #0a0a0a
  color: #fff
  line-height: 1.6

.app
  min-height: 100vh
  display: flex
  flex-direction: column

.hero
  flex: 1
  display: flex
  flex-direction: column
  justify-content: center
  align-items: center
  padding: 2rem
  text-align: center
  max-width: 900px
  margin: 0 auto

  h1
    font-size: clamp(2rem, 5vw, 4rem)
    font-weight: 800
    margin-bottom: 1.5rem
    line-height: 1.2
    background: linear-gradient(135deg, #ff6b6b, #ee5a6f, #c44569)
    -webkit-background-clip: text
    -webkit-text-fill-color: transparent
    background-clip: text

    .glitch
      font-family: 'Courier New', monospace
      font-weight: 900
      letter-spacing: 0.05em
      display: inline-block
      background: linear-gradient(135deg, #ff6b6b, #ee5a6f, #c44569)
      -webkit-background-clip: text
      -webkit-text-fill-color: transparent
      background-clip: text

  h2
    font-size: clamp(1.2rem, 3vw, 2rem)
    font-weight: 400
    color: #aaa
    max-width: 600px
</style>
