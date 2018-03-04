<template lang="pug">
	.overlay(:class="{ 'overlay__details -open': isActive }")
		button(class="close" @click="closeOverlay")
			svg(class="icon-x")
				use(class="icon-x" xlink:href="src/assets/symbol-defs.svg#icon-x")
		.overlay__bg
		.item(v-if="!showForm")
			.item__img(v-if="selectedIcon !== ' '")
				svg(:class="'icon-' + selectedIcon")
					use(:xlink:href="'/src/assets/symbol-defs.svg#icon-' + selectedIcon")
	
			.item__body
				span.title {{ name }} ({{ age }})
				span {{ jobTitle }}
				p {{ bio }}

		.action-bar(v-if="!confirmDeletion")
			template(v-if="!showForm")
				button(@click="openForm" class="btn btn--round -edit" aria-label="Edit profile")
					svg(version="1.1" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="icon-edit")
						title Edit profile
						path(d="M21.7 7.3l-5-5c-0.4-0.4-1-0.4-1.4 0l-13 13c-0.2 0.2-0.3 0.4-0.3 0.7v5c0 0.6 0.4 1 1 1h5c0.3 0 0.5-0.1 0.7-0.3l13-13c0.4-0.4 0.4-1 0-1.4zM7.6 20h-3.6v-3.6l12-12 3.6 3.6-12 12z")
					span Edit profile
				button(@click="confirm" class="btn btn--round -edit" aria-label="Delete profile")
					svg(version="1.1" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="icon-user-minus")
						path(d="M12 14h-7c-2.8 0-5 2.2-5 5v2c0 0.6 0.4 1 1 1s1-0.4 1-1v-2c0-1.7 1.3-3 3-3h7c1.7 0 3 1.3 3 3v2c0 0.6 0.4 1 1 1s1-0.4 1-1v-2c0-2.8-2.2-5-5-5z")
						path(d="M8.5 12c2.8 0 5-2.2 5-5s-2.2-5-5-5-5 2.2-5 5 2.2 5 5 5zM8.5 4c1.7 0 3 1.3 3 3s-1.3 3-3 3-3-1.3-3-3 1.3-3 3-3z")
						path(d="M23 10h-6c-0.6 0-1 0.4-1 1s0.4 1 1 1h6c0.6 0 1-0.4 1-1s-0.4-1-1-1z")
					span Delete profile
				
		.alert(role="alert" v-if="confirmDeletion")
			span Are you sure you want to delete this profile?
			.action-bar
				button(@click="deleteItem(index)" class="btn btn--round -edit" aria-label="Confirm")
					svg(version="1.1" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="icon-check")
						path(d="M20.7 5.3c-0.4-0.4-1-0.4-1.4 0l-10.3 10.3-4.3-4.3c-0.4-0.4-1-0.4-1.4 0s-0.4 1 0 1.4l5 5c0.2 0.2 0.4 0.3 0.7 0.3s0.5-0.1 0.7-0.3l11-11c0.4-0.4 0.4-1 0-1.4z")
					span Yes, delete profile
				button(@click="closeAlert" class="btn btn--round -edit" aria-label="No")
					svg(version="1.1" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="icon-x")
						path(d="M13.4 12l5.3-5.3c0.4-0.4 0.4-1 0-1.4s-1-0.4-1.4 0l-5.3 5.3-5.3-5.3c-0.4-0.4-1-0.4-1.4 0s-0.4 1 0 1.4l5.3 5.3-5.3 5.3c-0.4 0.4-0.4 1 0 1.4 0.2 0.2 0.4 0.3 0.7 0.3s0.5-0.1 0.7-0.3l5.3-5.3 5.3 5.3c0.2 0.2 0.5 0.3 0.7 0.3s0.5-0.1 0.7-0.3c0.4-0.4 0.4-1 0-1.4l-5.3-5.3z")
					span No, take me back


		div(v-if="showForm" :class="{'item__edit':showForm}")
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
				.action-bar
					button(class="btn btn--round -edit" id="form-btn" type="submit" @click.prevent="editItem(item)") 
						svg(class="icon-user-check")
							use(class="icon-user-check" xlink:href="src/assets/symbol-defs.svg#icon-user-check")
						span Save changes

					button(@click="closeForm" class="btn btn--round -edit")
						svg(version="1.1" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="icon-close")
							title Close form
							path(d="M13.4 12l5.3-5.3c0.4-0.4 0.4-1 0-1.4s-1-0.4-1.4 0l-5.3 5.3-5.3-5.3c-0.4-0.4-1-0.4-1.4 0s-0.4 1 0 1.4l5.3 5.3-5.3 5.3c-0.4 0.4-0.4 1 0 1.4 0.2 0.2 0.4 0.3 0.7 0.3s0.5-0.1 0.7-0.3l5.3-5.3 5.3 5.3c0.2 0.2 0.5 0.3 0.7 0.3s0.5-0.1 0.7-0.3c0.4-0.4 0.4-1 0-1.4l-5.3-5.3z")
						span Close form

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
				isActive: null,
				confirmDeletion: false
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
			confirm() {
				this.confirmDeletion = true;
			},
			closeAlert() {
				this.confirmDeletion = false;
			},
			deleteItem(index) {
				this.$emit('itemDeleted', index);
				this.isActive = false;
			},
			openForm() {
				this.showForm = true;
			},
			closeForm() {
				this.showForm = false;
			},
			openOverlay(e) {
				this.isActive = true;
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
