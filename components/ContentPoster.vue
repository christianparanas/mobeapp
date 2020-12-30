<template>
	<div id="ContentPoster" class="content-poster" v-if="havePoster">
		<NuxtLink :to="contentpath">
			<img :src="image" alt="">
		</NuxtLink>
		<div v-if="showSkel" class="skeleton animate-pulse"></div>
	</div>
</template>


<script>
export default {
   name: 'ContentPoster',
   props: ['content'],
   data() {
   	return {
   		contentpath: "",
   		image: "",
   		havePoster: true,
   		showSkel: true
   	}
   },
   mounted() {
   	// checks if the movie have a poster, if not, it will not be rendered
	if(this.content.poster_path == null) {
		this.havePoster = false
	} else {
		this.image = `https://image.tmdb.org/t/p/w342${this.content.poster_path}`
		this.contentpath = `/${this.content.id}`
	}
   },
   watch: {
   	image() {
   		if(!this.image == "") {
   			this.showSkel = false
   		}
   	}
   }
}
</script>


<style lang="scss" scoped>
	.skeleton {
		width: 100%;
		background-color: #1f2937; 
      height: 222px;
	}

	
</style>

