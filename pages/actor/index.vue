<template>
	<div class="actors">
		<Nav />
		<div class="actors_wrapper">
			<CastPoster class="actorPos" v-for="actor in actors" :content="actor" :key="actor.id" />
		</div>
	</div>
</template>


<script>
	export default {
		async asyncData({ $axios }) {
			// declare array of all actors
			let actors = []

			const popularActors1 = await $axios.$get(`https://api.themoviedb.org/3/person/popular?api_key=a807f0095433ac989503323b5b0bc933&language=en-US&page=1`)
			const popularActors2 = await $axios.$get(`https://api.themoviedb.org/3/person/popular?api_key=a807f0095433ac989503323b5b0bc933&language=en-US&page=2`)
			const popularActors3 = await $axios.$get(`https://api.themoviedb.org/3/person/popular?api_key=a807f0095433ac989503323b5b0bc933&language=en-US&page=3`)

			// set the first page array actor to actors array
			actors = popularActors1.results

			// push the 2nd page array into actors array and the same with 3rd page array
			popularActors2.results.map(function(value, key) {
     			actors.push(value);
   			});

   			popularActors3.results.map(function(value, key) {
     			actors.push(value);
   			});

			return {
				actors
			}
		},
		methods: {
			dynaImg(poster_path) {
        		return `https://image.tmdb.org/t/p/w342${poster_path}`
    		},
		}
	}
</script>


<style lang="scss">
	.actors {
		background-color: #091a28;
    	color: #fff;

		.actors_wrapper {
			padding: 100px 20px 20px;
			display: grid;
			grid-template-columns: 1fr 1fr 1fr;
			grid-gap: 8px;

		}
	}
		
</style>