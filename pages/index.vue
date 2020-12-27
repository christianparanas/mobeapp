<template>
  <div id="App" class="container">
    <Nav />
    <div class="content">
      <div class="header text-2xl">
        Find Movies, Tv shows and more ...
      </div>
      <div class="search-bar">
        <svg class="search-icon" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" ><path d="M10,18c1.846,0,3.543-0.635,4.897-1.688l4.396,4.396l1.414-1.414l-4.396-4.396C17.365,13.543,18,11.846,18,10 c0-4.411-3.589-8-8-8s-8,3.589-8,8S5.589,18,10,18z M10,4c3.309,0,6,2.691,6,6s-2.691,6-6,6s-6-2.691-6-6S6.691,4,10,4z"></path></svg>
        <form action="">
          <input type="text" placeholder="Search" >
        </form>
      </div>
      <div class="categories-slider-wrapper">
        <div class="category active-cat">Trending</div>
        <div class="category">Action</div>
        <div class="category">War</div>
        <div class="category">Fiction</div>
        <div class="category">Anime</div>
      </div>
      <div class="category-content">
        <ContentPreview v-for="content in contents.results" :content="content" :key="content.id" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  async asyncData({ $http }) {
    const contents = await $http.$get('https://api.themoviedb.org/3/trending/all/day?api_key=a807f0095433ac989503323b5b0bc933')
    console.log(contents.results);
    return {
      contents,
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

      .categories-slider-wrapper {
        margin-top: 20px;
        display: flex;
        flex-direction: row;
        overflow-x: scroll;

        .category {
          background-color: #1f2937;
          padding: 5px 10px;
          margin-right: 10px;
          border-radius: 5px;
        }

        &::-webkit-scrollbar {
          width: 0px;  /* Remove scrollbar space */
          background: transparent;  /* Optional: just make scrollbar invisible */
        }

        .active-cat {
          background-color: #6B21A8;
        }
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
