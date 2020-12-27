<template>
	<div id="ContentPreview" class="content-preview">
		<img :src="image" alt="Hi">
	</div>
</template>


<script>
export default {
   name: 'ContentPreview',
   props: ['content'],
   data() {
   	return {
   		image: `https://image.tmdb.org/t/p/w342/${this.content.poster_path}`,
   	}
   }
}
</script>


<style lang="scss" scoped>
	
</style>

