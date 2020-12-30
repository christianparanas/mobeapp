<template>
  <div class="slug">
    <Nav />
  	<div class="slug-head">
  		<img class="headimg" :src="contentBdImg" alt="">
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
    		<div class="trailer" v-if="haveTrailer">Trailer</div>
    		<iframe width="100%" height="250"
					:src="trailerVideo">
				</iframe>
    		<div class="casts">
    			<div class="title">
    				<div class="">Cast</div>
          </div>
    			<div class="castPosterWrapper">
    				<CastPoster v-for="cast in casts.cast" :content="cast" :key="cast.cast_id" />
    			</div>
    		</div>

        <div class="simillar__movies" v-if="haveSimillar">
          <div class="title">Simillar Movies</div>
          <div class="simillarMoviesWrapper">
            <ContentPoster v-for="simillarmovie in simillarMovies.results" :content="simillarmovie" :key="simillarmovie.id" />
        </div>
      </div>
    	</div>
  	</div>
  </div>
</template>
<script>
  export default {
    transition: 'home',
  	// getting the params id and use it to fetch specific data from tmdb
    async asyncData({ params, $axios }) {

      let contentData = await $axios.$get(`https://api.themoviedb.org/3/movie/${parseInt(params.slug)}?api_key=a807f0095433ac989503323b5b0bc933&language=en-US`)

      let genres = contentData.genres

      let contentBdImg = `https://image.tmdb.org/t/p/w342/${contentData.backdrop_path}`

      let casts = await $axios.$get(`https://api.themoviedb.org/3/movie/${parseInt(params.slug)}/credits?api_key=a807f0095433ac989503323b5b0bc933&language=en-US`)

      let movieTrailer = await $axios.$get(`https://api.themoviedb.org/3/movie/${parseInt(params.slug)}/videos?api_key=a807f0095433ac989503323b5b0bc933&language=en-US`)

      let simillarMovies = await $axios.$get(`https://api.themoviedb.org/3/movie/${parseInt(params.slug)}/similar?api_key=a807f0095433ac989503323b5b0bc933&language=en-US&page=1`);

      // declaring variables
      const trailer = []
      let trailerVideo = ""
      let videoId = ""
      let i = 0

      let haveSimillar = true
      // check if the movie have simillar movies
      if(simillarMovies.results.length == 0) {
        haveSimillar = false
      }

      let haveTrailer = true
      // checks if the trailer array has a content, if dont, just ignored
      if(!movieTrailer.results.length == 0) {
        for(i = 0; i < 1; i++) {
          trailer.push(movieTrailer.results[i])
        }

        // convert youtube video into youtube embed video by calling the getId function
        videoId = getId(`http://www.youtube.com/watch?v=${trailer[0].key}`) || getId(`http://www.youtube.com/watch?v=${trailer[0].key}`);
        trailerVideo = `https://www.youtube.com/embed/${videoId}`
      } else {
        haveTrailer = false
      }

      // function that converts youtube watch into embed
      function getId(url) {
    		var regExp = /^.*(youtu.be\/|v\/|u\/\w\/|embed\/|watch\?v=|\&v=)([^#\&\?]*).*/;
    		var match = url.match(regExp);

    		if (match && match[2].length == 11) {
        	return match[2];
    		} else {
        	return 'error';
    		}
			}
      
      return { 
      	contentData,
      	contentBdImg,
      	genres,
      	casts,
      	trailerVideo,
        simillarMovies,
        haveSimillar,
        haveTrailer
      }
    },
  }
</script>

<style lang="scss">
	.slug {
    z-index: 1;
		width: 100%;
    background-color: #091a28;
    color: #fff;

		.slug-head {

			.headimg {
				width: 100%;
        padding-top: 65px;
			}

			.movie_detail {
				padding: 10px 20px;

				.movie_title {
					font-size: 30px;
					width: fit-content;
				}


				.genres-wrapper {
					display: flex;
					flex-direction: row;

					.genres {
						margin-right: 10px;
						color: orange;
						font-size: 12px;
					}
				}

				.overview {
					font-size: 13px;
          margin-top: 20px;
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
						font-size: 15px;
            color: orange;

						.movie__see_all_cast {
							color: orange;
						}
					}

					.castPosterWrapper {
						margin-top: 10px;
        		display: grid;
        		grid-template-columns: repeat(100, 150px);
        		grid-column-gap: 8px;
        		overflow-x: scroll;

        		&::-webkit-scrollbar {
          		width: 0px;
          		background: transparent;
        		}
					}
				}

        .simillar__movies {
          margin: 30px 0;

          .title {
            font-size: 15px;
            color: orange;
          }

          .simillarMoviesWrapper {
            margin-top: 10px;
            display: grid;
            grid-template-columns: repeat(100, 150px);
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
						font-size: 15px;
            color: orange;
				}
			}
		}
	}
	

</style>>