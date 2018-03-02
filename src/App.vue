<template lang="pug">
	.layout
		app-header
		app-item(:items="items" @itemDeleted="deleteItem")

</template>

<script>
import Header from './components/Header.vue';
import Item from './components/Item.vue';

export default {
	name: 'App',
	components: {
		appHeader: Header,
		appItem: Item,
	},
	data: function() {
		return {
			items: [
				{
					name: 'machteld',
					age: '41',
					jobTitle: 'Frontend Developer',
					selectedIcon: 'gitlab'
				},
				{
					name: 'Jan Klaassen',
					age: '41',
					jobTitle: 'Frontend Developer',
					selectedIcon: 'gitlab'
				},
				{
					name: 'Jan Janssen',
					age: '41',
					jobTitle: 'Frontend Developer',
					selectedIcon: 'github'
				},
				{
					name: 'Jan Franssen',
					age: '25',
					jobTitle: 'Frontend Developer',
					selectedIcon: 'user'
				},
			],
			icons: ['github','gitlab', 'user'],
		}
	},
	
}
</script>