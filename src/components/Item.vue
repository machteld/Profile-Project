<template lang="pug">
	.grid
		template(v-for="(item, index) in items" )
			transition(name="fade" appear v-on:enter="enter")
				.item(:data-index="index" @click="toggleDetails(item)")
					.item__img(v-if="item.selectedIcon !== ' '" )
						svg(:class="'icon-' + item.selectedIcon")
							use(:xlink:href="'/src/assets/symbol-defs.svg#icon-' + item.selectedIcon")
							
					.item__body
						span {{ item.name }}
						span {{ item.jobTitle }}
						//button.btn.-primary(@click="deleteItem(index)") Delete profile
	
</template>

<script>
import { itemBus } from '../main';

export default {
	props:['items'],
	data: function(){
		return {
			name: '',
			jobTitle: '',
			age: '',
			bio: '',
			selectedIcon: '',
			item: {
				name: '',
				jobTitle: '',
				age: '',
				bio: '',
				selectedIcon: ''
			},
			
		}
	},

	methods: {
		deleteItem(index) {
			this.$emit('itemDeleted', index);
		},
		toggleDetails(item) {
			itemBus.$emit('detailToggled', item);
		},
		closeOverlay() {
			this.isActive = false;
		},
		enter: function (el) {
			console.log('enter');
			var delay = el.dataset.index * 250;
			el.style.setProperty('--delay', delay + 'ms');
		},
	}
}
</script>