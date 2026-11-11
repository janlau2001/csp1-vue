<script setup>
import { ref } from 'vue'

const emailAddress = 'janlaurenceolarte070101@gmail.com'
const name = ref('')
const email = ref('')
const message = ref('')
const copyStatus = ref('')

function composeMessage() {
	const subject = encodeURIComponent(`Portfolio message from ${name.value}`)
	const body = encodeURIComponent(`Name: ${name.value}\nEmail: ${email.value}\n\n${message.value}`)
	window.location.href = `mailto:${emailAddress}?subject=${subject}&body=${body}`
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
						<form @submit.prevent="composeMessage">
							<div class="mb-3">
								<label for="contact-name" class="form-label-custom">NAME</label>
								<input id="contact-name" v-model="name" type="text" class="form-control form-control-custom" placeholder="e.g. John Doe" required>
							</div>

							<div class="mb-3">
								<label for="contact-email" class="form-label-custom">DETAILS / EMAIL</label>
								<input id="contact-email" v-model="email" type="email" class="form-control form-control-custom" placeholder="e.g. johndoe@sample.com" required>
							</div>

							<div class="mb-4">
								<label for="contact-message" class="form-label-custom">MESSAGE</label>
								<textarea id="contact-message" v-model="message" class="form-control form-control-custom" rows="4" placeholder="Message me here anytime, and anywhere" required></textarea>
							</div>

							<div class="d-flex justify-content-between align-items-center flex-wrap gap-2 mb-4">
								<button type="submit" class="btn btn-white-pill px-4 py-2">SEND MESSAGE</button>
								<span class="response-time-text">Typical response: &lt; 24h</span>
							</div>
						</form>
						<p class="response-time-text">Sending opens your email app so you can review and send your message.</p>

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
