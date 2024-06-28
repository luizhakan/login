<script setup lang="ts">
import { ref } from 'vue'

// Reactive state for form fields, validation messages, and loading state
const usernameOrEmail = ref('')
const password = ref('')
const errors = ref({
  usernameOrEmail: '',
  password: ''
})
const loading = ref(false)
// ref for password visibility
const showPassword = ref(false)

// Validate the email format
const validateEmail = (email: string) => {
  const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return re.test(email)
}

// Validate the password format
const validatePassword = (password: string) => {
  const re = /^(?=.*[A-Za-z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{10,}$/
  return re.test(password)
}

// Validate the fields on blur
const validateField = (field: string) => {
  if (field === 'usernameOrEmail') {
    if (!usernameOrEmail.value) {
      errors.value.usernameOrEmail = 'Username or email is required'
    } else if (usernameOrEmail.value.length < 6 || usernameOrEmail.value.length > 50) {
      errors.value.usernameOrEmail = 'Username or email must be between 6 and 50 characters'
    } else if (!validateEmail(usernameOrEmail.value) && usernameOrEmail.value.includes('@')) {
      errors.value.usernameOrEmail = 'Email is not valid'
    } else {
      errors.value.usernameOrEmail = ''
    }
  } else if (field === 'password') {
    if (!password.value) {
      errors.value.password = 'Password is required'
    } else if (!validatePassword(password.value)) {
      errors.value.password =
        'Password must be at least 10 characters long and contain at least 1 letter, 1 symbol, and 1 number'
    } else {
      errors.value.password = ''
    }
  }
}

// Handle form submission
const handleSubmit = async (event: Event) => {
  event.preventDefault()
  validateField('usernameOrEmail')
  validateField('password')
  if (!errors.value.usernameOrEmail && !errors.value.password) {
    loading.value = true
    // Simulate an API call
    setTimeout(() => {
      loading.value = false
      console.log('Form submitted', {
        usernameOrEmail: usernameOrEmail.value,
        password: password.value
      })
    }, 5000)
  }
}

const togglePasswordVisibility = () => {
  showPassword.value = !showPassword.value
}
</script>

<template>
  <body class="flex justify-center items-center min-h-screen">
    <div
      class="container bg-[#212226] w-full max-w-lg flex flex-col md:flex-row px-8 md:px-16 justify-center items-center rounded py-8"
    >
      <div class="logos flex flex-row md:flex-col items-center md:items-start">
        <img
          src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/facebook/facebook-original.svg"
          class="w-[4rem] h-[4rem] my-2 mx-2 cursor-pointer"
        />
        <img
          src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/google/google-original.svg"
          class="w-[4rem] h-[4rem] my-2 mx-2 cursor-pointer"
        />
      </div>

      <div class="or my-4 md:my-0 mx-0 md:mx-4">
        <span class="rounded-full bg-black p-4 text-white font-bold block md:inline-block">or</span>
      </div>

      <div class="form bg-[#17232c] p-8 rounded w-full mb-4">
        <h1 class="text-white font-bold text-center md:text-left">Log in with awesome new thing</h1>
        <form
          @submit="handleSubmit"
          class="flex flex-col items-center md:flex-row md:relative w-full"
        >
          <div class="flex flex-col w-full">
            <input
              type="text"
              v-model="usernameOrEmail"
              @input="validateField('usernameOrEmail')"
              @blur="validateField('usernameOrEmail')"
              class="my-2 p-2 w-full rounded bg-[#111a21] text-white"
              placeholder="username or email"
            />
            <span class="text-red-500" v-if="errors.usernameOrEmail">{{
              errors.usernameOrEmail
            }}</span>

            <div class="relative">
              <input
                :type="showPassword ? 'text' : 'password'"
                v-model="password"
                @input="validateField('password')"
                @blur="validateField('password')"
                class="p-2 w-full rounded bg-[#111a21] text-white"
                placeholder="password"
              />
              <button
                type="button"
                @click="togglePasswordVisibility"
                class="absolute right-2 top-1/2 transform -translate-y-1/2 text-white"
              >
                <svg
                  v-if="!showPassword"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="w-5 h-5"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z"
                  />
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z"
                  />
                </svg>
                <svg
                  v-else
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="w-5 h-5"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M3.98 8.223A10.477 10.477 0 0 0 1.934 12C3.226 16.338 7.244 19.5 12 19.5c.993 0 1.953-.138 2.863-.395M6.228 6.228A10.451 10.451 0 0 1 12 4.5c4.756 0 8.773 3.162 10.065 7.498a10.522 10.522 0 0 1-4.293 5.774M6.228 6.228 3 3m3.228 3.228 3.65 3.65m7.894 7.894L21 21m-3.228-3.228-3.65-3.65m0 0a3 3 0 1 0-4.243-4.243m4.242 4.242L9.88 9.88"
                  />
                </svg>
              </button>
            </div>
            <span class="text-red-500" v-if="errors.password">{{ errors.password }}</span>
          </div>

          <button
            class="login-button mt-4 md:mt-0 md:absolute md:right-[-4rem] md:top-1/2 md:transform md:-translate-y-1/2"
          >
            <svg
              v-if="!loading"
              xmlns="http://www.w3.org/2000/svg"
              class="icon"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M9 5l7 7-7 7"
              />
            </svg>
            <svg
              v-else
              class="animate-spin h-6 w-6 text-white"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <circle
                class="opacity-25"
                cx="12"
                cy="12"
                r="10"
                stroke="currentColor"
                stroke-width="4"
              ></circle>
              <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8v8H4z"></path>
            </svg>
          </button>
        </form>
        <a href="#" class="text-[#bec2ca] font-bold block text-center md:text-left mt-2"
          >Forgot your password?</a
        >
      </div>
    </div>
  </body>
</template>

<style scoped>
.login-button {
  background-color: #f97316;
  border: 2px solid black;
  border-radius: 50%;
  padding: 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.icon {
  height: 2rem;
  width: 2rem;
  stroke: white;
}

input:focus {
  outline: 2px solid #f97316;
}

.animate-spin {
  -webkit-animation: spin 1s linear infinite;
  animation: spin 1s linear infinite;
}

@-webkit-keyframes spin {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}

@keyframes spin {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
</style>
