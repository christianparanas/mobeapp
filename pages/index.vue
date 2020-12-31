<template>
  <div id="App" class="container">
    <Nav />
    <div class="content">
      <div class="header text-2xl">
        Thea Myca
      </div>
      <div class="search-bar">
        <svg class="search-icon" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" ><path d="M10,18c1.846,0,3.543-0.635,4.897-1.688l4.396,4.396l1.414-1.414l-4.396-4.396C17.365,13.543,18,11.846,18,10 c0-4.411-3.589-8-8-8s-8,3.589-8,8S5.589,18,10,18z M10,4c3.309,0,6,2.691,6,6s-2.691,6-6,6s-6-2.691-6-6S6.691,4,10,4z"></path></svg>
        <form action="" @submit.prevent="fetchSearchResults">
          <input type="text" placeholder="Search" v-model="searchQuery">
          <svg v-if="showSpinner" class="spinner animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
            <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
            <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
          </svg>
        </form>
      </div>
      <div class="search-results-wrapper" v-if="searchResultbool"> 
          <NuxtLink class="result" v-for="result in results.results" :key="result.id" :to="searchItemId(result.id)">
            <img :src="dynaImg(result.poster_path)" alt="">
            <div class="result_title">
              <div class="">{{ result.title }}</div>
              <div class="searchRating">Rating: <span>{{ result.vote_average }}</span></div>
          </div>
          </NuxtLink>

      </div>
      <div class="noResult" v-if="noResult">No results found!</div>
      <div class="header-section">
        <div class="">Now Playing</div>
        <div class="">Explore</div>
      </div>
      <div class="category-content">
        <ContentPoster v-for="nowPlaying in nowPlayingMovies.results" :content="nowPlaying" :key="nowPlaying.id" />
      </div>

      <div class="header-section">
        <div class="">Popular</div>
        <div class="">Explore</div>
      </div>
      <div class="category-content">
        <ContentPoster v-for="popular in popularMovies.results" :content="popular" :key="popular.id" />
      </div>

      <div class="header-section">
        <div class="">Upcoming Movies</div>
        <div class="">Explore</div>
      </div>
      <div class="category-content" >
        <ContentPoster v-for="upcoming in upcomingMovies.results" :content="upcoming" :key="upcoming.id" />
      </div>

      <div class="header-section">
        <div class="">Top Rated Movies</div>
        <div class="">Explore</div>
      </div>
      <div class="category-content" >
        <ContentPoster v-for="toprated in topRatedMovies.results" :content="toprated" :key="toprated.id" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  transition: 'home',

  async asyncData({ $axios }) {
    const nowPlayingMovies = await $axios.$get('https://api.themoviedb.org/3/movie/now_playing?api_key=a807f0095433ac989503323b5b0bc933&language=en-US&page=1');

    const popularMovies = await $axios.$get('https://api.themoviedb.org/3/movie/popular?api_key=a807f0095433ac989503323b5b0bc933&language=en-US&page=1');

    const topRatedMovies = await $axios.$get('https://api.themoviedb.org/3/movie/top_rated?api_key=a807f0095433ac989503323b5b0bc933&language=en-US&page=1');

    const upcomingMovies = await $axios.$get('https://api.themoviedb.org/3/movie/upcoming?api_key=a807f0095433ac989503323b5b0bc933&language=en-US&page=1');


    return {
      nowPlayingMovies,
      popularMovies,
      topRatedMovies,
      upcomingMovies
    }
  },
  data() {
    return {
      searchResultbool: false,
      searchQuery: '',
      results: [],
      noResult: false,
      showSpinner: false,
    }
  },
  watch: {
    results() {
      if(this.results == [] || this.results.results.length == 0) {
        this.searchResultbool = false
        this.noResult = true
        this.showSpinner = false
      }
      else {
        this.searchResultbool = true
        this.noResult = false
        this.showSpinner = false
      }
    },
    searchQuery() {
      if(this.searchQuery == '') {
        this.searchResultbool = false
        this.noResult = false
      }
    }
  },
  methods: {
    async fetchSearchResults() {
      // showing the spinner in the input
      this.showSpinner = true

      // checks if the input have content, if it has do the command inside if
      if(!this.searchQuery == '') {
        const result = await this.$axios.$get(`https://api.themoviedb.org/3/search/movie?api_key=a807f0095433ac989503323b5b0bc933&language=en-US&query=${this.searchQuery}&page=1&include_adult=false`);
        this.results = result
      } else {
        this.showSpinner = false
      }
    },
    dynaImg(poster_path) {
        return `https://image.tmdb.org/t/p/w342/${poster_path}`
    },

    // formatting the movie id so that it can be used in dynamic page
    searchItemId(contentID) {
      return `/${contentID}`
    }
  }
}
</script>


<style lang="scss">
  body {
    width: 100%;
    background-color: #091a28;
    color: #fff;
  }

  .home-enter-active, .home-leave-active { transition: opacity .3s; }
  .home-enter, .home-leave-active { opacity: 0; }

  .container {
    width: 100%;

    .content {
      width: 100%;
      padding: 100px 20px 20px;

      .search-bar {
        z-index: 2;

        .spinner {
          position: absolute;
          top: 174px;
          right: 21px;
        }

        .search-icon {
          position: relative;
          bottom: -38px;
          left: 10px;
          fill: #718096;
        }

        input {
          width: 100%;
          font-size: 13px;
          padding: 15px 15px 15px 40px;
          border-radius: 12px;
          background-color: #1f2937;
          outline: none;
          color: #a0aec0;
        }
      }

      .search-results-wrapper {
        position: absolute;
        background-color: #1f2937;
        width: 90%;
        top: 220px;
        padding: 20px;
        border-radius: 8px;
        height: fit-content;
        max-height: 400px;
        overflow-y: scroll;

        .result {
          display: grid;
          grid-template-columns: 60px 1fr;
          margin-bottom: 10px;

          .result_title {
            padding: 20px 0;
            text-align: left;
            margin-left: 20px;

            .searchRating {
              font-size: 13px;

              span {
                color: orange;
              }
            }
          }
        }
      }

      .noResult {
        position: absolute;
        background-color: #1f2937;
        width: 90%;
        top: 220px;
        padding: 10px 20px;
        border-radius: 8px;
        height: fit-content;
      }

      .header-section {
        margin-top: 30px;
        display: flex;
        justify-content: space-between;
      }

      .category-content {
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
  }

</style>
