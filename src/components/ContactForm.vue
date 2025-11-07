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
        const formspreeEndpoint = import.meta.env.VITE_FORMSPREE_ENDPOINT || 'https://formspree.io/f/ramzidreessen@gmail.com'
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

<style lang="sass" scoped>
.contact-section
  background: #111
  padding: 4rem 2rem
  border-top: 1px solid #222

.container
  max-width: 600px
  margin: 0 auto

  h3
    font-size: 2rem
    font-weight: 700
    margin-bottom: 2rem
    text-align: center

.contact-form
  display: flex
  flex-direction: column
  gap: 1.5rem

.form-group
  display: flex
  flex-direction: column

  input,
  textarea
    padding: 1rem
    font-size: 1rem
    background: #1a1a1a
    border: 1px solid #333
    border-radius: 8px
    color: #fff
    font-family: inherit
    transition: all 0.3s ease

    &:focus
      outline: none
      border-color: #ff6b6b
      background: #222

    &::placeholder
      color: #666

.submit-btn
  padding: 1rem 2rem
  font-size: 1.1rem
  font-weight: 600
  background: linear-gradient(135deg, #ff6b6b, #ee5a6f)
  color: #fff
  border: none
  border-radius: 8px
  cursor: pointer
  transition: all 0.3s ease

  &:hover:not(:disabled)
    transform: translateY(-2px)
    box-shadow: 0 10px 20px rgba(255, 107, 107, 0.3)

  &:disabled
    opacity: 0.6
    cursor: not-allowed

.status-message
  padding: 1rem
  border-radius: 8px
  text-align: center
  font-weight: 500

  &.success
    background: rgba(72, 187, 120, 0.1)
    color: #68d391
    border: 1px solid rgba(72, 187, 120, 0.3)

  &.error
    background: rgba(245, 101, 101, 0.1)
    color: #fc8181
    border: 1px solid rgba(245, 101, 101, 0.3)
</style>
