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
    }
  }
}
</script>

<template>
  <div class="app">
    <main class="hero">
      <h1>Your bull<span class="glitch"><span
          v-for="item in glitchText"
          :key="item.key"
          class="glitch-char"
        >{{ item.char }}</span></span>t <br class="break-mobile"/>vibe-coded app <br class="break-mobile"/>is going to break in production.</h1>
      <h2>Let’s not let that happen.</h2>
    </main>

    <section class="about-section">
      <div class="about-background">
        <svg class="bg-shape shape-1" viewBox="0 0 200 200" preserveAspectRatio="none">
          <polygon points="0,0 200,40 200,200 0,160" />
        </svg>
        <svg class="bg-shape shape-2" viewBox="0 0 200 200" preserveAspectRatio="none">
          <polygon points="0,60 200,0 200,140 0,200" />
        </svg>
        <svg class="bg-shape shape-3" viewBox="0 0 200 200" preserveAspectRatio="none">
          <polygon points="0,100 200,80 200,200 0,200" />
        </svg>
      </div>

      <div class="about-content">
        <div class="section-header">
          <h2 class="section-title">The Reality of AI-Generated Code</h2>
          <div class="title-underline"></div>
        </div>

        <div class="about-grid">
          <div class="about-card primary-card">
            <div class="card-gradient"></div>
            <div class="card-content">
              <h3>The Problem</h3>
              <p>
                Modern AI code generators have democratized software development, but they've also
                introduced a new category of technical debt. Applications built by AI tools often
                ship with excessive abstraction layers, over-engineered patterns, duplicated logic,
                and subtle bugs that only manifest in production environments. What looks functional
                in development becomes a maintenance nightmare at scale.
              </p>
            </div>
          </div>

          <div class="about-card secondary-card">
            <div class="card-gradient"></div>
            <div class="card-content">
              <h3>Our Solution</h3>
              <p>
                We specialize in surgical refactoring and remediation of AI-generated SaaS applications.
                Our team performs deep code audits, eliminates unnecessary complexity, patches security
                vulnerabilities, optimizes performance bottlenecks, and restructures architectures for
                long-term maintainability—all while preserving your application's core functionality
                and business logic.
              </p>
            </div>
          </div>

          <div class="about-card tertiary-card full-width">
            <div class="card-gradient"></div>
            <div class="card-content">
              <h3>Our Team</h3>
              <p>
                Led by <a href="https://rkdvis.com" target="_blank" rel="noopener" class="team-link">Ramzi Dreessen</a>,
                a seasoned front-end engineer and product specialist with deep expertise in AI-assisted
                development workflows (Cursor, Claude), our multidisciplinary team brings together
                specialized talent across the full stack. We combine front-end precision with back-end
                infrastructure expertise (Java, Spring Boot, Terraform, AWS), strategic business
                consulting, and rapid development capabilities (Ruby on Rails) to deliver comprehensive
                solutions that don't just fix bugs—they transform fragile codebases into resilient,
                production-ready systems.
              </p>
            </div>
          </div>

          <div class="about-card accent-card">
            <div class="card-gradient"></div>
            <div class="card-content">
              <h3>Our Approach</h3>
              <p>
                Every engagement begins with a comprehensive technical audit. We trace through your
                entire codebase, identify structural weaknesses, document security risks, and map
                out a prioritized remediation plan. Then we execute with precision—refactoring with
                automated test coverage, modernizing deployment pipelines, and establishing code
                quality standards that prevent regression.
              </p>
            </div>
          </div>

          <div class="about-card accent-card-alt">
            <div class="card-gradient"></div>
            <div class="card-content">
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
        </div>
      </div>
    </section>

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
  padding: $spacing-md;
  text-align: center;
  max-width: $width-hero-max;
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
      .break-mobile {
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

.about-section {
  position: relative;
  padding: clamp(8rem, 15vw, 16rem) $spacing-md;
  background: linear-gradient(135deg, #000000 0%, #1a1a1a 50%, #0d0d0d 100%);
  overflow: hidden;

  .about-background {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    opacity: 0.08;
    pointer-events: none;

    .bg-shape {
      position: absolute;
      width: 100%;
      height: 100%;

      polygon {
        fill: url(#gradient);
      }

      &.shape-1 {
        top: -10%;
        left: -20%;
        width: 60%;
        height: 60%;
        opacity: 0.4;
        fill: rgba(255, 255, 255, 0.03);
        transform: rotate(-15deg);
      }

      &.shape-2 {
        top: 20%;
        right: -15%;
        width: 50%;
        height: 50%;
        opacity: 0.3;
        fill: rgba(192, 192, 192, 0.04);
        transform: rotate(25deg);
      }

      &.shape-3 {
        bottom: -20%;
        left: 10%;
        width: 70%;
        height: 40%;
        opacity: 0.25;
        fill: rgba(169, 169, 169, 0.03);
        transform: rotate(-8deg);
      }
    }
  }

  .about-content {
    position: relative;
    z-index: 1;
    max-width: 1400px;
    margin: 0 auto;
  }

  .section-header {
    text-align: center;
    margin-bottom: clamp(6rem, 10vw, 10rem);

    .section-title {
      font-size: clamp(2.5rem, 5vw, 4rem);
      font-weight: 700;
      background: linear-gradient(135deg, #ffffff 0%, #c0c0c0 50%, #a9a9a9 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      margin-bottom: 2rem;
      letter-spacing: -0.02em;
    }

    .title-underline {
      width: 180px;
      height: 2px;
      background: linear-gradient(90deg, transparent, #808080, #c0c0c0, #808080, transparent);
      margin: 0 auto;
    }
  }

  .about-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: clamp(3rem, 5vw, 4rem);

    @media (min-width: 768px) {
      grid-template-columns: repeat(2, 1fr);
    }

    @media (min-width: 1200px) {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  .about-card {
    position: relative;
    padding: clamp(3rem, 5vw, 4rem);
    background: rgba(0, 0, 0, 0.4);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.08);
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    overflow: hidden;

    &.full-width {
      @media (min-width: 768px) {
        grid-column: 1 / -1;
      }
    }

    .card-gradient {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      opacity: 0;
      transition: opacity 0.4s ease;
      pointer-events: none;
    }

    .card-content {
      position: relative;
      z-index: 1;

      h3 {
        font-size: clamp(1.5rem, 3vw, 2rem);
        font-weight: 600;
        margin-bottom: 1.5rem;
        color: #ffffff;
        letter-spacing: -0.01em;
      }

      p {
        font-size: clamp(1rem, 2vw, 1.15rem);
        line-height: 2;
        color: rgba(255, 255, 255, 0.75);
      }

      .team-link {
        color: #d3d3d3;
        text-decoration: none;
        font-weight: 600;
        position: relative;
        transition: color 0.3s ease;

        &::after {
          content: '';
          position: absolute;
          bottom: -2px;
          left: 0;
          width: 100%;
          height: 2px;
          background: linear-gradient(90deg, #808080, #c0c0c0);
          transform: scaleX(0);
          transform-origin: right;
          transition: transform 0.3s ease;
        }

        &:hover {
          color: #ffffff;

          &::after {
            transform: scaleX(1);
            transform-origin: left;
          }
        }
      }
    }

    &:hover {
      transform: translateY(-6px);
      border-color: rgba(255, 255, 255, 0.2);
      box-shadow:
        0 25px 50px rgba(0, 0, 0, 0.5),
        0 0 50px rgba(255, 255, 255, 0.05);

      .card-gradient {
        opacity: 0.06;
      }
    }

    &.primary-card .card-gradient {
      background: linear-gradient(135deg, #2a2a2a 0%, #1a1a1a 100%);
    }

    &.secondary-card .card-gradient {
      background: linear-gradient(135deg, #3a3a3a 0%, #2a2a2a 100%);
    }

    &.tertiary-card .card-gradient {
      background: linear-gradient(135deg, #4a4a4a 0%, #3a3a3a 100%);
    }

    &.accent-card .card-gradient {
      background: linear-gradient(135deg, #5a5a5a 0%, #4a4a4a 100%);
    }

    &.accent-card-alt .card-gradient {
      background: linear-gradient(135deg, #505050 0%, #404040 100%);
    }
  }
}
</style>
