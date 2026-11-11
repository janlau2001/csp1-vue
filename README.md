# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

Learn more about IDE Support for Vue in the [Vue Docs Scaling up Guide](https://vuejs.org/guide/scaling-up/tooling.html#ide-support).

## Contact form

The contact form submits directly to Web3Forms using the access key in
`src/components/ContactSection.vue`. The key is included in the public
client-side bundle. The form uses the Web3Forms free-plan hCaptcha site key.
In the Web3Forms dashboard, select the form and enable hCaptcha as the
preferred CAPTCHA (Block Spam setting) so submissions are verified server-side.
