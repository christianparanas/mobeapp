<template>
	<div id="CastPoster" class="" v-if="havePoster">
		<NuxtLink :to="actorId">
			<img :src="image" alt="">
			<div class="name">{{ content.name }}</div>
		</NuxtLink>
	</div>
</template>

<script>
  export default {
	name: 'CastPoster',
    props: ['content'],
    data() {
    	return {
    		havePoster: true,
    		image: '',
    		actorId: ''
    	}
    },
    mounted() {
   	// checks if the movie have a poster, if not, it will not be rendered
			if(this.content.profile_path == null) {
				this.havePoster = false
			} else {
				this.image = `https://image.tmdb.org/t/p/w342/${this.content.profile_path}`
				this.actorId = `/actor/${this.content.id}`
			}
   	}
  }
</script>

<style>
	
</style>