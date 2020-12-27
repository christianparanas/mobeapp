<template>
  <div class="container">
    <Nav />
    <div class="content">
      <div class="header text-3xl">
        Find Movies, Tv shows and more ...
      </div>
      <div class="search-bar">
        <input type="text" placeholder="Search" >
      </div>
    </div>
  </div>
</template>

<script>
export default {}
</script>

<style lang="scss">
  body {
    background-color: #091a28;
    color: #fff;
  }

  .container {

    .content {
      padding: 100px 30px 30px;

      .search-bar {
        margin-top: 20px;

        input {
          width: 100%;
          padding: 20px;
          border-radius: 20px;
          background-color: #1f2937;
          outline: none;
        }
      }

    }
  }

</style>
