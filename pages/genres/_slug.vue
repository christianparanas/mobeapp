<template>
	<div class="genreMovies">
		<Nav />
		<div class="genreMoviesWrapper">
			<ContentPoster v-for="moviebygenre in allMoviesbyGenre.results" :content="moviebygenre" :key="moviebygenre.id" />
		</div>
	</div>
</template>


<script>
	export default {
		transition: 'home',
		async asyncData({ params, $axios }) {
			const allMoviesbyGenre = await $axios.$get(`https://api.themoviedb.org/3/discover/movie?api_key=a807f0095433ac989503323b5b0bc933&with_genres=${parseInt(params.slug)}`)
			
			return {
				allMoviesbyGenre
			}
		}
	}
	
</script>


<style lang="scss" scoped>
	.home-enter-active, .home-leave-active { transition: opacity .3s; }
  	.home-enter, .home-leave-active { opacity: 0; }

	.genreMovies {
		background-color: #091a28;
    	color: #fff;

    	.genreMoviesWrapper {
    		padding: 100px 20px 20px;
    		display: grid;
    		grid-template-columns: 1fr 1fr 1fr;
    		grid-gap: 8px;
    	}
	}

</style>