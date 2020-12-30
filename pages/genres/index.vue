<template>
	<div class="allgenre">
		<Nav />
		<div class="genre-wrapper">
			<Genre v-for="genre in allgenres.genres" :content="genre" :key="genre.id" />
		</div>
	</div>
</template>


<script>
	export default {
		async asyncData({ $axios }) {
			const allgenres = await $axios.$get(`https://api.themoviedb.org/3/genre/movie/list?api_key=a807f0095433ac989503323b5b0bc933&language=en-US`)

			console.log(allgenres)
			return {
				allgenres
			}
		}
	}
</script>


<style lang="scss" scoped>
	.allgenre {
		background-color: #091a28;
    	color: #fff;

		.genre-wrapper {
			padding: 100px 20px 20px;
			display: grid;
			grid-template-columns: 1fr 1fr 1fr;
			grid-gap: 8px;
		}
	}

</style>