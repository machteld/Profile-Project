<template lang="pug">
	.grid
		template(v-for="(item, index) in items" )
			transition(name="fade" appear v-on:enter="enter")
				.item(:data-index="index" @click="toggleDetails(item, index)" :class="randomClass()")
					.item__img(v-if="item.selectedIcon !== ' '" )
						svg(:class="'icon-' + item.selectedIcon")
							use(:xlink:href="'/src/assets/symbol-defs.svg#icon-' + item.selectedIcon")
							
					.item__body
						span {{ item.name }}
						span {{ item.jobTitle }}
	
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
			}
		}
	},

	methods: {
		toggleDetails(item, index) {
			itemBus.$emit('detailToggled', item, index);
		},
		closeOverlay() {
			this.isActive = false;
		},
		enter: function (el) {
			let delay = el.dataset.index * 250;
			el.style.setProperty('--delay', delay + 'ms');
		},
		randomClass() {
			return 'color' + Math.floor(Math.random()*(3-1+1)+1)
		}
	}
}
</script>