<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";
import { Notyf } from "notyf";
import "notyf/notyf.min.css";

const notyf = new Notyf();

const SITE_KEY = "6LegktwrAAAAAFCtkwS0Kocs4IfnajzSVoivuzoY";

const WEB3FORMS_ACCESS_KEY = "5df2ffe9-dc09-4431-86fe-3eabf1f303af";

const subject = "New message from portfolio Contact Form";

const name = ref("");
const email = ref("");
const message = ref("");

const isLoading = ref(false);

const recaptchaContainer = ref(null);
const recaptchaWidgetId = ref(null);
const recaptchaToken = ref("");

function onRecaptchaSuccess(token) {
  recaptchaToken.value = token;
}

function onRecaptchaExpired() {
  recaptchaToken.value = "";
}

function resetRecaptcha() {
  if (recaptchaWidgetId.value !== null && window.grecaptcha) {
    window.grecaptcha.reset(recaptchaWidgetId.value);
    recaptchaToken.value = "";
  }
}

const submitForm = async () => {
  if (!recaptchaToken.value) {
    notyf.error("Please verify that you are not a robot.");
    return;
  }
  isLoading.value = true;

  try {
    let response = await fetch("https://api.web3forms.com/submit", {
      method: "POST",
      headers: {
        "Content-type": "application/json",
        Accept: "application/json",
      },
      body: JSON.stringify({
        access_key: WEB3FORMS_ACCESS_KEY,
        subject,
        name: name.value,
        email: email.value,
        message: message.value,
        "g-recaptcha-response": recaptchaToken.value,
      }),
    });

    const result = await response.json();

    if (result.success) {
      console.log(result);

      // Reset form fields
      name.value = "";
      email.value = "";
      message.value = "";

      isLoading.value = false;
      notyf.success("Message Sent!");
    } else {
      // Handle failure
      notyf.error(result.message || "Failed to send message");
      isLoading.value = false;
    }
  } catch (e) {
    console.error(e);
    isLoading.value = false;
    notyf.error("Failed to send message");
  } finally {
    resetRecaptcha();
  }
};

// ⭐ RENDER FUNCTION UPDATED TO USE VUE REF AFTER MOUNT
function renderRecaptcha() {
  // Ensure grecaptcha exists and the container is mounted before rendering
  if (!window.grecaptcha || !recaptchaContainer.value) {
    console.error("reCAPTCHA API or container not ready.");
    return;
  }

  // Only render if it hasn't been rendered before
  if (recaptchaWidgetId.value === null) {
    recaptchaWidgetId.value = window.grecaptcha.render(
      recaptchaContainer.value,
      {
        sitekey: SITE_KEY,
        size: "normal",
        callback: onRecaptchaSuccess,
        "expired-callback": onRecaptchaExpired,
      }
    );
  }
}

onMounted(() => {
  // 1. Check if grecaptcha is already loaded (e.g., from a previous component)
  if (window.grecaptcha && window.grecaptcha.render) {
    renderRecaptcha();
  } else {
    // 2. If not loaded, attach the render function to the global window object.
    // The script in index.html will call this function when it finishes loading.
    window.recaptchaRenderCallback = renderRecaptcha;
  }
});

// onBeforeUnmount is no longer necessary as we are not using a continuous interval
</script>

<template>
  <div id="contact" class="container my-5">
    <div class="row">
      <div
        class="col-12 col-md-6 d-none d-md-block d-flex justify-content-center"
      >
        <img src="/images/img3.jpg" class="img-fluid" />
      </div>
      <div
        class="col-12 col-md-6 d-flex justify-content-around"
        id="contact-form"
      >
        <div id="form">
          <h4>Contact Us</h4>
          <form class="text-center">
            <input type="checkbox" name="botcheck" style="display: none" />

            <input
              class="d-block"
              type="text"
              placeholder="fullname"
              id="fullname"
              v-model="name"
              required
            />
            <input
              class="d-block"
              type="email"
              placeholder="email"
              id="email"
              v-model="email"
              required
            />
            <textarea
              placeholder="message"
              id="message"
              rows="3"
              v-model="message"
            ></textarea>
            <button
              class="d-block btn"
              type="button"
              @click="submitForm"
              :disabled="isLoading"
            >
              {{ isLoading ? "Sending..." : "Contact Me" }}
            </button>
          </form>
        </div>
        <div id="contact-div">
          <div class="contact-section">
            <p class="head">Contact</p>
            <p>super@bhenj.com</p>
          </div>
          <div class="contact-section">
            <p class="head">Based in</p>
            <p>San Francisco,</p>
            <p>California</p>
          </div>
          <div
            ref="recaptchaContainer"
            class="mt-3 mb-3 d-flex justify-content-center"
          ></div>
        </div>
      </div>
    </div>
  </div>
</template>
