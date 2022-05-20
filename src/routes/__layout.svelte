<script>
	import { layoutBGWhite } from '../layout-bg';
	import '../app.css';
</script>

<header class="main-header" class:white={$layoutBGWhite}>
	<a href="/">Home</a>
	<a href="/#work">Work</a>
	<a href="./Britti-Resume-2022.pdf" target="__blank">Resume</a>
</header>
<main>
	<slot />
</main>

<style lang="scss">
	header {
		position: fixed;
		top: 1rem;
		right: 1rem;
		z-index: 2000;
		border-radius: 0.25rem;
		// background: rgba(255, 255, 255, 0.25);
		// background: var(--white);
		font-size: 1rem;
		padding: 0.5rem;
		// padding: 1rem 0.5rem;
		backdrop-filter: blur(10px);
		-webkit-backdrop-filter: blur(10px);
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
