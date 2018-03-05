<template lang="pug">
	.overlay(v-bind:class="{ 'overlay__form -open': isActive }")
		button(class="close" @click="closeOverlay")
			svg(class="icon-x")
				use(class="icon-x" xlink:href="src/assets/symbol-defs.svg#icon-x")
		.overlay__bg
		.overlay__body
			form()
				legend Voeg een nieuw profiel toe
				.form-field--select
					label(for="select-icon" class="label__select") Kies een icoon
					select(id="select-icon" class="input__select" v-model="item.selectedIcon")
						option(v-for="i in icons" :key="i") {{ i }}
				.form-field
					input(type="text" class="input-field" v-on:focus="focusFn" v-model="item.name")
					label Naam
				.form-field
					input(type="text" class="input-field" v-on:focus="focusFn" v-model="item.jobTitle")
					label Functietitel
				.form-field
					input(type="text" class="input-field" v-on:focus="focusFn" v-model="item.age")
					label Leeftijd
				.form-field
					textarea(class="form-field" rows="3" class="input-field" v-on:focus="focusFn" v-model="item.bio")
					label Bio

				button(class="btn btn__submit" id="form-btn" type="submit" disabled @click.prevent="newItem") 
					svg(class="icon-user-plus")
						use(class="icon-user-plus" xlink:href="src/assets/symbol-defs.svg#icon-user-plus")
					svg(class="icon-user-check")
						use(class="icon-user-check" xlink:href="src/assets/symbol-defs.svg#icon-user-check")
					span Opslaan
</template>

<script>
import { itemBus } from '../main';

export default {
	props: ['items'],
	data: function(){
		return {
			item: {
				name: '',
				jobTitle: '',
				age: '',
				bio: '',
				selectedIcon: ''
			},
			icons: ['github','gitlab', 'user'],
			isActive: null
		}
	},
	methods: {
		newItem() {
			this.$emit('itemAdded', this.item);
			this.item = '';
			this.isActive = false;
		},
		focusFn(e) {
			let el = e.target,
				submitBtn = document.getElementById('form-btn');

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
				submitBtn.removeAttribute('disabled');
			}

		},
		closeOverlay() {
			this.isActive = false;
		}
	},
	created() {
		itemBus.$on('overlayToggled', (isActive) => {
			this.isActive = true;
		});
	}

}
</script>