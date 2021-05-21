<script>
	import CoinCard from "./CoinCard.svelte"

	let coins = [];
	let limit = 20;
	const pageSize = 20;
	async function fetchCoins() {
		const response = await fetch(
				`https://api.coinstats.app/public/v1/coins?skip=${limit - pageSize}&limit=${limit}`
		);
		const data = await response.json();
		coins = coins.concat(data.coins);
		console.log("coins:", coins);
	}

	const loadMore = () => {
		limit += pageSize;
		fetchCoins()
	}
</script>

<style>
	main {
		text-align: center;
		padding: 40px 0;
		margin: 0 auto;
	}
	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}
	.grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
		gap: 30px;
	}
	@media (min-width: 640px) {
		main {
			max-width: 1600px;
			padding: 40px 20px;
		}
	}
</style>

<main>
	<main>
		<h1>{name}</h1>
		{#if coins.length === 0}
			<button on:click={fetchCoins}>Fetch Coin Data!</button>
		{:else}
			<div class="grid">
				{#each coins as coin}
					<CoinCard {coin} />
				{/each}
				<button on:click={loadMore}>Fetch next page</button>
			</div>
		{/if}
	</main>
</main>
