<script>
	import Footer from '$lib/Footer.svelte';
</script>

<div class="wrapper">
	<slot />
</div>

<Footer />

<style>
	.wrapper {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding-bottom: 3rem;
        min-height: 90%;
	}
</style>
