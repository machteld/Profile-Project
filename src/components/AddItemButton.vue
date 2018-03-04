<template lang="pug">
	.add-item-button(id="toggle-overlay")
		button(type="button" class="btn btn--round" @click="toggleOverlay")
			svg(version="1.1" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="icon-user")
				title user-plus
				path(d="M12 14h-7c-2.8 0-5 2.2-5 5v2c0 0.6 0.4 1 1 1s1-0.4 1-1v-2c0-1.7 1.3-3 3-3h7c1.7 0 3 1.3 3 3v2c0 0.6 0.4 1 1 1s1-0.4 1-1v-2c0-2.8-2.2-5-5-5z")
				path(d="M8.5 12c2.8 0 5-2.2 5-5s-2.2-5-5-5-5 2.2-5 5 2.2 5 5 5zM8.5 4c1.7 0 3 1.3 3 3s-1.3 3-3 3-3-1.3-3-3 1.3-3 3-3z")
				path(d="M23 10h-2v-2c0-0.6-0.4-1-1-1s-1 0.4-1 1v2h-2c-0.6 0-1 0.4-1 1s0.4 1 1 1h2v2c0 0.6 0.4 1 1 1s1-0.4 1-1v-2h2c0.6 0 1-0.4 1-1s-0.4-1-1-1z")
			span Nieuw profiel
</template>

<script>
  	import { itemBus } from '../main';

	export default {
        props: ['isActive'],
        methods: {
            toggleOverlay() {
                itemBus.$emit('overlayToggled', this.isActive);
            }
        }
    }
</script>