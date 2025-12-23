<script setup>
import { ref } from 'vue'
import PasswordPage from './components/PasswordPage.vue'
import ScrollPage from './components/Scrollpage.vue'
import LetterPage from './components/LetterPage.vue'

const currentPage = ref('password') // password, scroll, letter

const handlePasswordSuccess = () => {
  currentPage.value = 'scroll'
}

const handleScrollOpen = () => {
  currentPage.value = 'letter'
}
</script>

<template>
  <div class="app-container">
    <Transition name="fade" mode="out-in">
      <PasswordPage 
        v-if="currentPage === 'password'" 
        @success="handlePasswordSuccess"
      />
      <ScrollPage 
        v-else-if="currentPage === 'scroll'"
        @open="handleScrollOpen"
      />
      <LetterPage 
        v-else
      />
    </Transition>
  </div>
</template>

<style scoped>
.app-container {
  width: 100%;
  min-height: 100vh;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>