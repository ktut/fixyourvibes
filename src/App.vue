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

<style lang="scss">
@import './styles/global.scss';
@import './styles/variables.scss';

.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.hero {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: $spacing-md;
  text-align: center;
  max-width: $width-hero-max;
  margin: 0 auto;

  h1 {
    font-size: clamp(2rem, 5vw, 4rem);
    font-weight: 800;
    margin-bottom: $spacing-sm;
    line-height: 1.2;
    background: $gradient-primary;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;

    .glitch {
      font-family: 'Courier New', monospace;
      font-weight: 900;
      letter-spacing: 0.05em;
      display: inline-block;
      background: $gradient-primary;
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }
  }

  h2 {
    font-size: clamp(1.2rem, 3vw, 2rem);
    font-weight: 400;
    color: $text-secondary;
    max-width: $width-container-max;
  }
}
</style>
