<template>
	<div id="CastPoster" class="" v-if="havePoster">
		<img :src="image" alt="">
		<div class="name">{{ content.name }}</div>
	</div>
</template>

<script>
  export default {
	name: 'CastPoster',
    props: ['content'],
    data() {
    	return {
    		havePoster: true,
    		image: ''
    	}
    },
    mounted() {
   	// checks if the movie have a poster, if not, it will not be rendered
			if(this.content.profile_path == null) {
				this.havePoster = false
			} else {
				this.image = `https://image.tmdb.org/t/p/w342/${this.content.profile_path}`
			}
   	}
  }
</script>

<style>
	
</style>