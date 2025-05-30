<template>
  <div>
    <!-- Your header/navigation could go here -->
    <header v-if="$route.path !== '/'">
      <NuxtLink to="/">← Back to all events</NuxtLink>
    </header>
    
    <!-- This is where your page content will be injected -->
    <NuxtPage />
    
    <!-- Notification system for success/error messages -->
    <div v-if="notification.show" :class="['notification', notification.type]">
      {{ notification.message }}
    </div>
  </div>
</template>

<script setup>
// You can add global state or notifications here if needed
const notification = ref({
  show: false,
  message: '',
  type: '' // 'success' or 'error'
})

// Example function to show notifications
const showNotification = (message, type = 'success') => {
  notification.value = { show: true, message, type }
  setTimeout(() => {
    notification.value.show = false
  }, 3000)
}

// Expose to all components if needed
provide('notify', showNotification)
</script>

<style>
/* Basic global styles */
body {
  font-family: Arial, sans-serif;
  line-height: 1.6;
  margin: 0;
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

header {
  margin-bottom: 20px;
}

/* Notification styles */
.notification {
  position: fixed;
  bottom: 20px;
  right: 20px;
  padding: 15px;
  border-radius: 5px;
  color: white;
  max-width: 300px;
}

.notification.success {
  background-color: #4CAF50;
}

.notification.error {
  background-color: #f44336;
}

/* Link styles */
a {
  color: #2563eb;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
</style>