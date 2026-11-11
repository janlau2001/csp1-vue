<script setup>
import { onBeforeUnmount, onMounted, ref } from 'vue'
import { Notyf } from 'notyf'
import 'notyf/notyf.min.css'

const emailAddress = 'janlaurenceolarte070101@gmail.com'
const WEB3FORMS_ACCESS_KEY = 'c3f83bc0-f765-4b3b-bb3e-9666a50c7dc1'
const HCAPTCHA_SITE_KEY = '50b2fe65-b00b-4b9e-ad62-3ba471098be2'
const name = ref('')
const email = ref('')
const message = ref('')
const copyStatus = ref('')
const isLoading = ref(false)
const submitError = ref('')
const captchaContainer = ref(null)
const captchaWidgetId = ref(null)
const captchaToken = ref('')
const captchaError = ref('')
let notyf
let captchaTimeout

function showCaptchaError(error) {
	clearTimeout(captchaTimeout)
	captchaToken.value = ''
	captchaError.value = 'Could not load or verify hCaptcha. Please reload the page and try again.'
	console.error('Could not load or verify hCaptcha:', error)
}

function renderCaptcha() {
	if (captchaWidgetId.value !== null) return
	if (!window.hcaptcha || !window.hcaptcha.render) {
		showCaptchaError('hCaptcha API is unavailable.')
		return
	}

	try {
		captchaWidgetId.value = window.hcaptcha.render(captchaContainer.value, {
			sitekey: HCAPTCHA_SITE_KEY,
			size: 'compact',
			callback: token => {
				captchaToken.value = token
				captchaError.value = ''
			},
			'expired-callback': () => { captchaToken.value = '' },
			'error-callback': showCaptchaError,
		})
		captchaError.value = ''
		clearTimeout(captchaTimeout)
	} catch (error) {
		showCaptchaError(error)
	}
}

function resetCaptcha() {
	captchaToken.value = ''
	if (captchaWidgetId.value !== null && window.hcaptcha) {
		window.hcaptcha.reset(captchaWidgetId.value)
	}
}

onMounted(() => {
	notyf = new Notyf({ position: { x: 'left', y: 'bottom' }, dismissible: true })
	const script = document.getElementById('hcaptcha-script')
	window.addEventListener('hcaptcha-ready', renderCaptcha)
	if (window.hcaptchaReady) {
		renderCaptcha()
	} else if (script) {
		script.addEventListener('error', showCaptchaError)
		captchaTimeout = setTimeout(() => showCaptchaError('Loading timed out.'), 15000)
	} else {
		showCaptchaError('hCaptcha script is missing.')
	}
})

onBeforeUnmount(() => {
	clearTimeout(captchaTimeout)
	window.removeEventListener('hcaptcha-ready', renderCaptcha)
	const script = document.getElementById('hcaptcha-script')
	if (script) {
		script.removeEventListener('error', showCaptchaError)
	}
})

async function submitForm() {
	if (isLoading.value) return

	submitError.value = ''
	if (!captchaToken.value) {
		notyf.error('Please verify that you are not a robot.')
		return
	}

	isLoading.value = true
	try {
		const response = await fetch('https://api.web3forms.com/submit', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json',
				Accept: 'application/json',
			},
			body: JSON.stringify({
				access_key: WEB3FORMS_ACCESS_KEY,
				subject: `Portfolio message from ${name.value}`,
				name: name.value,
				email: email.value,
				message: message.value,
				'h-captcha-response': captchaToken.value,
			}),
		})
		const result = await response.json()
		if (!response.ok || result.success !== true) {
			throw new Error(result.message || `Web3Forms request failed (${response.status})`)
		}

		notyf.success('Message sent!')
		name.value = ''
		email.value = ''
		message.value = ''
	} catch (error) {
		console.error('Could not send contact message:', error)
		submitError.value = 'Failed to send your message. Please try again or use the email address below.'
	} finally {
		isLoading.value = false
		resetCaptcha()
	}
}

async function copyAddress() {
	try {
		await navigator.clipboard.writeText(emailAddress)
		copyStatus.value = 'Email address copied.'
	} catch (error) {
		console.error('Could not copy email address:', error)
		copyStatus.value = 'Could not copy the address. Please copy it manually.'
	}
}
</script>

<template>
	<section id="contact" class="py-5">
		<div class="container text-center my-4">
			<span class="badge-pill-nav">CONTACT ME</span>
		</div>

		<div class="container pb-5">
			<div class="portfolio-card">
				<div class="row g-4 align-items-stretch">

					<!-- Left Column: Profile Card -->
					<div class="col-lg-4">
						<div class="contact-profile-card d-flex flex-column align-items-center text-center justify-content-between h-100">
							<div class="w-100">
								<div class="avatar-container mx-auto mb-3">
									<img src="/images/MyPersona.png" alt="Jan Laurence Olarte" class="avatar-img">
								</div>

								<h3 class="contact-name mb-2">Jan Laurence Olarte</h3>
								<p class="contact-bio mx-auto text-center mb-4">
									Open to work with website development, mobile app development, and freelancing. I can also give tutorials, demonstrations, and consultations regarding tech stacks and skills. Just reach out!
								</p>
							</div>

							<div class="w-100">
								<hr class="contact-divider mb-4">
								<span class="direct-channels-title d-block mb-3">DIRECT CHANNELS</span>
								<div class="d-flex justify-content-center gap-2">
									<a href="https://www.linkedin.com/in/jan-laurence-olarte/" class="social-icon-btn" target="_blank" rel="noopener noreferrer" aria-label="LinkedIn"><i class="bi bi-linkedin"></i></a>
									<a href="https://github.com/janlau2001" class="social-icon-btn" target="_blank" rel="noopener noreferrer" aria-label="GitHub"><i class="bi bi-github"></i></a>
									<a href="https://www.facebook.com/janlaurence.olarte" class="social-icon-btn" target="_blank" rel="noopener noreferrer" aria-label="Facebook"><i class="bi bi-facebook"></i></a>
									<a :href="`mailto:${emailAddress}`" class="social-icon-btn" aria-label="Email"><i class="bi bi-envelope-fill"></i></a>
								</div>
							</div>
						</div>
					</div>

					<!-- Right Column: Form & Direct Contact -->
					<div class="col-lg-8 ps-lg-4">
						<form @submit.prevent="submitForm">
							<div class="mb-3">
								<label for="contact-name" class="form-label-custom">NAME</label>
								<input id="contact-name" v-model="name" type="text" class="form-control form-control-custom" placeholder="e.g. John Doe" :disabled="isLoading" required>
							</div>

							<div class="mb-3">
								<label for="contact-email" class="form-label-custom">DETAILS / EMAIL</label>
								<input id="contact-email" v-model="email" type="email" class="form-control form-control-custom" placeholder="e.g. johndoe@sample.com" :disabled="isLoading" required>
							</div>

							<div class="mb-4">
								<label for="contact-message" class="form-label-custom">MESSAGE</label>
								<textarea id="contact-message" v-model="message" class="form-control form-control-custom" rows="4" placeholder="Message me here anytime, and anywhere" :disabled="isLoading" required></textarea>
							</div>

							<div class="mb-4 captcha-wrapper">
								<div ref="captchaContainer"></div>
								<p v-if="captchaError" class="response-time-text mt-2" role="alert">{{ captchaError }}</p>
							</div>

							<div class="d-flex justify-content-between align-items-center flex-wrap gap-2 mb-4">
								<button type="submit" class="btn btn-white-pill px-4 py-2" :disabled="isLoading">{{ isLoading ? 'SENDING...' : 'SEND MESSAGE' }}</button>
								<span class="response-time-text">Typical response: &lt; 24h</span>
							</div>
						</form>
						<p v-if="submitError" class="response-time-text" role="alert">{{ submitError }}</p>

						<div class="divider-container my-4 text-center">
							<span class="divider-text">OR REACH OUT DIRECTLY</span>
						</div>

						<div class="direct-email-box d-flex justify-content-between align-items-center flex-wrap gap-2">
							<div class="email-address-text">
								<span class="email-prefix">EMAIL:</span> {{ emailAddress }}
							</div>
							<button type="button" class="btn btn-copy-address" @click="copyAddress">
								COPY ADDRESS
							</button>
						</div>
						<p v-if="copyStatus" class="response-time-text mt-2" role="status">{{ copyStatus }}</p>
					</div>

				</div>
			</div>
		</div>
	</section>
</template>
