<script>
	import MdCheckCircle from 'svelte-icons/md/MdCheckCircle.svelte';
	export let border = false;
	export let check = false;
</script>

<div class="card" class:border>
	<h5><slot name="label">Card label</slot></h5>
	<h4><slot name="title">Card Title</slot></h4>
	<div class="content">
		<slot name="content">Card content</slot>
	</div>
	{#if check}
		<div class="check">
			<MdCheckCircle />
		</div>
	{/if}
</div>

<style lang="scss">
	.card {
		display: grid;
		grid-template-areas: 'label check' 'title check' 'content check';
		grid-template-rows: auto auto auto;
		grid-template-columns: auto auto;
		margin-bottom: 0.75rem;
		box-shadow: var(--box-shadow-default);
		border-radius: 0.25rem;
		padding: 1rem;
		font-size: 0.9em;
		background: var(--white);
		gap: 0 0.5rem;
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
		grid-area: label;
	}
	h4 {
		margin: 0 0 0.5rem 0;
		grid-area: title;
	}
	.content {
		grid-area: content;
	}
	.check {
		align-self: center;
		justify-self: flex-end;
		grid-area: check;
		width: 40px;
		height: 40px;
		color: var(--article-color);
	}
</style>
