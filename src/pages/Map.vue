<template>
	<q-page class="flex flex-center">
		<iframe
			width="100%"
			height="100%"
			frameborder="0"
			style="border: 0"
			:src="'https://www.google.com/maps/embed/v1/place?key=AIzaSyByMQs_LiFzIu_tTNcKnkTUhMysu3W14V0&&q=' + data_coordinates"
			allowfullscreen
				>
		</iframe>
	</q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
	name: "Map",
	data() {
		return {
			data_coordinates: ""
		}
	},
	created() {
		this.data_coordinates = this.$route.params.loc;
	}
});
</script>

<style scoped>
iframe {
	height: 100vh;
}
</style>
