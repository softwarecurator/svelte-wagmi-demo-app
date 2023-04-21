<script lang="ts">
	import { web3Modal, connected, disconnectWagmi, signerAddress } from 'svelte-wagmi';
	import { fetchEnsName, fetchEnsAvatar } from '@wagmi/core';

	const connect = async () => {
		if (!$connected) $web3Modal.openModal();
	};

	const getENS = async () => {
		try {
			const address: any = `0x${$signerAddress?.slice(2)}`;

			const ensName: any = await fetchEnsName({
				address,
				chainId: 1
			});
			const avatarUrl = await fetchEnsAvatar({
				address: ensName,
				chainId: 1
			});

			return { ensName, avatarUrl };
		} catch (err) {
			console.log(err);
			return { ensName: $signerAddress, avatarUrl: null };
		}
	};
</script>

<div
	class="py-3 text-black flex items-center justify-center border-b-[1px] border-[#0e0f14] w-full px-6 md:px-16 m-auto"
>
	{#if $connected}
		<div class="flex gap-2 items-center">
			{#await getENS()}
				<button class="inline-block bg-semi-dark animate-pulse py-2 px-4 rounded-lg">
					<div class="items-center flex justify-around">
						<div class="h-8 w-8 rounded-full mr-4 bg-gray-400" />
						<div class="w-24 h-4 rounded-full bg-gray-400" />
					</div>
				</button>
			{:then { ensName, avatarUrl }}
				<button
					class="dark:bg-semi-dark border-[1px] border-dark py-2 px-4 rounded-lg m-auto"
					on:click={disconnectWagmi}
				>
					<div class="items-center flex justify-around">
						{#if avatarUrl !== null}
							<img src={avatarUrl} alt={ensName} class="h-8 rounded-full mr-4" />
							<p>{ensName.slice(0, -4)}</p>
						{:else}
							<p>{ensName.slice(0, 8)}</p>
						{/if}
					</div>
				</button>
			{/await}
		</div>
	{:else}
		<button on:click={connect} class="bg-black py-2 px-4 md:px-8 text-white rounded-lg m-auto"
			>Connect Wallet</button
		>
	{/if}
</div>
