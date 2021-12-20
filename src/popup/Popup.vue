<template>
  <main class="w-[300px] px-4 py-5 text-center text-gray-700">
    <Logo />
    <div>Screen Recording</div>
    <p class="mt-2 opacity-50">
      Ctrl + Alt + R
    </p>
    <button class="btn mt-2 mr-2" @click="openOptionsPage">
      Options
    </button>
    <p class="opacity-30 mt-2 text-center">
      ver 0.0.1
    </p>
  </main>
</template>

<script setup lang="ts">

function openOptionsPage() {
  browser.runtime.openOptionsPage()
}

</script>
