<script>
	export let border = false;
</script>

<div class="card" class:border>
	<h5><slot name="label">Card label</slot></h5>
	<h4><slot name="title">Card Title</slot></h4>
	<div>
		<slot name="content">Card content</slot>
	</div>
</div>

<style lang="scss">
	.card {
		margin-bottom: 0.75rem;
		/*  */
		box-shadow: var(--box-shadow-default);
		border-radius: 0.25rem;
		padding: 1rem;
		font-size: 0.9em;
		background: var(--white);
		&.border {
			border: 1px solid rgba(51, 51, 51, 0.15);
			box-shadow: none;
		}
	}
	h5 {
		font-size: 0.75rem;
		margin: 0 0 0.25rem 0;
		text-transform: uppercase;
		color: rgba(51, 51, 51, 0.4);
		letter-spacing: 1px;
	}
	h4 {
		margin: 0 0 0.5rem 0;
	}
</style>
