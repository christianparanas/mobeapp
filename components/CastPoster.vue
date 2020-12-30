<template>
	<div id="CastPoster" class="" v-if="havePoster">
		<NuxtLink :to="actorId">
			<img :src="image" alt="">
		</NuxtLink>
		<div v-if="showSkel" class="skeleton animate-pulse"></div>
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
    		actorId: '',
    		showSkel: true
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

<style>
	.skeleton {
		width: 100%;
		background-color: #1f2937; 
    height: 222px;
	}
	
</style>