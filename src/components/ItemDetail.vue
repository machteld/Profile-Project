<template lang="pug">
	.overlay(:class="{ 'overlay__details -open': isActive }")
		button(class="close" @click="closeOverlay")
			svg(class="icon-x")
				use(class="icon-x" xlink:href="src/assets/symbol-defs.svg#icon-x")
		.item(v-if="!showForm")
			.item__bg
			.item__img(v-if="selectedIcon !== ' '")
				svg(:class="'icon-' + selectedIcon")
					use(:xlink:href="'/src/assets/symbol-defs.svg#icon-' + selectedIcon")
	
			.item__body
				span {{ name }}
				span {{ jobTitle }}
				span {{ age }}
				span {{ bio }}
				span {{ index }}

		.action-bar
			button(@click="openForm" class="btn btn--round -edit")
				svg(version="1.1" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="icon-edit")
					title Edit profile
					path(d="M21.7 7.3l-5-5c-0.4-0.4-1-0.4-1.4 0l-13 13c-0.2 0.2-0.3 0.4-0.3 0.7v5c0 0.6 0.4 1 1 1h5c0.3 0 0.5-0.1 0.7-0.3l13-13c0.4-0.4 0.4-1 0-1.4zM7.6 20h-3.6v-3.6l12-12 3.6 3.6-12 12z")
				span Edit profile
			button(@click="deleteItem(index)" class="btn btn--round -edit")
				svg(version="1.1" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="icon-user-minus")
					path(d="M12 14h-7c-2.8 0-5 2.2-5 5v2c0 0.6 0.4 1 1 1s1-0.4 1-1v-2c0-1.7 1.3-3 3-3h7c1.7 0 3 1.3 3 3v2c0 0.6 0.4 1 1 1s1-0.4 1-1v-2c0-2.8-2.2-5-5-5z")
					path(d="M8.5 12c2.8 0 5-2.2 5-5s-2.2-5-5-5-5 2.2-5 5 2.2 5 5 5zM8.5 4c1.7 0 3 1.3 3 3s-1.3 3-3 3-3-1.3-3-3 1.3-3 3-3z")
					path(d="M23 10h-6c-0.6 0-1 0.4-1 1s0.4 1 1 1h6c0.6 0 1-0.4 1-1s-0.4-1-1-1z")
				span Delete profile



		div(v-if="showForm" :class="{'item__edit':showForm}")
			button(@click="closeForm" class="btn btn--round")
				svg(version="1.1" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="icon-close")
					title Close form
					path(d="M13.4 12l5.3-5.3c0.4-0.4 0.4-1 0-1.4s-1-0.4-1.4 0l-5.3 5.3-5.3-5.3c-0.4-0.4-1-0.4-1.4 0s-0.4 1 0 1.4l5.3 5.3-5.3 5.3c-0.4 0.4-0.4 1 0 1.4 0.2 0.2 0.4 0.3 0.7 0.3s0.5-0.1 0.7-0.3l5.3-5.3 5.3 5.3c0.2 0.2 0.5 0.3 0.7 0.3s0.5-0.1 0.7-0.3c0.4-0.4 0.4-1 0-1.4l-5.3-5.3z")

			form
				.form-field--select
					label(for="select-icon" class="label__select") Kies een icoon
					select(id="select-icon" class="input__select" v-model="selectedIcon")
						option(v-for="i in icons" :key="i") {{ i }}
				.form-field
					input(type="text" class="input-field input--filled" v-on:focus="focusFn" v-model="name")
					label Naam
				.form-field
					input(type="text" class="input-field input--filled" v-on:focus="focusFn" v-model="jobTitle")
					label Functietitel
				.form-field
					input(type="text" class="input-field input--filled" v-on:focus="focusFn" v-model="age")
					label Leeftijd
				.form-field
					textarea(class="form-field" rows="3" class="input-field input--filled" v-on:focus="focusFn" v-model="bio")
					label Bio

				button(class="btn btn__submit" id="form-btn" type="submit" @click.prevent="editItem(item)") 
					svg(class="icon-user-plus")
						use(class="icon-user-plus" xlink:href="src/assets/symbol-defs.svg#icon-user-plus")
					svg(class="icon-user-check")
						use(class="icon-user-check" xlink:href="src/assets/symbol-defs.svg#icon-user-check")
					| Opslaan

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
				index: '',
				item : {},
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
					if( el.value == '' ) {
						el.classList.toggle('input--filled');
					}
				}
			},
			editItem(item) {
				item = {
					name: this.name,
					jobTitle: this.jobTitle,
					age: this.age,
					bio: this.bio,
					selectedIcon: this.selectedIcon,
					index: this.index
				};
				this.$emit('itemEdited', item);
				this.showForm = false;
			},
			deleteItem(index) {
				this.$emit('itemDeleted', index);
			},
			openForm() {
				this.showForm = true;
			},
			closeForm() {
				this.showForm = false;
			},
			closeOverlay() {
				this.isActive = false;
			},
		},
		created() {
			itemBus.$on('detailToggled', (item, index) => {
				this.index = index;
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
