# Fix Your Vibes

A Vue 3 application built with the Options API and SASS.

## Setup

1. Install dependencies:
```bash
npm install
```

2. Configure email forwarding (optional):
   - Sign up at [Formspree](https://formspree.io/)
   - Create a new form that forwards to your email
   - Copy the `.env.example` file to `.env`:
   ```bash
   cp .env.example .env
   ```
   - Update `VITE_FORMSPREE_ENDPOINT` with your Formspree form endpoint

   Note: The contact form is currently configured to send emails to ramzidreessen@gmail.com using Formspree's direct email endpoint. For production use, you should create a verified Formspree form.

3. Start the development server:
```bash
npm run dev
```

4. Open your browser to the URL shown in the terminal (typically http://localhost:5173/)

## Build for Production

```bash
npm run build
```

The built files will be in the `dist` directory.

## Preview Production Build

```bash
npm run preview
```

## Project Structure

- `src/App.vue` - Main application component with hero section
- `src/components/ContactForm.vue` - Contact form component
- `src/main.js` - Application entry point
