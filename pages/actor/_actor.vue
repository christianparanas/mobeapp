<template>
	<div class="actor">
		<Nav />
		<img :src="actorProfileImg" alt="">
		<div class="actor__details">
			<div class="actor__name shadow">{{ actorDetails.name }}</div>
			<div class="bio">
				<div class="title">Biography</div>
				<div class="content">{{ actorDetails.biography }}</div>
			</div>

			<div class="actor__movies">
				<div class="title">Movies of <span>{{ actorDetails.name }}</span></div>
				<div class="actorMoviesWrapper">
					<ContentPoster v-for="actormovie in actorMovies.cast" :content="actormovie" :key="actormovie.id" />
				</div>
			</div>
		</div>
	</div>
</template>


<script>
	export default {
		transition: 'home',
		async asyncData({ params, $axios }) {

			// get actor credential by params id
			const actorDetails = await $axios.$get(`https://api.themoviedb.org/3/person/${parseInt(params.actor)}?api_key=a807f0095433ac989503323b5b0bc933&language=en-US`)

			const actorMovies = await $axios.$get(`https://api.themoviedb.org/3/person/${parseInt(params.actor)}/movie_credits?api_key=a807f0095433ac989503323b5b0bc933&language=en-US`);
			
			const actorProfileImg = `https://image.tmdb.org/t/p/w342${actorDetails.profile_path}`

			// limit the actorMovies array
      // const topMovies = []
      // for(let i = 0; i < 20; i++) {
      // 	topMovies.push(actorMovies.cast[i])
      // }

      // console.log(topMovies)

			return {
				actorDetails,
				actorProfileImg,
				actorMovies
			}
		},
	}
</script>


<style lang="scss">
	.actor {
		width: 100%;
    	background-color: #091a28;
    	color: #fff;
    	z-index: -1;

		img {
			width: 100%;
		}

		.actor__details {
			padding: 10px 20px;

			.actor__name {
				position: relative;
				top: -30px;
				z-index: 1;
				font-size: 35px;
				background-color: #1f2937;
				width: fit-content;
				padding: 0 10px;
			}

			.bio {

				.title {
					font-size: 18px;
					color: orange;
					margin-bottom: 5px;
				}

				.content {
					font-size: 15px;
				}
			}

			.actor__movies {
				margin-top: 30px;

				.title {
					font-size: 18px;

					span {
						color: orange;
					}
				}
				
				.actorMoviesWrapper {
					margin-top: 10px;
		  		display: grid;
		  		grid-template-columns: repeat(1000, 150px);
		  		grid-column-gap: 8px;
		  		overflow-x: scroll;

		  		&::-webkit-scrollbar {
		    		width: 0px;
		    		background: transparent;
		  		}
				}
			}
		}
	}
		
</style>