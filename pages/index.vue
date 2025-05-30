<template>
  <div class="events-container">
    <header class="events-header">
      <h1>Events for {{ organization.name }}</h1>
      <div v-if="orgError" class="error-message">
        Organization details error: {{ orgError.message }}
      </div>
    </header>
    
    <div v-if="loading" class="loading-container">
      <div class="loading-spinner"></div>
      <p>Loading events...</p>
    </div>
    
    <div v-else-if="error" class="error-message">
      {{ error.message || 'Failed to load events' }}
      <button @click="retryLoading" class="retry-button">Retry</button>
    </div>
    
    <div v-else>
      <div v-if="events.length === 0" class="no-events">
        <p>No upcoming events found.</p>
      </div>
      
      <div v-else class="events-grid">
        <div v-for="event in events" :key="event.id" class="event-card">
          <h2>{{ event.title }}</h2>
          <p class="event-description">{{ event.description }}</p>
          <div class="event-details">
            <p><strong>Date:</strong> {{ formatDate(event.date) }}</p>
            <p><strong>Time:</strong> {{ formatTime(event.date) }}</p>
            <p><strong>Location:</strong> {{ event.location }}</p>
          </div>
          <NuxtLink 
            :to="`/events/${event.id}/register`" 
            class="register-link"
            :aria-label="`Register for ${event.title}`"
          >
            <button class="register-button">
              Register
              <span class="icon">→</span>
            </button>
          </NuxtLink>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// Mock data - remove this when backend is ready
const mockOrganization = {
  id: 'org123',
  name: 'Tech Conference 2023'
};

const mockEvents = [
  {
    id: 'event1',
    title: 'Opening Keynote',
    description: 'The future of technology with our special guest speakers discussing emerging trends and innovations.',
    date: '2023-12-15T09:00:00',
    location: 'Main Hall',
    image: '/images/event1.jpg'
  },
  {
    id: 'event1',
    title: 'Opening Keynote',
    description: 'The future of technology with our special guest speakers discussing emerging trends and innovations.',
    date: '2023-12-15T09:00:00',
    location: 'Main Hall',
    image: '/images/event1.jpg'
  },
  {
    id: 'event1',
    title: 'Opening Keynote',
    description: 'The future of technology with our special guest speakers discussing emerging trends and innovations.',
    date: '2023-12-15T09:00:00',
    location: 'Main Hall',
    image: '/images/event1.jpg'
  },
  {
    id: 'event2',
    title: 'Web Development Workshop',
    description: 'Hands-on workshop covering modern web development with Nuxt.js, Vue 3, and best practices.',
    date: '2023-12-16T10:00:00',
    location: 'Room 101',
    image: '/images/event2.jpg'
  }
];

// State management
const organization = ref(mockOrganization);
const events = ref(mockEvents);
const loading = ref(false);
const error = ref(null);
const orgError = ref(null);

// Real API call (commented out for now)
/*
const organizationId = 'org123';
const fetchData = async () => {
  loading.value = true;
  error.value = null;
  try {
    const orgResponse = await useFetch(
      `https://api.example.com/organizations/${organizationId}`
    );
    organization.value = orgResponse.data.value;
    orgError.value = orgResponse.error.value;
    
    const eventsResponse = await useFetch(
      `https://api.example.com/organizations/${organizationId}/events`
    );
    events.value = eventsResponse.data.value;
    error.value = eventsResponse.error.value;
  } catch (err) {
    error.value = err;
  } finally {
    loading.value = false;
  }
};

onMounted(fetchData);
*/

const retryLoading = () => {
  // fetchData(); // Uncomment when using real API
  events.value = mockEvents; // Remove when using real API
  error.value = null;
};

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
</script>

<style scoped>
.events-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.events-header {
  margin-bottom: 2rem;
  text-align: center;
}

.loading-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  padding: 2rem;
}

.loading-spinner {
  width: 50px;
  height: 50px;
  border: 5px solid #f3f3f3;
  border-top: 5px solid #4f46e5;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.error-message {
  padding: 1rem;
  background-color: #fee2e2;
  color: #b91c1c;
  border-radius: 0.5rem;
  margin-bottom: 1rem;
  text-align: center;
}

.retry-button {
  margin-top: 0.5rem;
  background-color: #b91c1c;
  color: white;
}

.no-events {
  text-align: center;
  padding: 2rem;
  font-size: 1.2rem;
  color: #64748b;
}

.events-grid {
  display: grid;
  gap: 1.5rem;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
}

.event-card {
  border: 1px solid #e2e8f0;
  border-radius: 0.5rem;
  padding: 1.5rem;
  transition: transform 0.2s, box-shadow 0.2s;
}

.event-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

.event-card h2 {
  margin-top: 0;
  color: #1e293b;
}

.event-description {
  color: #475569;
  margin-bottom: 1rem;
}

.event-details {
  margin-bottom: 1.5rem;
}

.event-details p {
  margin: 0.5rem 0;
  color: #64748b;
}

.register-link {
  display: inline-block;
  text-decoration: none;
}

.register-button {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background: #4f46e5;
  color: white;
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 0.5rem;
  cursor: pointer;
  font-weight: 600;
  transition: background-color 0.2s;
}

.register-button:hover {
  background-color: #4338ca;
}

.icon {
  font-size: 1.2rem;
}
</style>