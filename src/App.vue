<script setup lang="ts"></script>

<template>
  <body class="flex justify-center items-center min-h-screen">
    <div class="container bg-[#212226] w-full max-w-lg flex flex-col md:flex-row px-8 md:px-16 justify-center items-center rounded py-8">
      <div class="logos flex flex-row md:flex-col items-center md:items-start">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/facebook/facebook-original.svg" class="w-[4rem] h-[4rem] my-2 mx-2 cursor-pointer" />
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/google/google-original.svg" class="w-[4rem] h-[4rem] my-2 mx-2 cursor-pointer" />
      </div>

      <div class="or my-4 md:my-0 mx-0 md:mx-4">
        <span class="rounded-full bg-black p-4 text-white font-bold block md:inline-block">or</span>
      </div>

      <div class="form bg-[#17232c] p-8 rounded w-full mb-4">
        <h1 class="text-white font-bold text-center md:text-left">Log in with awesome new thing</h1>
        <form class="flex flex-col items-center md:flex-row md:relative w-full">
          <div class="flex flex-col w-full">
            <input type="text" class="my-2 p-2 w-full rounded bg-[#111a21] text-white" placeholder="username or email" />
            <input type="password" class="p-2 w-full rounded bg-[#111a21] text-white" placeholder="password" />
          </div>

          <button class="login-button mt-4 md:mt-0 md:absolute md:right-[-4rem] md:top-1/2 md:transform md:-translate-y-1/2">
            <svg xmlns="http://www.w3.org/2000/svg" class="icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
            </svg>
          </button>
        </form>
        <a href="#" class="text-[#bec2ca] font-bold block text-center md:text-left mt-2">Forgot your password?</a>
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
</style>
