<script setup>
defineProps({
  cardProp: {
    type: Object,
    required: true,
  },
})

const formatDate = (dateStr) => {
  if (!dateStr) return ''
  const date = new Date(dateStr)
  return date.toLocaleDateString(undefined, {
    year: 'numeric',
    month: 'short',
    day: 'numeric',
  })
}

const emit = defineEmits(['edit', 'delete'])
</script>

<template>
  <div class="post-card">
    <div class="actions">
      <button class="btn-delete" @click="$emit('delete', cardProp.id)"><i class="pi pi-times"></i></button>
    </div>
    
    <h2>{{ cardProp.title }}</h2>
    <h6>
      {{ cardProp.location }}
      <br />
      <span class="date">{{ formatDate(cardProp.blog_date) }}</span>
    </h6>
    <span>{{ cardProp.content }}</span>
  </div>
</template>
