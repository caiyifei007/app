<template>
	<van-swipe :autoplay="3000">
		<van-swipe-item v-for="(item, index) in images" :key="index">
			<img :src="serverBaseURL + item.img_url" width="100%" />
		</van-swipe-item>
	</van-swipe>
</template>

<script>
import { mapState } from "vuex";
export default {
	data() {
		return {
			images: [],
			startX: ""
		};
	},
	computed: {
		...mapState(["serverBaseURL"])
	},
	created() {
		this.$api
			.getCommunityCarousel()
			.then(res => {
				this.images = res.data.result;
				// console.log(res.data.result);
			})
			.catch(err => {
				console.log(err);
			});
	},
	methods: {}
};
</script>
<style scoped>
.van-swipe {
	margin-top: 10px;
	height: 150px;
}
.van-swipe img {
	width: 100%;
	height: 100%;
}
</style>
