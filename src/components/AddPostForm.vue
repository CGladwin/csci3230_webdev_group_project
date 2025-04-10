<script setup>
import { ref, defineProps, watch, defineEmits } from 'vue'

const props = defineProps({
  post: Object
})

const emit = defineEmits(['submit', 'close'])

const time = ref('')
const content = ref('')
const latitude = ref('')
const longitude = ref('')
const date = ref('')

watch(() => props.post, (post) => {
  if (post) {
    time.value = post.time
    content.value = post.content
    latitude.value = post.latitude || ''  // Set initial value for latitude
    longitude.value = post.longitude || ''  // Set initial value for longitude
    date.value = post.date || ''  // Set initial value for date
  }
}, { immediate: true })

const submitForm = () => {
  emit('submit', {
    id: props.post?.id || Date.now(),
    time: time.value,
    content: content.value,
    latitude: latitude.value,
    longitude: longitude.value,
    date: date.value, 
  })

  time.value = ''
  content.value = ''
  latitude.value = ''
  longitude.value = ''
  date.value = ''  // Clear the form fields

  emit('close')
}
</script>

<template>
  <div class="modal-overlay" @click.self="emit('close')">
    <form class="modal-content" @submit.prevent="submitForm">
      <h2>Add New Post</h2>
      <input v-model="time" type="date" required />
      <textarea v-model="content" placeholder="Notes..." required></textarea>
      <input v-model="latitude" type="number" step="any" placeholder="Latitude" required />
      <input v-model="longitude" type="number" step="any" placeholder="Longitude" required />

      <div class="actions">
        <button type="button" @click="$emit('close')">Cancel</button>
        <button type="submit">Add</button>
      </div>
    </form>
  </div>
</template>
