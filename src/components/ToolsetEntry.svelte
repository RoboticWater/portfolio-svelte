<script>
	import MdPlayCircleFilled from 'svelte-icons/md/MdPlayCircleFilled.svelte';
	import Observer from '../components/Observer.svelte';
	export let imgBackground;
	export let imgForeground;
	export let demoHash = '#';
	export let openDemo = () => {};
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
			<div class="toolset-entry__img">
				<img src={imgBackground} alt="" />
				<img src={imgForeground} class:intersecting alt="" />
			</div>
		</Observer>
		<a href={demoHash} on:click={openDemo}>
			<span class="icon"><MdPlayCircleFilled /></span> Watch the demo
		</a>
	</div>
</div>

<style>
	.toolset-entry {
		display: flex;
		gap: 1.25rem;
		margin-bottom: 1.25rem;
	}
	a {
		text-decoration: none;
		color: var(--article-color);
		fill: var(--article-color);
		display: flex;
		align-items: center;
		gap: 4px;
	}
	a:hover {
		text-decoration: underline;
		text-decoration-thickness: 0.075em;
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
	.icon {
		width: 22px;
		height: 22px;
		display: inline-block;
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
	.toolset-entry__img {
		overflow: hidden;
		position: relative;
		display: flex;
		margin-bottom: 0.5rem;
	}
	.toolset-entry__img img:last-child {
		position: absolute;
		top: 80px;
		left: 0;
		opacity: 0;
		transition: top 0.5s ease, opacity 0.5s ease;
	}
	.toolset-entry__img img.intersecting {
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
