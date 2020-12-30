<template>
	<NuxtLink :to="contentpath">
		<div class="gen">{{ content.name }}</div>
	</NuxtLink>	
</template>


<script>
	export default {
		props: ['content'],
		data() {
			return {
				contentpath: "",
			}
		},
		mounted() {
			this.contentpath = `/genres/${this.content.id}`
		}
	}
</script>

<style lang="scss" scoped>
	.gen {
		display: grid;
		place-items: center;
		background-color: #1f2937;
		height: 100px;
		font-size: 13px;
	}
</style>