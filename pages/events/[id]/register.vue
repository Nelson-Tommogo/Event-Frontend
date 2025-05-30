<template>
  <div class="registration-container">
    <div class="registration-header">
      <h1>Register for {{ event.title }}</h1>
      <p class="event-description">{{ event.description }}</p>
      <div class="event-meta">
        <p><strong>Date:</strong> {{ formatDate(event.date) }}</p>
        <p><strong>Time:</strong> {{ formatTime(event.date) }}</p>
        <p><strong>Location:</strong> {{ event.location }}</p>
      </div>
    </div>

    <div class="registration-card">
      <form @submit.prevent="handleSubmit" class="registration-form">
        <div class="form-group">
          <label for="name">Full Name</label>
          <input
            id="name"
            v-model="form.name"
            type="text"
            required
            placeholder="John Doe"
            class="form-input"
          />
        </div>

        <div class="form-group">
          <label for="email">Email Address</label>
          <input
            id="email"
            v-model="form.email"
            type="email"
            required
            placeholder="john@example.com"
            class="form-input"
          />
        </div>

        <div class="form-group">
          <label for="phone">Phone Number</label>
          <input
            id="phone"
            v-model="form.phone"
            type="tel"
            required
            placeholder="+1 (555) 123-4567"
            class="form-input"
          />
        </div>

        <button
          type="submit"
          :disabled="isSubmitting"
          class="submit-button"
          :class="{ 'is-submitting': isSubmitting }"
        >
          <span v-if="!isSubmitting">Complete Registration</span>
          <span v-else class="submitting-text">
            <span class="spinner"></span> Processing...
          </span>
        </button>

        <div v-if="successMessage" class="notification success">
          <span class="icon">✓</span>
          {{ successMessage }}
          <NuxtLink to="/" class="back-link">Back to Events</NuxtLink>
        </div>

        <div v-if="errorMessage" class="notification error">
          <span class="icon">!</span>
          {{ errorMessage }}
          <button @click="resetForm" class="retry-button">Try Again</button>
        </div>
      </form>
    </div>

    <NuxtLink to="/" class="back-link">
      ← Return to all events
    </NuxtLink>
  </div>
</template>

<script setup>
const route = useRoute();
const eventId = route.params.id;

// Mock data - remove when backend is ready
const mockEvent = {
  id: eventId,
  title: eventId === 'event1' ? 'Opening Keynote' : 'Web Development Workshop',
  description: eventId === 'event1' 
    ? 'The future of technology with our special guest speakers' 
    : 'Hands-on workshop covering modern web development',
  date: eventId === 'event1' 
    ? '2023-12-15T09:00:00' 
    : '2023-12-16T10:00:00',
  location: eventId === 'event1' ? 'Main Hall' : 'Room 101'
};

// Use mock data in development
const event = ref(mockEvent);
// Uncomment for real API:
// const { data: event, error: fetchError } = await useFetch(
//   `https://api.example.com/events/${eventId}`
// );

const form = reactive({
  name: '',
  email: '',
  phone: ''
});

const successMessage = ref('');
const errorMessage = ref('');
const isSubmitting = ref(false);

const formatDate = (dateString) => {
  return new Date(dateString).toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  });
};

const formatTime = (dateString) => {
  return new Date(dateString).toLocaleTimeString('en-US', {
    hour: '2-digit',
    minute: '2-digit'
  });
};

const handleSubmit = async () => {
  isSubmitting.value = true;
  successMessage.value = '';
  errorMessage.value = '';
  
  try {
    // Mock submission - remove when backend is ready
    await new Promise(resolve => setTimeout(resolve, 1500));
    successMessage.value = 'Registration successful! You will receive a confirmation email shortly.';
    form.name = '';
    form.email = '';
    form.phone = '';
    
    // Uncomment for real API:
    // const { data, error } = await useFetch(
    //   `https://api.example.com/events/${eventId}/registrations`,
    //   {
    //     method: 'POST',
    //     body: JSON.stringify(form),
    //     headers: {
    //       'Content-Type': 'application/json'
    //     }
    //   }
    // );
    // if (error.value) throw error.value;
    // successMessage.value = 'Registration successful! Thank you.';
    
  } catch (err) {
    errorMessage.value = err.message || 'Registration failed. Please try again later.';
    console.error('Registration error:', err);
  } finally {
    isSubmitting.value = false;
  }
};

const resetForm = () => {
  errorMessage.value = '';
};
</script>

<style scoped>
.registration-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

.registration-header {
  text-align: center;
  margin-bottom: 2rem;
}

.registration-header h1 {
  font-size: 2rem;
  margin-bottom: 0.5rem;
  color: #1e293b;
}

.event-description {
  color: #64748b;
  margin-bottom: 1rem;
}

.event-meta {
  display: flex;
  gap: 1.5rem;
  justify-content: center;
  color: #475569;
  margin-bottom: 2rem;
}

.registration-card {
  background: white;
  border-radius: 0.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  padding: 2rem;
  margin-bottom: 2rem;
}

.registration-form {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

label {
  font-weight: 600;
  color: #334155;
}

.form-input {
  padding: 0.75rem;
  border: 1px solid #e2e8f0;
  border-radius: 0.375rem;
  font-size: 1rem;
  transition: border-color 0.2s;
}

.form-input:focus {
  outline: none;
  border-color: #4f46e5;
  box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.1);
}

.submit-button {
  padding: 1rem;
  background-color: #4f46e5;
  color: white;
  border: none;
  border-radius: 0.375rem;
  font-weight: 600;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.2s;
  margin-top: 1rem;
}

.submit-button:hover {
  background-color: #4338ca;
}

.submit-button:disabled {
  background-color: #cbd5e1;
  cursor: not-allowed;
}

.is-submitting {
  position: relative;
}

.submitting-text {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.spinner {
  width: 1rem;
  height: 1rem;
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-radius: 50%;
  border-top-color: white;
  animation: spin 1s ease-in-out infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

.notification {
  padding: 1rem;
  border-radius: 0.375rem;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin-top: 1.5rem;
}

.notification .icon {
  font-weight: bold;
  margin-right: 0.5rem;
}

.success {
  background-color: #f0fdf4;
  color: #166534;
  border: 1px solid #bbf7d0;
}

.error {
  background-color: #fef2f2;
  color: #b91c1c;
  border: 1px solid #fecaca;
}

.retry-button {
  margin-top: 0.5rem;
  padding: 0.5rem 1rem;
  background-color: #b91c1c;
  color: white;
  border: none;
  border-radius: 0.25rem;
  cursor: pointer;
}

.back-link {
  display: inline-block;
  margin-top: 1rem;
  color: #4f46e5;
  text-decoration: none;
  font-weight: 500;
}

.back-link:hover {
  text-decoration: underline;
}
</style>