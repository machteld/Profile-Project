<template lang="pug">
	.overlay(v-bind:class="{ 'overlay__details -open': isActive }")
		button(class="close" @click="closeOverlay")
			svg(class="icon-x")
				use(class="icon-x" xlink:href="src/assets/symbol-defs.svg#icon-x")
		.item
			button(@click="showForm = !showForm" class="btn btn--round")
				svg(version="1.1" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="icon-edit")
					title edit-2
					path(d="M21.7 7.3l-5-5c-0.4-0.4-1-0.4-1.4 0l-13 13c-0.2 0.2-0.3 0.4-0.3 0.7v5c0 0.6 0.4 1 1 1h5c0.3 0 0.5-0.1 0.7-0.3l13-13c0.4-0.4 0.4-1 0-1.4zM7.6 20h-3.6v-3.6l12-12 3.6 3.6-12 12z")
			
			.item__img(v-if="selectedIcon !== ' '")
				svg(:class="'icon-' + selectedIcon")
					use(:xlink:href="'/src/assets/symbol-defs.svg#icon-' + selectedIcon")
					
			.item__body
				span {{ name }}
				span {{ jobTitle }}
				span {{ age }}
				span {{ bio }}

		.item__edit(v-if="showForm")
			form()
				legend Bewerk profiel
					
</template>

<script>
import { itemBus } from '../main';

	export default {
		props: ['items'],
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
				icons: ['github','gitlab', 'user'],
				showForm: false,
				isActive: null
			}
		},
		methods: {
			focusFn(e) {
				let el = e.target;
				if( el.value == '' ) {
					el.classList.toggle('input--filled');
				}
				el.addEventListener('blur', checkElement);

				function checkElement() {

					if( el.value.trim() !== '' ) {
						el.classList.add( 'input--filled' );
						checkField();
					} else {
						el.classList.remove('input--filled');
					}
				}

				function checkField() {
					document.getElementById('form-btn').removeAttribute('disabled');
				}
			},
			newItem() {
				this.$emit('itemAdded', this.item);
				this.item = '';
			},
			closeOverlay() {
				this.isActive = false;
			},
		},
		created() {
			itemBus.$on('detailToggled', (item) => {
				this.name = item.name;
				this.jobTitle = item.jobTitle;
				this.age = item.age;
				this.bio = item.bio;
				this.selectedIcon = item.selectedIcon;
				this.isActive = true;
			});
		},		
	}
</script>
