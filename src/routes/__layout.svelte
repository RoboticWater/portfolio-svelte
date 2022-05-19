<script>
	import '../app.css';
</script>

<header>
	<a href="/">Home</a>
	<a href="/#work">Work</a>
	<a href="/">Resume</a>
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
		backdrop-filter: blur(10px);
		-webkit-backdrop-filter: blur(10px);
		a {
			color: var(--black);
			text-decoration: none;
			margin: 0.25rem;
			&:hover {
				text-decoration: underline;
			}
		}
	}
</style>
