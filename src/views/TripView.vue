<script setup>
import { ref, watch } from 'vue'
import { useRoute, RouterView} from 'vue-router'
import { trips } from '../stores/trip'
import PostCard from '../components/PostCard.vue'

const route = useRoute()
const trip = ref(null)
const tripId = parseInt(route.params.id)


const updateTrip = async () => {
  const tripId = parseInt(route.params.id)
  const selectedTrip = trips.value.find((t) => t.id === tripId)

  if (!selectedTrip) {
    trip.value = null
    return
  }

  try {
    const res = await fetch(`http://localhost:3001/trips/${tripId}/posts`)
    const posts = await res.json()
    trip.value = { ...selectedTrip, posts }
  } catch (error) {
    console.error('Failed to fetch posts:', error)
    trip.value = { ...selectedTrip, posts: [] }
  }
}

watch(() => route.params.id, updateTrip, { immediate: true })

const deletePost = async (postId) => {
  const tripId = parseInt(route.params.id);
  console.log(`Deleting post with ID: ${postId} from trip with ID: ${tripId}`);

  try {
    const response = await fetch(`http://localhost:3001/trips/${tripId}/posts/${postId}`, {
      method: 'DELETE',
    });

    if (response.ok) {
      trip.value.posts = trip.value.posts.filter((post) => post.id !== postId);
    } else {
      const errorText = await response.text();
      console.error('Error deleting post:', errorText);
    }
  } catch (error) {
    console.error('Error deleting post:', error);
  }
};

</script>

<template>
    <div class="trip-view">
      <h1 v-if="trip">{{ trip.title }}</h1>
      <h1 v-else>Trip not found</h1>
  
      <div class="posts-container" v-if="trip">
        <PostCard
          v-for="post in trip.posts"
          :key="post.id"
          :cardProp="post"
          @delete="deletePost"
        />
      </div>

    <button class="btn-add-post">
        <RouterLink :to="{path: '/globe', query: {tripId: tripId}}">Add New Post</RouterLink>
    </button>
    <br>
    <button class="btn-add-post">
        <RouterLink to="/">Back To Trips</RouterLink>
    </button>
    </div>
</template>


