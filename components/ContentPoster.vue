<template>
	<div id="ContentPoster" class="content-poster" v-if="havePoster">
		<img :src="image" alt="image">
	</div>
</template>


<script>
export default {
   name: 'ContentPoster',
   props: ['content'],
   data() {
   	return {
   		image: "",
   		havePoster: true
   	}
   },
   mounted() {
   	// checks if the movie have a poster, if not, it will not be rendered
	if(this.content.poster_path == null) {
		this.havePoster = false
	} else {
		this.image = `https://image.tmdb.org/t/p/w342/${this.content.poster_path}`
	}
   }
}
</script>


<style lang="scss" scoped>
	
</style>

