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
    		<div class="overview">{{ contentData.overview }}</div>
    		<div class="mini-infos">
    			<div class="movie__release-date">
    				<div class="">Release Date</div><div class="">{{ contentData.release_date }}</div>
    			</div>
    			<div class="movie__release-date">
    				<div class="">Status</div><div class="">{{ contentData.status }}</div>
    			</div>
    			<div class="movie__release-date">
    				<div class="">Rating</div><div class="">{{ contentData.vote_average }}</div>
    			</div>
    			<div class="movie__release-date">
    				<div class="">Runtime</div><div class="">{{ contentData.runtime }}min</div>
    			</div>
    		</div>
    		<div class="trailer">Trailer</div>
    		<iframe width="100%" height="280"
					:src="trailerVideo">
				</iframe>
    		<div class="casts">
    			<div class="title">
    				<div class="">Cast</div>
    				<div class="movie__see_all_cast">See All</div></div>
    			<div class="castPosterWrapper">
    				<CastPoster v-for="cast in topCast" :content="cast" :key="cast.cast_id" />
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
      const casts = await $axios.$get(`https://api.themoviedb.org/3/movie/${parseInt(params.slug)}/credits?api_key=a807f0095433ac989503323b5b0bc933&language=en-US`)

      const movieTrailer = await $axios.$get(`https://api.themoviedb.org/3/movie/${parseInt(params.slug)}/videos?api_key=a807f0095433ac989503323b5b0bc933&language=en-US`)


      // filter the array, limit items 10
      let i = 0
      const topCast = []
      for(i; i < 10; i++) {
      	topCast.push(casts.cast[i])
      }

      const trailer = []
      for(i = 0; i < 1; i++) {
      	trailer.push(movieTrailer.results[i])
      }

      function getId(url) {
    		var regExp = /^.*(youtu.be\/|v\/|u\/\w\/|embed\/|watch\?v=|\&v=)([^#\&\?]*).*/;
    		var match = url.match(regExp);

    		if (match && match[2].length == 11) {
        	return match[2];
    		} else {
        	return 'error';
    		}
			}

      // convert youtube video into youtube embed video by calling the getId function
      const videoId = getId(`http://www.youtube.com/watch?v=${trailer[0].key}`);
      const trailerVideo = `https://www.youtube.com/embed/${videoId}`

      return { 
      	contentData,
      	contentBdImg,
      	genres,
      	topCast,
      	trailerVideo
      }
    },
  }
</script>

<style lang="scss">
	.slug {
		width: 100%;
    background-color: #091a28;
    color: #fff;

		.slug-head {

			img {
				width: 100%;
			}

			.movie_detail {
				padding: 10px 20px;

				.movie_title {
					font-size: 30px;
					width: fit-content;
					position: relative;
					top: -30px;
				}


				.genres-wrapper {
					display: flex;
					flex-direction: row;
					position: relative;
					top: -25px;

					.genres {
						margin-right: 10px;
						color: orange;
						font-size: 12px;
					}
				}

				.overview {
					font-size: 13px;
				}

				.mini-infos {
					display: grid;
					margin-top: 30px;
					font-size: 13px;
					grid-row-gap: 10px;
					grid-template-columns: 1fr 1fr 1fr;
				}

				.casts {
					margin-top: 40px;

					.title {
						font-size: 20px;
						display: flex;
						justify-content: space-between;

						.movie__see_all_cast {
							color: orange;
						}
					}

					.castPosterWrapper {
						margin-top: 10px;
        		display: grid;
        		grid-template-columns: repeat(20, 150px);
        		grid-column-gap: 8px;
        		overflow-x: scroll;

        		&::-webkit-scrollbar {
          		width: 0px;
          		background: transparent;
        		}
					}
				}

				.trailer {
						margin: 30px 0 10px 0;
						font-size: 20px;
				}
			}
		}
	}
	

</style>>