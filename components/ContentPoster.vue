<template>
  <div class="">
    <div id="ContentPoster" class="content-poster" v-if="!showSkelBool">
      <NuxtLink :to="contentpath">
        <img :src="image" alt="" />
      </NuxtLink>
    </div>
    <div v-if="showSkelBool" class="skeleton animate-pulse"></div>
  </div>
</template>

<script>
export default {
  name: "ContentPoster",
  props: ["content"],
  data() {
    return {
      contentpath: "",
      image: "",
      havePoster: true,
      showSkelBool: true
    };
  },
  mounted() {
    // checks if the movie have a poster, if not, it will not be rendered
    if (this.content.poster_path == null) {
      this.havePoster = false;
    } else {
      this.image = `https://image.tmdb.org/t/p/w342${this.content.poster_path}`;
      this.contentpath = `/${this.content.id}`;
    }

		this.showSkeletonLoader()
  },
	methods: {
		showSkeletonLoader() {
			setTimeout(() => {
				this.showSkelBool = false
			}, 2000)
		}
	}
};
</script>

<style lang="scss" scoped>
.skeleton {
  width: 100%;
  background-color: #1f2937;
  height: 222px;
}
</style>
