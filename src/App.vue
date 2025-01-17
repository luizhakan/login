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

      // reload
      location.reload()
    }, 5000)
  }
}

const togglePasswordVisibility = () => {
  showPassword.value = !showPassword.value
}
</script>

<template>

  <body class="flex justify-center items-center min-h-screen max-[767px]:mx-[2rem]">
    <div
      class="container w-[50rem] bg-[#212226] flex flex-col md:flex-row justify-center items-center rounded py-20 px-4 md:py-20 md:px-8">
      <div class="logos flex flex-col items-center">
        <img
          src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/cd/Facebook_logo_%28square%29.png/800px-Facebook_logo_%28square%29.png"
          class="w-[20rem] h-[17rem] cursor-pointer" />
      </div>

      <div class="or my-4 md:my-0 mx-0 md:mx-[-.7rem] self-start z-50 max-[767px]:self-center">
        <span class="rounded-[100%] bg-[#212226] p-3 text-white font-bold block">or</span>
      </div>

      <div class="form bg-[#535556] content-center p-4 rounded w-full md:w-[30rem] md:h-[17rem]">
        <h1 class="text-white font-bold text-[1.3rem] text-center whitespace-nowrap mb-4">
          Log in with awesome new thing
        </h1>
        <form @submit="handleSubmit" class="flex flex-col items-center md:relative w-full">
          <div class="flex flex-col w-full relative">
            <input type="text" v-model="usernameOrEmail" @input="validateField('usernameOrEmail')"
              @blur="validateField('usernameOrEmail')"
              class="my-2 mb-6 p-2 w-full rounded bg-[#323232] text-white pr-12" placeholder="username or email" />
            <span class="text-red-500 error-message mt-[-1rem]" v-if="errors.usernameOrEmail">{{
              errors.usernameOrEmail
              }}</span>
            <button
              class="login-button absolute top-[30%] right-[-2.5rem] transform -translate-y-1/2 -translate-x-1/4 z-50">
              <svg v-if="!loading" xmlns="http://www.w3.org/2000/svg" class="icon" fill="none" viewBox="0 0 24 24"
                stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3" />
              </svg>
              <svg v-else class="animate-spin h-6 w-6 text-white" xmlns="http://www.w3.org/2000/svg" fill="none"
                viewBox="0 0 24 24" stroke="currentColor">
                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8v8H4z"></path>
              </svg>
            </button>
            <div class="relative">
              <input :type="showPassword ? 'text' : 'password'" v-model="password" @input="validateField('password')"
                @blur="validateField('password')" class="p-2 w-full rounded bg-[#323232] text-white pr-12"
                placeholder="password" />
              <button type="button" @click="togglePasswordVisibility"
                class="absolute right-8 top-1/2 transform -translate-y-1/2 text-white">
                <svg v-if="!showPassword" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                  stroke-width="1.5" stroke="currentColor" class="w-5 h-5">
                  <path stroke-linecap="round" stroke-linejoin="round"
                    d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z" />
                  <path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z" />
                </svg>
                <svg v-else xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                  stroke="currentColor" class="w-5 h-5">
                  <path stroke-linecap="round" stroke-linejoin="round"
                    d="M3.98 8.223A10.477 10.477 0 0 0 1.934 12C3.226 16.338 7.244 19.5 12 19.5c.993 0 1.953-.138 2.863-.395M6.228 6.228A10.451 10.451 0 0 1 12 4.5c4.756 0 8.773 3.162 10.065 7.498a10.522 10.522 0 0 1-4.293 5.774M6.228 6.228 3 3m3.228 3.228 3.65 3.65m7.894 7.894L21 21m-3.228-3.228-3.65-3.65m0 0a3 3 0 1 0-4.243-4.243m4.242 4.242L9.88 9.88" />
                </svg>
              </button>
            </div>
            <span class="text-red-500 error-message" v-if="errors.password">{{
              errors.password
              }}</span>
          </div>
        </form>
        <a href="#" id="forgot-password" class="font-medium block text-center md:text-left mt-6">Forgot your
          password?</a>
      </div>
    </div>
  </body>
</template>

<style scoped>
#forgot-password {
  color: rgb(161, 161, 161);
}

.login-button {
  background-color: #f97316;
  border: 5px solid #323232;
  border-color: #323232 !important;
  border-radius: 50%;
  padding: 0.75rem;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transform: scale(1.5);
}

.icon {
  height: 1.5rem;
  width: 1.5rem;
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

.error-message {
  height: 1.5rem;
  /* Define a fixed height to avoid changing the layout */
}
</style>
