<template>
	<q-page class="flex flex-center">
		<iframe
			width="100%"
			height="100%"
			frameborder="0"
			style="border: 0"
			:src="
				'https://www.google.com/maps/embed/v1/directions?key=AIzaSyByMQs_LiFzIu_tTNcKnkTUhMysu3W14V0&mode=walking&origin=' +
				data_coordinates_origin +
				'&destination=' +
				data_coordinates_destination
			"
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
			data_coordinates_origin: "",
			data_coordinates_destination: "",
		};
	},
	created() {
		this.data_coordinates_origin = this.$route.query.origin;
		this.data_coordinates_destination = this.$route.query.destination;
	},
});
</script>

<style scoped>
iframe {
	height: 92.5vh;
}
</style>
