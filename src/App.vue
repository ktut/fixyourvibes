<script>
import ContactForm from './components/ContactForm.vue'

export default {
  name: 'App',
  components: {
    ContactForm
  },
  data() {
    return {
      glitchText: [
        { char: '$', key: 0 },
        { char: 'h', key: 1 },
        { char: 'i', key: 2 }
      ],
      animationInterval: null,
      glitchIndex: 0, // Track which position shows special character
      logoInverted: false // Track if logo should be inverted
    }
  },
  mounted() {
    this.startAnimation()
    window.addEventListener('scroll', this.handleScroll)
    this.handleScroll() // Check initial state
  },
  beforeUnmount() {
    if (this.animationInterval) {
      clearInterval(this.animationInterval)
    }
    window.removeEventListener('scroll', this.handleScroll)
  },
  methods: {
    startAnimation() {
      const letters = [
        { correct: 's', special: '$' },
        { correct: 'h', special: '%' },
        { correct: 'i', special: '!' }
      ]

      this.animationInterval = setInterval(() => {
        this.glitchText = this.glitchText.map((item, index) => {
          // Only the position at glitchIndex shows its special character
          // The other positions show the correct letter
          const newChar = index === this.glitchIndex
            ? letters[index].special
            : letters[index].correct

          // Only update if character changed
          if (newChar !== item.char) {
            return {
              char: newChar,
              // Only increment key (trigger animation) when changing TO special character
              key: index === this.glitchIndex ? item.key + 3 : item.key
            }
          }
          return item
        })

        // Move to next position
        this.glitchIndex = (this.glitchIndex + 1) % 3
      }, 3000)
    },
    handleScroll() {
      // Check if we're scrolled into a dark section (about-section)
      const aboutSection = document.querySelector('.about-section')
      if (!aboutSection) {
        this.logoInverted = false
        return
      }

      const aboutRect = aboutSection.getBoundingClientRect()
      const logoElement = this.$refs.logo
      if (!logoElement) return

      const logoRect = logoElement.getBoundingClientRect()

      // Check if logo overlaps with about section
      const isOverlapping = logoRect.bottom > aboutRect.top && logoRect.top < aboutRect.bottom

      this.logoInverted = isOverlapping
    },
    scrollToTop() {
      window.scrollTo({
        top: 0,
        behavior: 'smooth'
      })
    }
  }
}
</script>

<template>
  <div class="app">
    <img
      ref="logo"
      src="/assets/robot-fix.png"
      alt="Fix Your Vibes Logo"
      class="logo"
      :class="{ 'logo-inverted': logoInverted }"
      @click="scrollToTop"
    />
    <main class="hero">
      <h1>Your bull<span class="glitch"><span
          v-for="item in glitchText"
          :key="item.key"
          class="glitch-char"
        >{{ item.char }}</span></span>t <br class="break-mobile hide-larger"/>vibe-coded app <br class="break-mobile"/>is going to break <br class="break-mobile hide-larger"/>in production.</h1>
      <h2>Let us take a look instead.</h2>
    </main>

    <div class="transition-section"></div>

    <section class="about-section">
      <div class="journey-container">

        <h2 class="section-title-standalone">The Reality of AI-Generated Code</h2>

        <div class="neuro-card content-card">
          <h3>The Problem</h3>
          <p>
            Modern AI code generators have democratized software development, but they've also
            introduced a new category of technical debt. Applications built by AI tools often
            ship with excessive abstraction layers, over-engineered patterns, duplicated logic,
            and subtle bugs that only manifest in production environments. What looks functional
            in development becomes a maintenance nightmare at scale.
          </p>
        </div>

        <div class="neuro-card content-card">
          <h3>Our Solution</h3>
          <p>
            We specialize in surgical refactoring and remediation of AI-generated SaaS applications.
            Our team performs deep code audits, eliminates unnecessary complexity, patches security
            vulnerabilities, optimizes performance bottlenecks, and restructures architectures for
            long-term maintainability—all while preserving your application's core functionality
            and business logic.
          </p>
        </div>

        <div class="neuro-card content-card large-card">
          <h3>Our Team</h3>
          <p>
            We combine front-end precision with back-end
            infrastructure expertise (Java, Spring Boot, Terraform, AWS), strategic business
            consulting, and rapid development capabilities (Ruby on Rails) to deliver comprehensive
            solutions that don't just fix bugs—they transform fragile codebases into resilient,
            production-ready systems.
          </p>
        </div>

        <div class="neuro-card content-card">
          <h3>Our Approach</h3>
          <p>
            Every engagement begins with a comprehensive technical audit. We trace through your
            entire codebase, identify structural weaknesses, document security risks, and map
            out a prioritized remediation plan. Then we execute with precision—refactoring with
            automated test coverage, modernizing deployment pipelines, and establishing code
            quality standards that prevent regression.
          </p>
        </div>

        <div class="neuro-card content-card">
          <h3>Why It Matters</h3>
          <p>
            Your application is more than code—it's your business engine. When that engine is
            built on unstable foundations, every new feature becomes a gamble, every deployment
            a risk. We give you confidence. Confidence that your infrastructure can scale.
            Confidence that your security posture is sound. Confidence that your team can
            iterate quickly without fear of breaking production.
          </p>
        </div>

      </div>
    </section>

    <ContactForm />
  </div>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Lora:wght@300;400;500;600&display=swap');

@import './styles/global.scss';
@import './styles/variables.scss';

.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.logo {
  position: fixed;
  top: 2rem;
  left: 2rem;
  width: 60px;
  height: 60px;
  cursor: pointer;
  z-index: 1000;
  transition: filter 0.3s ease, transform 0.2s ease;

  &:hover {
    transform: scale(1.1);
  }

  &.logo-inverted {
    filter: invert(1);
  }

  @media (max-width: 768px) {
    width: 50px;
    height: 50px;
    top: 1.5rem;
    left: 1.5rem;
  }
}

.hero {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: $spacing-md;
  text-align: center;
  margin: 0 auto;
  min-height: 100vh;
  @media (min-width: 768px) {
    align-items: center;
  }

  h1,
  h2 {
    text-align: left;
    @media (min-width: 768px) {
      text-align: center;
    }
    @media (min-width: 1200px) {
      .break-mobile.hide-larger {
        display: none;
      }
    }
  }

  h1 {
    font-size: clamp(2.5rem, 6vw, 6rem);
    font-weight: 800;
    margin-bottom: $spacing-sm;
    line-height: 1.2;
    background: $gradient-primary;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;

    .glitch {
      display: inline-block;
      perspective: 1000px;

      .glitch-char {
        display: inline-block;
        animation: flip3d 0.6s ease-in-out;
        transform-style: preserve-3d;
        background: $gradient-primary;
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
      }
    }

    @keyframes flip3d {
      0% {
        transform: rotateX(0deg);
        opacity: 1;
      }
      50% {
        transform: rotateX(90deg);
        opacity: 0;
      }
      51% {
        transform: rotateX(-90deg);
        opacity: 0;
      }
      100% {
        transform: rotateX(0deg);
        opacity: 1;
      }
    }
  }

  h2 {
    font-size: clamp(1.2rem, 3vw, 2rem);
    font-weight: 400;
    color: $text-secondary;
    max-width: $width-container-max;
  }
}

.transition-section {
  position: relative;
  width: 100%;
  height: 50vh;
  background: linear-gradient(180deg,
    rgba(255, 255, 255, 0) 0%,
    rgba(240, 240, 240, 0.05) 10%,
    rgba(220, 220, 220, 0.1) 20%,
    rgba(180, 180, 180, 0.2) 30%,
    rgba(140, 140, 140, 0.35) 40%,
    rgba(100, 100, 100, 0.5) 50%,
    rgba(70, 70, 70, 0.65) 60%,
    rgba(40, 40, 40, 0.8) 70%,
    rgba(25, 25, 25, 0.9) 80%,
    rgba(15, 15, 15, 0.95) 90%,
    #0f0f0f 100%
  );

  @media (max-width: 768px) {
    height: 30vh;
  }
}

.about-section {
  position: relative;
  background: #0f0f0f;
  padding: clamp(5rem, 8vw, 8rem) $spacing-sm clamp(10rem, 10vw, 20rem);
  min-height: auto;

  .journey-container {
    position: relative;
    max-width: $width-hero-max;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    gap: clamp(15rem, 25vw, 30rem);
  }

  .section-title-standalone {
    font-size: clamp(2.5rem, 5vw, 4.5rem);
    font-weight: 700;
    background: linear-gradient(135deg, #ffffff 0%, #d3d3d3 50%, #a0a0a0 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    letter-spacing: -0.03em;
    line-height: 1.1;
    text-align: center;
    margin-bottom: clamp(8rem, 12vw, 15rem);
    width: 100%;
  }

  .neuro-card {
    position: relative;
    padding: clamp(2.5rem, 6vw, 6rem);
    background: #0f0f0f;
    box-shadow:
      20px 20px 60px #050505,
      -20px -20px 60px #1a1a1a,
      inset 2px 2px 5px rgba(255, 255, 255, 0.02),
      inset -2px -2px 5px rgba(0, 0, 0, 0.5);
    transition: all 0.6s cubic-bezier(0.4, 0, 0.2, 1);

    @media (max-width: 768px) {
      padding: clamp(2rem, 4vw, 2.5rem);
    }

    &.content-card {
      h3 {
        font-size: clamp(1.8rem, 3.5vw, 2.5rem);
        font-weight: 600;
        margin-bottom: clamp(1.5rem, 3vw, 3rem);
        color: #e0e0e0;
        letter-spacing: -0.02em;

        @media (max-width: 768px) {
          margin-bottom: 1.5rem;
        }
      }

      p {
        font-family: 'Lora', 'Georgia', 'Times New Roman', serif;
        font-size: clamp(1.05rem, 2vw, 1.2rem);
        line-height: 1.7;
        color: rgba(255, 255, 255, 0.7);
        font-weight: 400;
        text-wrap: balance;

        @media (max-width: 768px) {
          line-height: 1.6;
          font-size: 1.05rem;
        }
      }

      .team-link {
        color: #c0c0c0;
        text-decoration: none;
        font-weight: 500;
        position: relative;
        transition: all 0.3s ease;
        border-bottom: 1px solid rgba(192, 192, 192, 0.3);

        &:hover {
          color: #ffffff;
          border-bottom-color: #ffffff;
        }
      }

      &.large-card {
        padding: clamp(3rem, 7vw, 7rem);
        box-shadow:
          25px 25px 75px #080808,
          -25px -25px 75px #1c1c1c,
          inset 3px 3px 8px rgba(255, 255, 255, 0.03),
          inset -3px -3px 8px rgba(0, 0, 0, 0.6);

        @media (max-width: 768px) {
          padding: 2.5rem;
        }
      }
    }

    &:hover {
      box-shadow:
        25px 25px 70px #030303,
        -25px -25px 70px #1f1f1f,
        inset 3px 3px 6px rgba(255, 255, 255, 0.04),
        inset -3px -3px 6px rgba(0, 0, 0, 0.7);
      transform: translateY(-8px) scale(1.01);
    }

    &:nth-child(even) {
      margin-left: auto;
      margin-right: 0;
      max-width: 85%;

      @media (max-width: 768px) {
        margin-left: 0;
        max-width: 100%;
      }
    }

    &:nth-child(odd):not(.section-title-standalone) {
      margin-left: 0;
      margin-right: auto;
      max-width: 85%;

      @media (max-width: 768px) {
        margin-right: 0;
        max-width: 100%;
      }
    }
  }
}
</style>
