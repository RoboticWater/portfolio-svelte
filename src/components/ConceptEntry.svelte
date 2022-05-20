<script>
	import Observer from '../components/Observer.svelte';
</script>

<div class="toolset-entry">
	<div class="toolset-entry__number"><slot name="number">1</slot></div>
	<div class="toolset-entry__details">
		<h3><slot name="title">Title</slot></h3>
		<div>
			<slot name="description">Description</slot>
		</div>
	</div>
	<div class="toolset-entry__demo">
		<Observer let:intersecting rootMargin="0% 0% -40%" once>
			<div class="toolset__img" class:intersecting>
				<slot name="image">
					<img src="" alt="" />
				</slot>
			</div>
		</Observer>
	</div>
</div>

<style>
	.toolset-entry {
		display: flex;
		gap: 1.25rem;
		margin-bottom: 1.25rem;
	}
	h3 {
		margin-bottom: 0.25rem;
		margin-top: 0;
		font-size: 1.75rem;
	}
	.toolset-entry__number {
		font-size: 4rem;
		font-weight: 700;
		color: var(--article-color);
		line-height: 95%;
	}
	.toolset-entry__details {
		flex: 1;
	}
	.toolset-entry__demo {
		flex: 1;
	}
	img {
		width: 100%;
	}
	.toolset__img {
		position: relative;
		top: 80px;
		opacity: 0;
		transition: top 0.5s ease, opacity 0.5s ease;
	}
	.toolset__img.intersecting {
		top: 0;
		opacity: 1;
	}
	@media screen and (max-width: 660px) {
		.toolset-entry__number {
			display: none;
		}
		.toolset-entry {
			flex-direction: column-reverse;
			margin-bottom: 2rem;
		}
		.toolset-entry__img {
			overflow: hidden;
			position: relative;
			display: flex;
		}
	}
</style>
