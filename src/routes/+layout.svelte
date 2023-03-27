<script>
	import Header from './Header.svelte';
	import './styles.css';

	import copyleft from '$lib/images/copyleft.svg';
</script>

<div class="app">
	<Header />

	<main>
		<slot />
	</main>

	<footer>
		<a href="https://github.com/Daniel-Clough-LLC/PleaseSendNoodles/tree/main/LICENSE.md"><img class="copyleft" src={copyleft} alt="copyleft" /> Copyleft AGPLv3</a>
	</footer>
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
	}
	.copyleft {
		width: 1rem;
	}

	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 100%;
		max-width: 64rem;
		margin: 0 auto;
		box-sizing: border-box;
	}

	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 12px;
	}

	footer a {
		font-weight: bold;
		align-items: center;
		display: flex;
	}
	img {
		margin-right: .25rem;
	}

	@media (min-width: 480px) {
		footer {
			padding: 12px 0;
		}
	}
</style>
