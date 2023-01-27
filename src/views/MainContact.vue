<template>
  <div>
    <section class="">
      <h2
        class="lg:text-2xl text-lg font-bold underline underline-offset-8 decoration-2"
      >
        Get in touch
      </h2>
      <br />
      <p class="lg:text-lg text-sm text-justify leading-6">
        Have an idea you want it to become a reality? I can make it happen for
        you! Don't feel hesitated to contact me, I will be responding to your
        messages as soon as possible.
      </p>
    </section>
    <section class="pt-5">
      <p class="lg:text-lg text-sm text-justify leading-6">
        You can reach me by emailing me at
        <strong>s.simonovski@hotmail.com.com</strong> or fill in the contact
        form bellow.
      </p>
    </section>
    <br />
    <section class="">
      <p class="lg:text-lg text-sm text-justify leading-6">
        You can visit my
        <strong class="underline"
          ><router-link to="/work">portfolio</router-link></strong
        >
        to view my works. Alternatively, you can visit my
        <strong class="underline"
          ><a href="https://github.com/stefansimonovski" target="_blank"
            >Github Profile</a
          ></strong
        >
        to view my projects posted on Github
      </p>
    </section>
    <section class="pt-10">
      <div class="">
        <h2
          class="lg:text-2xl text-lg font-bold underline underline-offset-8 decoration-2"
        >
          Send me a message!
        </h2>
      </div>
      <div class="divCenter pt-10">
        <form class="w-full max-w-lg">
          <div
            v-if="!loading && (!showPostSuccess || !failedPost)"
            class="flex flex-wrap -mx-3 mb-6"
          >
            <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
              <label
                class="block uppercase tracking-wide text-xs font-bold mb-2"
                for="grid-first-name"
              >
                First Name
              </label>
              <input
                class="appearance-none block w-full border rounded py-3 px-4 mb-3 leading-tight focus:outline-none"
                v-model="firstName"
                type="text"
                placeholder="Jane"
              />
              <p class="text-red-500 text-xs italic" v-if="showNoFirstName">
                Please fill out this field.
              </p>
            </div>
            <div class="w-full md:w-1/2 px-3">
              <label
                class="block uppercase tracking-wide text-xs font-bold mb-2"
                for="grid-last-name"
              >
                Last Name
              </label>
              <input
                class="appearance-none block w-full border rounded py-3 px-4 mb-3 leading-tight focus:outline-none"
                v-model="lastName"
                type="text"
                placeholder="Doe"
              />
              <p class="text-red-500 text-xs italic" v-if="showNoLastName">
                Please fill out this field.
              </p>
            </div>
          </div>
          <div
            v-if="!loading && (!showPostSuccess || !failedPost)"
            class="flex flex-wrap -mx-3 mb-6"
          >
            <div class="w-full px-3">
              <label
                class="block uppercase tracking-wide text-xs font-bold mb-2"
                for="grid-password"
              >
                E-mail
              </label>
              <input
                class="appearance-none block w-full border rounded py-3 px-4 mb-3 leading-tight focus:outline-none"
                v-model="userEmail"
                type="email"
              />
              <p class="text-red-500 text-xs italic" v-if="showNoEmail">
                Please fill out this field.
              </p>
              <p class="text-red-500 text-xs italic" v-if="showInvalidEmail">
                Email entered is invalid
              </p>
            </div>
          </div>
          <div
            v-if="!loading && (!showPostSuccess || !failedPost)"
            class="flex flex-wrap -mx-3 mb-6"
          >
            <div class="w-full px-3">
              <label
                class="block uppercase tracking-wide text-xs font-bold mb-2"
                for="grid-password"
              >
                Message
              </label>
              <textarea
                class="no-resize block w-full appearance-none border rounded py-3 px-4 mb-3 leading-tight focus:outline-none h-48 resize-none"
                placeholder="Enter your message here"
                v-model="userText"
              />
              <p class="text-red-500 text-xs italic" v-if="showNoMessage">
                Please fill out this field.
              </p>
            </div>
          </div>
          <div class="md:flex md:items-center">
            <div
              v-if="!loading && (!showPostSuccess || !failedPost)"
              class="md:w-2/3"
            >
              <button
                class="shadow focus:shadow-outline focus:outline-none font-bold py-4 px-10 rounded"
                @click.prevent="submitHandler($event)"
              >
                Submit
              </button>
            </div>
            <div class="w-full flex justify-center">
              <p class="text-red-500 text-xs italic" v-if="failedPost">
                An error has occurred, please try again later.
              </p>
              <p class="success text-md" v-if="showPostSuccess">
                Your message has been received! <br />
                Thank you for reaching out to me.
              </p>
            </div>
          </div>
        </form>
      </div>
    </section>
    <section v-if="loading">
      <div class="pt-4 div-center w-full overflow-hidden animate-pulse">
        <div class="w-64">
          <p class="text-2xl mt-2 ml-5">Sending message...</p>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import { ref } from 'vue'
import emailjs from '@emailjs/browser'
import { useRouter } from 'vue-router'

export default {
  setup() {
    const router = useRouter()

    const failedPost = ref(false)

    const firstName = ref('')
    const lastName = ref('')
    const userEmail = ref('')
    const userText = ref('')

    const showNoFirstName = ref(false)
    const showNoLastName = ref(false)
    const showNoEmail = ref(false)
    const showNoMessage = ref(false)
    const showInvalidEmail = ref(false)
    const showPostSuccess = ref(false)
    const loading = ref(false)

    const submitHandler = e => {
      loading.value = true
      showNoFirstName.value = false
      showNoLastName.value = false
      showNoEmail.value = false
      showNoMessage.value = false
      showInvalidEmail.value = false
      showPostSuccess.value = false
      failedPost.value = false

      if (!firstName.value) showNoFirstName.value = true
      if (!lastName.value) showNoLastName.value = true
      if (!userEmail.value) showNoEmail.value = true
      if (!userText.value) showNoMessage.value = true

      if (
        showNoMessage.value ||
        showNoEmail.value ||
        showNoFirstName.value ||
        showNoLastName.value
      )
        return

      const emailValidator = /^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/g
      const validEmail = userEmail.value.match(emailValidator)
      if (!validEmail) {
        showInvalidEmail.value = true
        return
      }

      const payload = {
        firstName: firstName.value,
        lastName: lastName.value,
        userEmail: userEmail.value,
        userMessage: userText.value
      }
      emailjs
        .send(
          import.meta.env.VITE_SERVICE_ID,
          import.meta.env.VITE_TEMPLATE_ID,
          payload,
          import.meta.env.VITE_PUBLIC_KEY
        )
        .then(
          () => {
            showPostSuccess.value = true
            firstName.value = ''
            lastName.value = ''
            userEmail.value = ''
            userText.value = ''
            loading.value = false
            setTimeout(() => {
              showPostSuccess.value = false
            }, 10000)
          },
          err => {
            loading.value = false
            failedPost.value = true
            setTimeout(() => {
              failedPost.value = false
            }, 10000)
            console.error('FAILED...', err)
          }
        )
    }

    return {
      submitHandler,
      firstName,
      lastName,
      userEmail,
      userText,
      showNoFirstName,
      showNoLastName,
      showNoEmail,
      showNoMessage,
      showInvalidEmail,
      showPostSuccess,
      failedPost,
      loading
    }
  }
}
</script>

<style scoped>
hr {
  border-color: var(--text);
}

input {
  background-color: var(--bg-sel);
  border-color: var(--text-highlight);
}

input:focus {
  border-color: var(--text-highlight-2);
}

textarea {
  background-color: var(--bg-sel);
  border-color: var(--text-highlight);
}

textarea:focus {
  border-color: var(--text-highlight-2);
}

button {
  background-color: var(--bg-sel);
}

.success {
  color: var(--text-highlight);
}
</style>
