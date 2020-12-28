<template>
  <div id="App" class="container">
    <Nav />
    <div class="content">
      <div class="header text-2xl">
        TMDb data
      </div>
      <div class="search-bar">
        <svg class="search-icon" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" ><path d="M10,18c1.846,0,3.543-0.635,4.897-1.688l4.396,4.396l1.414-1.414l-4.396-4.396C17.365,13.543,18,11.846,18,10 c0-4.411-3.589-8-8-8s-8,3.589-8,8S5.589,18,10,18z M10,4c3.309,0,6,2.691,6,6s-2.691,6-6,6s-6-2.691-6-6S6.691,4,10,4z"></path></svg>
        <form action="">
          <input type="text" placeholder="Search" >
        </form>
      </div>
      <div class="header-section">
        <div class="">Now Playing</div>
        <div class="">Explore</div>
      </div>
      <div class="category-content">
        <ContentPoster v-for="nowPlaying in nowPlayingMovies.results" :content="nowPlaying" :key="nowPlaying.id" @mainContent="toogleMainContent" />
      </div>

      <div class="header-section">
        <div class="">Popular</div>
        <div class="">Explore</div>
      </div>
      <div class="category-content">
        <ContentPoster v-for="popular in popularMovies.results" :content="popular" :key="popular.id" @mainContent="toogleMainContent" />
      </div>

      <div class="header-section">
        <div class="">Upcoming Movies</div>
        <div class="">Explore</div>
      </div>
      <div class="category-content" >
        <ContentPoster v-for="upcoming in upcomingMovies.results" :content="upcoming" :key="upcoming.id" @mainContent="toogleMainContent" />
      </div>

      <div class="header-section">
        <div class="">Top Rated Movies</div>
        <div class="">Explore</div>
      </div>
      <div class="category-content" >
        <ContentPoster v-for="toprated in topRatedMovies.results" :content="toprated" :key="toprated.id" @mainContent="toogleMainContent" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  async asyncData({ $axios }) {
    const nowPlayingMovies = await $axios.$get('https://api.themoviedb.org/3/movie/now_playing?api_key=a807f0095433ac989503323b5b0bc933&language=en-US&page=1');

    const popularMovies = await $axios.$get('https://api.themoviedb.org/3/movie/popular?api_key=a807f0095433ac989503323b5b0bc933&language=en-US&page=1');

    // const actionMovies = await $axios.$get('https://api.themoviedb.org/3/discover/movie?api_key=a807f0095433ac989503323b5b0bc933&language=en-US&sort_by=popularity.asc&include_adult=false&include_video=false&page=1&with_genres=28');
    const topRatedMovies = await $axios.$get('https://api.themoviedb.org/3/movie/top_rated?api_key=a807f0095433ac989503323b5b0bc933&language=en-US&page=1');

    const upcomingMovies = await $axios.$get('https://api.themoviedb.org/3/movie/upcoming?api_key=a807f0095433ac989503323b5b0bc933&language=en-US&page=1');

    return {
      nowPlayingMovies,
      popularMovies,
      topRatedMovies,
      upcomingMovies
    }
  },
  methods: {
    toogleMainContent(id) {
      console.log(`ID: ${id}`)
    }
  }
}
</script>

<!-- https://api.themoviedb.org/3/trending/all/day?api_key=a807f0095433ac989503323b5b0bc933 -->

<style lang="scss">
  body {
    width: 100%;
    background-color: #091a28;
    color: #fff;
  }

  .container {
    width: 100%;

    .content {
      width: 100%;
      padding: 100px 20px 20px;

      .search-bar {

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
