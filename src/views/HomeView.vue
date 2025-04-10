<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { trips } from '../stores/trip'
import { fetchTrips, createTrip, deleteTrip } from '../app/app'

const router = useRouter()

const showForm = ref(false)
const newTripTitle = ref('')
const currentUser = '1' 

// Load trips for user on mount
const loadTrips = async () => {
  try {
    const data = await fetchTrips(currentUser)
    trips.value = data
  } catch (err) {
    console.error('Error fetching trips:', err)
  }
}

onMounted(loadTrips)

// Add a trip
const addTrip = async () => {
  const title = newTripTitle.value.trim()
  if (!title) return

  try {
    await createTrip(title, currentUser)
    newTripTitle.value = ''
    showForm.value = false
    await loadTrips()
  } catch (err) {
    console.error('Error adding trip:', err)
  }
}


const removeTrip = async (tripId) => {
  try {
    await deleteTrip(tripId)
    trips.value = trips.value.filter(trip => trip.id !== tripId)
  } catch (error) {
    console.error('Error deleting trip:', error)
  }
}

const goToTrip = (tripId) => {
  router.push({ name: 'Trip', params: { id: tripId } })
}
</script>



<template>
  <div class="home">
    
    <h1 style="color:#333">Your Planned Trips</h1>

    <div class="trip-grid">
      <div
        v-for="trip in trips"
        :key="trip.id"
        class="trip-card"
        @click="goToTrip(trip.id)"
      >
        <button class="btn-delete" @click.stop="removeTrip(trip.id)"><i class="pi pi-times"></i></button>
        <h2>{{ trip.title }}</h2>
      </div>
    </div>

    <button class="btn-add-post" @click="showForm = true">Add New Trip</button>

    <div v-if="showForm" class="modal-overlay" @click.self="showForm = false">
      <div class="modal-content">
        <h2>Create a New Trip</h2>
        <input
          v-model="newTripTitle"
          type="text"
          placeholder="Trip Title"
        />
        <div class="modal-actions">
          <button @click="addTrip" class="btn-save">Add</button>
          <button @click="showForm = false" class="btn-cancel">Cancel</button>
        </div>
      </div>
    </div>
  </div>
</template>