<script>
	import { onMount } from 'svelte';
	import { page } from '$app/stores';
	import { browser } from '$app/env';
	import Overlay from './provider/Overlay.svelte';
	import { closeSidenav } from './provider/store';
	import TopNavigation from './topnavigation/Index.svelte';
	import SideNavigation from './sidenavigation/Index.svelte';

	onMount(() => {
		document.getElementsByTagName('body').item(0).removeAttribute('tabindex');
	});

	if (browser) {
		page.subscribe(() => {
			// close side navigation when route changes. it's triggered when viewport is less than 1024px
			if (window.innerWidth < 1024) {
				closeSidenav();
			}
		});
	}
</script>

<!-- w-[calc(100%-16rem)] class get the remain width of the main tag from lg:viewport by subtracting
(the total width by the width of the side navigation component which is w-64 = 16rem)-->

<div class="bg-mac bg-center bg-cover h-screen overflow-hidden w-full lg:p-4">
	<div class="content h-screen overflow-hidden relative lg:rounded-2xl">
		<div class="flex items-start">
			<Overlay />
			<SideNavigation mobilePosition="right" />
			<div class="flex flex-col h-screen pl-0 w-full lg:space-y-4 lg:w-[calc(100%-16rem)]">
				<TopNavigation />
				<main class="main h-screen pb-36 pt-4 px-2 md:pb-8 md:px-4 lg:px-6">
					<slot />
				</main>
			</div>
		</div>
	</div>
</div>

<style>
	.content {
		background-color: rgba(16 18 27 / 40%);
		backdrop-filter: blur(24px);
	}

	.main {
		color: #f9fafb;
		background-color: rgba(16 18 27 / 40%);
		overflow: auto;
	}

	.main::-webkit-scrollbar {
		width: 6px;
		border-radius: 10px;
	}

	.main::-webkit-scrollbar-thumb {
		background: rgb(1 2 3 / 40%);
		border-radius: 10px;
	}
</style>
