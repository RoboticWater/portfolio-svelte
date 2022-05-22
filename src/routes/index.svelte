<script>
	import MainGraphic from '../components/MainGraphic.svelte';
	import ReturnToTop from '../components/ReturnToTop.svelte';
	import Observer from '../components/Observer.svelte';
	import { fade } from 'svelte/transition';
	import { quadOut } from 'svelte/easing';
	import { layoutBGWhite } from '../layout-bg';
	import { onMount } from 'svelte';

	import whiteboardingImg from '../assets/whiteboarding/main-image.png';

	onMount(() => {
		layoutBGWhite.set(false);
	});

	let list = [
		{
			title: 'Participatory Whiteboarding',
			slug: '/work/whiteboarding',
			date: '2022',
			type: 'UX Design',
			result: 'Figma Prototype',
			image: whiteboardingImg
		},
		{
			title: 'WhatsApp for SMBs',
			slug: '/work/whatsapp',
			date: '2021',
			type: 'UX Design',
			result: 'Balsamiq Prototype',
			image:
				'https://images.unsplash.com/photo-1614680376408-81e91ffe3db7?crop=entropy&cs=tinysrgb&fm=jpg&ixlib=rb-1.2.1&q=80&raw_url=true&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1548'
		}
	];

	let y = 0;
</script>

<svelte:head>
	<title>John Britti — Portfolio</title>
</svelte:head>

<svelte:window bind:scrollY={y} />

<section>
	{#if y > 500}
		<div
			class="return"
			in:fade|local={{ duration: 200, easing: quadOut }}
			out:fade|local={{ duration: 200, easing: quadOut }}
		>
			<ReturnToTop />
		</div>
	{/if}
	<div class="graphic__container">
		<div class="graphic__align"><MainGraphic /></div>
	</div>
	<div id="about">
		<div class="about__content">
			<div class="about__content__item">
				<p class="name">Hi, I'm John</p>
			</div>
			<div class="about__content__item">
				<p>I'm a designer, user researcher, and web developer located in Atlanta, GA.</p>
				<p>I do a lot of different stuff.</p>
			</div>
		</div>

		<Observer let:intersecting rootMargin="0% 0% -15%" once>
			<div class="description-blocks" class:intersecting>
				<div class="description-block">
					<div class="number">1</div>
					<h2>Expert Design</h2>
					<div class="description-block__content">
						I have experience with a multitude of modern design tools, 2D and 3D, static and
						animated, and I’m excited to use those skills to make beautiful visuals and compelling
						interactions.
					</div>
				</div>
				<div class="description-block" style="transition-delay: 150ms">
					<div class="number">2</div>
					<h2>Professional Research</h2>
					<div class="description-block__content">
						I’m trained in the full user research process: finding data and making it work for
						anything from strategy to interface design.
					</div>
				</div>
				<div class="description-block" style="transition-delay: 300ms">
					<div class="number">3</div>
					<h2>Experienced Development</h2>
					<div class="description-block__content">
						I’ve been developing websites since high school and I bring a hacker mentality to all my
						work, developing the tools and prototypes I need to realize my designs.
					</div>
				</div>
				<div class="description-block" style="transition-delay: 450ms">
					<div class="number">4</div>
					<h2>Skilled Communication</h2>
					<div class="description-block__content">
						I make things for people, and that means a lot of talking. Fortinately, I’m always
						excited to engage in critical dialogs and learn from users and colleagues alike.
					</div>
				</div>
			</div>
		</Observer>
	</div>
	<div id="work" class="flex justify-center">
		<h1>Selected Work</h1>
		<div class="work-items">
			{#each list as item}
				<a class="work-item" href={item.slug}>
					<div class="image-container">
						<div class="image" style={`background-image: url(${item.image})`} />
					</div>
					<h2>{item.title}</h2>
					<div class="info">
						<div>{item.date}</div>
						<div class="spacer" />
						<div>{item.type}</div>
						<div class="spacer" />
						<div>{item.result}</div>
					</div>
				</a>
			{/each}
		</div>
	</div>
</section>

<style lang="scss">
	.return {
		position: fixed;
		bottom: 20px;
		right: 20px;
		z-index: 1000;
		background: var(--white);
		border-radius: 2px;
		// backdrop-filter: blur(10px);
		// -webkit-backdrop-filter: blur(10px);
	}
	#work {
		padding: 0 1rem;
	}
	.work-items {
		gap: 1rem;
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
	}
	.work-item {
		display: flex;
		flex-direction: column;
		cursor: pointer;
		text-decoration: none;
		.image-container {
			display: grid;
			overflow: hidden;
			height: 20rem;
			border-radius: 0.75rem;
			margin-bottom: 0.5rem;
			.image {
				background-position: center;
				background-size: cover;
				transition: transform 0.3s ease;
			}
		}
		h2 {
			font-style: italic;
			font-family: var(--font-serif);
			color: var(--black);
			font-weight: bold;
			margin: 0 0 2px 0;
			font-size: 2rem;
		}
		&:hover {
			h2 {
				text-decoration: underline;
			}
			.image {
				transform: scale(1.1);
			}
		}
		.info {
			display: flex;
			align-items: center;
			font-family: var(--font-mono);
			gap: 0.5rem;
			font-size: 1rem;
			.spacer {
				width: 0.25rem;
				height: 0.25rem;
				border-radius: 1px;
				background: var(--black);
			}
			div {
				color: var(--black);
			}
		}
	}
	section {
		padding-bottom: 48rem;
		animation: fade-section 800ms cubic-bezier(0.33, 0.43, 0.04, 0.97);
		scroll-behavior: smooth;
	}
	.graphic__container {
		height: 100vh;
		width: 100%;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		z-index: -10;
		overflow: hidden;
		position: fixed;

		pointer-events: none;
	}
	.graphic__align {
		position: relative;
		left: 50%;
		text-align: center;
		width: 10000px;
		margin: 0 0 0 -5000px;
		& > :global(.main-graphic) {
			height: 100vh;
		}
	}
	#about {
		margin: 0 auto;
		margin-top: 110vh;
		margin-bottom: 6rem;
		max-width: 64rem;
		padding: 1rem;
		.name {
			line-height: 1.1;
			margin: 0;
			font-size: 4rem;
			font-weight: bold;
		}
	}
	.about__content {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		gap: 0rem 8rem;
		padding: 3rem;
	}
	#work {
		margin: 0 auto;
		max-width: 64rem;
		h1 {
			font-size: 4rem;
			// text-transform: uppercase;
			font-weight: 700;
		}
	}
	.about__content__item {
		min-width: 324px;
		flex: 1;
	}

	.description-blocks {
		display: flex;
		flex-wrap: wrap;
		gap: 3rem;
		.description-block {
			position: relative;
			flex: 1;
			display: grid;
			min-width: 22rem;
			grid-template-areas: 'number title' 'number content';
			grid-template-rows: auto 1fr;
			grid-template-columns: auto 1fr;
			gap: 0 0.5rem;
			opacity: 0;
			top: 60px;
			transition: opacity 0.4s ease, top 0.4s ease, left 0.4s ease;
			// &:nth-child(odd) {
			// 	left: -60px;
			// }
			// &:nth-child(even) {
			// 	left: 60px;
			// }
			.number {
				grid-area: number;
				font-size: 1.5rem;
				font-weight: 500;
				width: 3rem;
				height: 3rem;
				border-radius: 2px;
				border: var(--black) solid 1px;
				display: grid;
				align-content: center;
				justify-content: center;
			}
			h2 {
				grid-area: title;
				margin: 0;
				font-size: 1.5rem;
			}
			.description-block__content {
				grid-area: content;
				font-size: 1.25rem;
				align-self: flex-start;
			}
		}
		&.intersecting .description-block {
			opacity: 1;
			top: 0;
			left: 0;
		}
	}
	@media screen and (max-width: 500px) {
		section {
			font-size: 0.8em;
		}
		.description-blocks {
			gap: 1rem;
			.description-block {
				min-width: 340px;
				.number {
					font-size: 1rem;
					width: 2rem;
					height: 2rem;
				}
				h2 {
					font-size: 1.25rem;
				}
				.description-block__content {
					font-size: 1rem;
				}
			}
		}
	}
	@keyframes fade-section {
		from {
			opacity: 0;
		}
		to {
			opacity: 1;
		}
	}
</style>
