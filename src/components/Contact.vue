<template>
	<div id="contact">
		<div class="container pt-5">
			<h1 class="text-center mt-5 pt-5 text-light">Contact</h1>
			<div class="row py-5 px-5">
				<div class="col-12 col-md-6 mb-4 mb-md-0 mt-5">
					<div class="ratio ratio-4x3">
						<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d7082.373562420042!2d121.02932773830403!3d14.55962224681984!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3397c9b8d33b2c6d%3A0xb0c2f766338a92bd!2sZuitt%20Makati!5e0!3m2!1sen!2sph!4v1768395315382!5m2!1sen!2sph" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade" class="rounded">
						</iframe>
					</div>
				</div>
				<div class="col-12 col-md-6" id="form-col" @submit.prevent="submitForm">
					<form id="contact_form" class="p-3 rounded text-light" >
						<div class="mb-3">
							<label for="exampleName" class="form-label">Name</label>
							<input type="text" class="form-control" id="exampleName" aria-describedby="nameHelp" v-model="name">
						</div>
						<div class="mb-3">
							<label for="exampleInputEmail1" class="form-label">Email Address</label>
							<input type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" v-model="email">
						</div>
						<div class="mb-3">
							<label for="message" class="form-label">Message</label>
							<textarea class="form-control" id="message" rows="7" v-model="message"></textarea>
						</div>
						<div class="text-center">
							<button type="submit" class="btn btn-warning rounded":disabled="isLoading">{{isLoading ? "Sending..." : "Send"}}</button>
						</div>
						<div class="d-flex justify-content-end mt-2">
							<div ref="recaptchaContainer"></div>
						</div>
					</form>
				</div>
			</div>
		</div>
	</div>
</template>

<script setup>
	import { ref, onMounted, onBeforeUnmount } from 'vue';
	import { Notyf } from 'notyf';
	import 'notyf/notyf.min.css';

	const notyf = new Notyf();

	const WEB3FORMS_ACCESS_KEY = "7f3b3bd7-aae3-4555-813c-30660372707d";

	const subject = "New message from Portfolio Contact Form";

	const name = ref("");
	const email = ref("");
	const message = ref("");
	const isLoading = ref(false);

	const submitForm = async() => {

		// Check if reCAPTCHA token is present, return an error when not verified.
		if (!recaptchaToken.value) {
			notyf.error('Please verify that you are not a robot.');
			return;
		}

		isLoading.value = true;
		
		try {

			const response = await fetch("https://api.web3forms.com/submit", {
				method: "POST",
				headers: {
					"Content-Type": "application/json",
				},
				body: JSON.stringify({
					access_key: WEB3FORMS_ACCESS_KEY,
					subject: subject,
					name: name.value,
					email: email.value,
					message: message.value
				})
			})

			const result = await response.json();

			if(result.success) {
				isLoading.value = false;
				notyf.success("Message Sent!");
			} else {
				notyf.error("Something went wrong.");
			}

		} catch(e) {
			console.log(e);
			isLoading.value = false;
			notyf.error("Failed to send message. Please try again.")
		}
	}

	const SITE_KEY = '6LfNKLwsAAAAAG86ncSN23EwRm5TTealw9QoWFpn';  // Replace with your site key

	const recaptchaContainer = ref(null);
	const recaptchaWidgetId = ref(null);
	const recaptchaToken = ref('');

	// Callback called by reCAPTCHA when successful
	function onRecaptchaSuccess(token) {
		recaptchaToken.value = token;
	}

	// Callback when expired
	function onRecaptchaExpired() {
		recaptchaToken.value = '';
	}

	// Function to render the reCAPTCHA widget
	function renderRecaptcha() {
		if (!window.grecaptcha) {
			console.error('reCAPTCHA not loaded');
			return;
		}

		recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
			sitekey: SITE_KEY,
    		size: 'normal', // or 'compact'
    		callback: onRecaptchaSuccess,
    		'expired-callback': onRecaptchaExpired,
    	});
	}

	// Function to reset reCAPTCHA 
	function resetRecaptcha() {
		if (recaptchaWidgetId.value !== null) {
			window.grecaptcha.reset(recaptchaWidgetId.value);
			recaptchaToken.value = '';
		}
	}

	onMounted(() => {
		  // This code waits for the Google reCAPTCHA library to load, then renders the reCAPTCHA widget using onMounted hook. 
		  // The widget is rendered with grecaptcha.render(), which requires a sitekey. 
		  // Callback functions handle success and expiration events. 
		  // reCAPTCHA is reset upon form submission to clear the token.
		const interval = setInterval(() => {
			if (window.grecaptcha && window.grecaptcha.render) {
				renderRecaptcha();
				clearInterval(interval);
			}
		}, 100);

		onBeforeUnmount(() => {
			clearInterval(interval);
		});
	});
</script>