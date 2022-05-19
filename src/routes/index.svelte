<!-- <script context="module">
	import {
		SITE_URL,
		REPO_URL,
		SITE_TITLE,
		SITE_DESCRIPTION,
		DEFAULT_OG_IMAGE,
		MY_TWITTER
	} from '$lib/siteConfig';
	export const prerender = true; // index page is most visited, lets prerender

	// export const prerender = true; // turned off so it refreshes quickly
	export async function load({ params, fetch }) {
		const res = await fetch(`/api/listBlogposts.json`);
		// alternate strategy https://www.davidwparker.com/posts/how-to-make-an-rss-feed-in-sveltekit
		// Object.entries(import.meta.glob('./*.md')).map(async ([path, page]) => {
		if (res.status > 400) {
			return {
				status: res.status,
				error: await res.text()
			};
		}
		const items = await res.json();
		return {
			props: { items },
			maxage: 60 // 1 minute
		};
	}
</script> -->
<script>
	// import { format, parse } from 'date-fns';
	import MainGraphic from '../components/MainGraphic.svelte';
	import ReturnToTop from '../components/ReturnToTop.svelte';
	import { fade } from 'svelte/transition';
	import { quadOut } from 'svelte/easing';

	export let items = { list: [] };
	$: list = items.list;

	let y = 0;
</script>

<!-- <svelte:head>
	<title>{SITE_TITLE}</title>
	<link rel="canonical" href={SITE_URL} />
	<link rel="alternate" type="application/rss+xml" href={SITE_URL + '/api/rss.xml'} />
	<meta property="og:url" content={SITE_URL} />
	<meta property="og:type" content="article" />
	<meta property="og:title" content={SITE_TITLE} />
	<meta name="Description" content={SITE_DESCRIPTION} />
	<meta property="og:description" content={SITE_DESCRIPTION} />
	<meta property="og:image" content={DEFAULT_OG_IMAGE} />
	<meta name="twitter:card" content="summary" />
	<meta name="twitter:creator" content={MY_TWITTER} />
	<meta name="twitter:title" content={SITE_TITLE} />
	<meta name="twitter:description" content={SITE_DESCRIPTION} />
	<meta name="twitter:image" content={DEFAULT_OG_IMAGE} />
</svelte:head> -->

<svelte:window bind:scrollY={y} />

<section>
	{#if y > 500}
		<div
			style="position: fixed; bottom: 20px; right: 20px; z-index: 1000;
		backdrop-filter: blur(10px); 
		-webkit-backdrop-filter: blur(10px);"
			in:fade={{ duration: 200, easing: quadOut }}
			out:fade={{ duration: 200, easing: quadOut }}
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
				<p style="margin:0; font-size: 4rem; font-weight: bold;">Hi, I'm John</p>
			</div>
			<div class="about__content__item">
				<p style="">I'm a designer, user researcher, and web developer located in Atlanta, GA.</p>
			</div>
		</div>
	</div>
	<div id="work" class="flex justify-center">
		<div class="grid p-4 gap-4 max-w-5xl w-full">
			<!-- <h2 class="inline text-6xl font-bold my-6 col-span-2">Work</h2> -->
			<!-- <h2
				class="inline text-8xl font-serif italic uppercase font-bold self-center text-center my-6 col-span-2"
			>
				Work
			</h2> -->
			{#each list as item}
				<a sveltekit:prefetch class="work-item flex flex-col cursor-pointer" href={item.slug}>
					<div class="grid overflow-hidden h-96 rounded-md mb-2">
						<div
							class="image bg-cover bg-center"
							style={`background-image: url(${item.data.img})`}
						/>
					</div>
					<h2 class="text-4xl italic font-serif font-black text-zinc-800 mb-1">{item.title}</h2>
					<div class="flex items-center gap-2 text-zinc-800 text-mono">
						<div>
							<!-- {format(parse(item.data.date, 'yyyy-MM-dd', new Date()), 'yyyy')} -->
						</div>
						<div class="w-1 h-1 rounded bg-zinc-800" />
						<div>{item.data.type}</div>
						<div class="w-1 h-1 rounded bg-zinc-800" />
						<div>{item.data.result}</div>
					</div>
				</a>
			{/each}
		</div>
	</div>
</section>

<style>
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
	}
	.graphic__align > :global(.main-graphic) {
		height: 100vh;
	}
	#about {
		margin: 0 auto;
		margin-top: 110vh;
		margin-bottom: 200px;
		max-width: 64rem;
	}
	.about__content {
		display: flex;
		flex-wrap: wrap;
		padding: 4rem;
		justify-content: center;
		gap: 8rem;
	}
	#work > div {
		grid-template-columns: repeat(auto-fit, minmax(324px, 1fr));
	}
	.about__content__item {
		min-width: 324px;
		flex: 1;
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
