<script>
	import '../app.css';
	import { configureWagmi, wagmiLoaded } from 'svelte-wagmi';
	import { onMount } from 'svelte';
	import { PUBLIC_ALCHEMY_KEY, PUBLIC_WALLETCONNECT_PROJECTID } from '$env/static/public';

	onMount(async () => {
		await configureWagmi({
			walletconnect: true,
			walletconnectProjectID: PUBLIC_WALLETCONNECT_PROJECTID,
			alchemyKey: PUBLIC_ALCHEMY_KEY
		});
	});
</script>

{#if $wagmiLoaded}
	<slot />
{/if}
