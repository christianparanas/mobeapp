<template>
  <div>
  	<img :src="contentBdImg" alt="">
    <h1>{{ contentData.title }}</h1>

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

      const contentBdImg = `https://image.tmdb.org/t/p/w342/${contentData.backdrop_path}`
      console.log(contentData)
      return { 
      	contentData,
      	contentBdImg
      }
    },
  }
</script>