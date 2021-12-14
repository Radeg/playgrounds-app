<template>
	<q-layout view="lHh Lpr lFf">
		<q-header elevated>
			<q-toolbar>
				<q-btn
					@click="goBack"
					icon="chevron_left"
					label="Zpět"
					flat
					no-caps
				/>
				<q-toolbar-title> Dětská hřiště </q-toolbar-title>
			</q-toolbar>
		</q-header>

		<q-page-container>
			<router-view />
		</q-page-container>
	</q-layout>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
	name: "SidepageLayout",
	methods: {
		goBack() {
			return this.$router.go(-1);
		},
	},
});
</script>
