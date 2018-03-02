<template lang="pug">
	.layout
		app-header
		app-add-item-button
		app-item(:items="items" @itemDeleted="deleteItem")
		app-item-detail
		app-form(:items="items" @itemAdded="newItem")
</template>

<script>
import Header from './components/Header.vue';
import AddItem from './components/AddItemButton.vue';
import Item from './components/Item.vue';
import Form from './components/Form.vue';
import ItemDetail from './components/ItemDetail.vue';

export default {
	name: 'App',
	components: {
		appHeader: Header,
		appAddItemButton: AddItem,
		appItem: Item,
		appForm: Form,
		appItemDetail: ItemDetail
	},
	data: function() {
		return {
			items: [
				{
					name: 'Machteld Vlietstra',
					age: '41',
					jobTitle: 'Frontend Developer',
					selectedIcon: 'github',
					bio: 'Space, the final frontier. These are the voyages of the Starship Enterprise. Its five-year mission: to explore strange new worlds, to seek out new life and new civilizations, to boldly go where no man has gone before.'
				},
				{
					name: 'Jan Klaassen',
					age: '32',
					jobTitle: 'Backend Developer',
					selectedIcon: 'gitlab',
					bio:'Zombie ipsum reversus ab viral inferno, nam rick grimes malum cerebro. De carne lumbering animata corpora quaeritis. Summus brains sit​​, morbo vel maleficia?'
				},
				{
					name: 'Jan Janssen',
					age: '51',
					jobTitle: 'Lead Frontend Developer',
					selectedIcon: 'user',
					bio: 'Bacon ipsum dolor amet short ribs brisket venison rump drumstick pig sausage prosciutto chicken spare ribs salami picanha doner. '
				},
				{
					name: 'Jan Franssen',
					age: '25',
					jobTitle: 'Junior Frontend Developer',
					selectedIcon: 'gitlab',
					bio: 'Lorem ipsum dolor amet mustache knausgaard +1, blue bottle waistcoat tbh semiotics artisan synth stumptown gastropub cornhole celiac swag.'
				},
			],
			icons: ['github','gitlab', 'user'],
			isActive: false
		}
	},
	mounted() {
		this.getItems();
	},
	methods: {
		getItems() {
			if (localStorage.getItem('item_list')) {
				this.items = JSON.parse(localStorage.getItem('item_list'));
			}
		},
		newItem(item) {
			this.items.push(item);
		},
		deleteItem(index) {
			this.items.splice(index, 1);
		},		
	},
	watch: {
		items: {
			handler: function(updatedList) {
				localStorage.setItem('item_list', JSON.stringify(updatedList));
			},
			deep: true
		}
	},
	
}
</script>