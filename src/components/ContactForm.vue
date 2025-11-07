<script>
export default {
  name: 'ContactForm',
  data() {
    return {
      form: {
        name: '',
        email: '',
        message: ''
      },
      isSubmitting: false,
      submitStatus: null
    }
  },
  methods: {
    async handleSubmit() {
      this.isSubmitting = true
      this.submitStatus = null

      try {
        const formspreeEndpoint = import.meta.env.VITE_FORMSPREE_ENDPOINT || 'https://formspree.io/f/myzlggao'
        const response = await fetch(formspreeEndpoint, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            name: this.form.name,
            email: this.form.email,
            message: this.form.message,
            _replyto: this.form.email,
            _subject: 'New contact from Fix Your Vibes'
          })
        })

        if (response.ok) {
          this.submitStatus = 'success'
          this.form = {
            name: '',
            email: '',
            message: ''
          }
        } else {
          this.submitStatus = 'error'
        }
      } catch (error) {
        this.submitStatus = 'error'
      } finally {
        this.isSubmitting = false
      }
    }
  }
}
</script>

<template>
  <section class="contact-section">
    <div class="container">
      <h3>Get in touch</h3>

      <form @submit.prevent="handleSubmit" class="contact-form">
        <div class="form-group">
          <input
            v-model="form.name"
            type="text"
            id="name"
            placeholder="Your name"
            required
          />
        </div>

        <div class="form-group">
          <input
            v-model="form.email"
            type="email"
            id="email"
            placeholder="Your email"
            required
          />
        </div>

        <div class="form-group">
          <textarea
            v-model="form.message"
            id="message"
            rows="5"
            placeholder="Tell us about your project"
            required
          ></textarea>
        </div>

        <button
          type="submit"
          class="submit-btn"
          :disabled="isSubmitting"
        >
          {{ isSubmitting ? 'Sending...' : 'Send Message' }}
        </button>

        <div v-if="submitStatus === 'success'" class="status-message success">
          Thanks! We'll get back to you soon.
        </div>

        <div v-if="submitStatus === 'error'" class="status-message error">
          Something went wrong. Please try again.
        </div>
      </form>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@import '../styles/variables.scss';

.contact-section {
  background: $bg-secondary;
  padding: $spacing-lg $spacing-md;
  border-top: 1px solid $border-primary;
}

.container {
  max-width: $width-container-max;
  margin: 0 auto;

  h3 {
    font-size: 2rem;
    font-weight: 700;
    margin-bottom: $spacing-md;
    text-align: center;
  }
}

.contact-form {
  display: flex;
  flex-direction: column;
  gap: $spacing-sm;
}

.form-group {
  display: flex;
  flex-direction: column;

  input,
  textarea {
    padding: $spacing-xs;
    font-size: 1rem;
    background: $bg-tertiary;
    border: 1px solid $border-secondary;
    border-radius: $border-radius;
    color: $text-primary;
    font-family: inherit;
    transition: $transition-default;

    &:focus {
      outline: none;
      border-color: $border-focus;
      background: $bg-hover;
    }

    &::placeholder {
      color: $text-tertiary;
    }
  }
}

.submit-btn {
  padding: $spacing-xs $spacing-md;
  font-size: 1.1rem;
  font-weight: 600;
  background: $gradient-button;
  color: $text-primary;
  border: none;
  border-radius: $border-radius;
  cursor: pointer;
  transition: $transition-default;
  color: $text-white;

  &:hover:not(:disabled) {
    transform: translateY(-2px);
    box-shadow: 0 10px 20px rgba(100, 100, 100, 0.3);
  }

  &:disabled {
    opacity: 0.6;
    cursor: not-allowed;
  }
}

.status-message {
  padding: $spacing-xs;
  border-radius: $border-radius;
  text-align: center;
  font-weight: 500;

  &.success {
    background: $success-bg;
    color: $success-text;
    border: 1px solid $success-border;
  }

  &.error {
    background: $error-bg;
    color: $error-text;
    border: 1px solid $error-border;
  }
}
</style>
