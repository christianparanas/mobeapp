<template>
  <div class="container">
    <Nav />
    <div class="content">
      <div class="header text-2xl">
        Find Movies, Tv shows and more ...
      </div>
      <div class="search-bar">
        <input type="text" placeholder="Search" >
      </div>
      <div class="categories-slider-wrapper">
        <div class="category active-cat">recommended</div>
        <div class="category">recommended</div>
        <div class="category">recommended</div>
        <div class="category">recommended</div>
        <div class="category">recommended</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {}
</script>

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
        margin-top: 20px;

        input {
          width: 100%;
          font-size: 13px;
          padding: 15px;
          border-radius: 12px;
          background-color: #1f2937;
          outline: none;
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

    }
  }

</style>
