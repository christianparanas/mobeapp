<template>
  <div class="slug">
  	<div class="slug-head">
  		<img :src="contentBdImg" alt="">
    	<div class="movie_detail">
    		<h1 class="movie_title">{{ contentData.title }}</h1>
    		<div class="genres-wrapper">
    			<div class="genres" v-for="genre in genres" :key="genre.id">
    			{{ genre.name }}
    			</div>
    		</div>
    	</div>
  	</div>

    <NuxtLink to="/">
    	<p>Go back</p>
    </NuxtLink>
  </div>
</template>
<script>
  export default {
  	// getting the params id and use it to fetch specific data from tmdb
    async asyncData({ params, $axios }) {

      const contentData = await $axios.$get(`https://api.themoviedb.org/3/movie/${parseInt(params.slug)}?api_key=a807f0095433ac989503323b5b0bc933&language=en-US`)

      const genres = contentData.genres
      const contentBdImg = `https://image.tmdb.org/t/p/w342/${contentData.backdrop_path}`

      return { 
      	contentData,
      	contentBdImg,
      	genres
      }
    },
  }
</script>

<style lang="scss">
	.slug {

		.slug-head {

			img {
				width: 100%;
			}

			.movie_detail {
				padding: 10px 20px;

				.movie_title {
					font-size: 25px;
					background: rgba(0, 0, 0, 0.2);
					width: fit-content;
					padding: 5px 10px;
					position: relative;
					top: -30px;
				}


				.genres-wrapper {
					display: flex;
					flex-direction: row;
					padding-left: 15px;
					position: relative;
					top: -20px;

					.genres {
						margin-right: 10px;
						color: orange;
						font-size: 12px;
					}
				}
			}
		}
	}
	

</style>>