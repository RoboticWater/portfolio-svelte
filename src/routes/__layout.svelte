<script context="module">
	export async function load({ url }) {
		return {
			props: {
				url
			}
		};
	}
</script>

<script>
	import { layoutBGWhite } from '../layout-bg';
	import '../app.css';

	export let url;

	let y, innerHeight;
	$: hideExtra = y > innerHeight * 0.4;
</script>

<svelte:window bind:scrollY={y} bind:innerHeight />

<header class="main-header" class:white={$layoutBGWhite}>
	<a href="/">Home</a>
	<a href="/#work">Work</a>
	<a href="./Britti-Resume-2022.pdf" target="__blank">Resume</a>
</header>
<div class="name" class:remove={url.pathname !== '/'}>
	<a href="/" class:hide={hideExtra}>
		J<span class="shrink" style="width: 32px">ohn&nbsp;</span>B<span
			class="shrink"
			style="width: 26px">ritti</span
		>
	</a>
</div>
<main>
	<slot />
</main>

<style lang="scss">
	.name {
		position: fixed;
		left: 1rem;
		top: 1rem;
		// backdrop-filter: blur(10px);
		// -webkit-backdrop-filter: blur(10px);
		background: var(--white);
		border-radius: 0.25rem;
		z-index: 2000;
		padding: 0.5rem;
		font-size: 1rem;
		a {
			color: var(--black);
			text-decoration: none;
			display: flex;
		}
		span {
			white-space: nowrap;
		}

		.shrink {
			overflow: hidden;
			display: inline-block;
			transition: opacity 0.5s ease, width 0.5s ease;
		}

		.hide .shrink {
			width: 0 !important;
			opacity: 0;
		}
		&.remove {
			display: none;
		}
	}
	header {
		position: fixed;
		top: 1rem;
		right: 1rem;
		z-index: 2000;
		border-radius: 0.25rem;
		font-size: 1rem;
		padding: 0.5rem;
		background: var(--white);
		// backdrop-filter: blur(10px);
		// -webkit-backdrop-filter: blur(10px);
		transition: background 0.2s ease;
		&.white {
			background: var(--white);
		}
		a {
			color: var(--black);
			text-decoration: none;
			padding: 0.5rem;
			border-radius: 2px;
			&:hover {
				text-decoration: underline;
				// background: rgba(0, 0, 0, 0.05);
			}
		}
	}
</style>
