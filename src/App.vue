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
      animationInterval: null,
      glitchIndex: 0 // Track which position shows special character
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
      const correctLetters = ['s', 'h', 'i', 't']
      this.animationInterval = setInterval(() => {
        this.glitchText = Array.from({ length: 4 }, (_, index) => {
          // Only the position at glitchIndex shows a special character
          // The other 3 positions show the correct letter
          if (index === this.glitchIndex) {
            return this.animatedChars[Math.floor(Math.random() * this.animatedChars.length)]
          }
          return correctLetters[index]
        }).join('')

        // Move to next position
        this.glitchIndex = (this.glitchIndex + 1) % 4
      }, 1000)
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
