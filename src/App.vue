<template lang="pug">
	.layout
		app-header
</template>

<script>
import Header from './components/Header.vue';

export default {
	name: 'App',
	components: {
		appHeader: Header,
	},
	data: function() {
		return {
			
		}
	},
	
}
</script>