<script>
	import { onMount } from 'svelte';
	import { fade } from 'svelte/transition';
	import { quadOut } from 'svelte/easing';
	import { layoutBGWhite } from '../../layout-bg';

	import Card from '../../components/Card.svelte';
	import ProblemSolution from '../../components/ProblemSolution.svelte';
	import Observer from '../../components/Observer.svelte';
	import ConceptEntry from '../../components/ConceptEntry.svelte';
	import Sidebar from '../../components/Sidebar.svelte';
	import ReturnToTop from '../../components/ReturnToTop.svelte';

	import volumetric01 from '../../assets/ireti/volumetric-01.jpg';
	import zoo01 from '../../assets/ireti/zoopraxiscope-01.jpg';
	import zoo02 from '../../assets/ireti/zoopraxiscope-02.jpg';
	import tunnel01 from '../../assets/ireti/tunnel-01.jpg';
	import podium01 from '../../assets/ireti/podium-01.jpg';
	import podium02 from '../../assets/ireti/podium-02.jpg';
	import desert01 from '../../assets/ireti/desert-01.jpg';
	import desert02 from '../../assets/ireti/desert-02.jpg';
	import elisaroom01 from '../../assets/ireti/elisa-room-01.jpg';
	import damianroom01 from '../../assets/ireti/damian-room-01.jpg';
	import eyes01 from '../../assets/ireti/eyes-01.jpg';
	import questTest from '../../assets/ireti/quest-test.mp4';
	import helmetCry from '../../assets/ireti/helmet-cry.mp4';
	import glitch from '../../assets/ireti/glitch.mp4';
	import desertSun from '../../assets/ireti/desert-sun.mp4';
	// import splash from '../../assets/ireti/splash.png';
	// import home from '../../assets/ireti/home.png';
	// import passHistory from '../../assets/ireti/pass-history.png';
	// import listCropped from '../../assets/ireti/list-cropped.gif';
	// import problemDefinition from '../../assets/ireti/CS 6023 Problem Definition.jpg';
	// import affinityMap from '../../assets/ireti/R2 Affinity Map.png';
	// import journeyMap from '../../assets/ireti/R2 Workshop Current State Map_CROPPED.png';
	// import journeyMapFinal from '../../assets/ireti/R2 WIP Final Current State Map.png';
	// import qualtrics from '../../assets/ireti/Qualtrics.png';
	// import journeyMapZoom from '../../assets/ireti/R2 Workshop Current State Map_ZOM.png';
	// import sketches from '../../assets/ireti/Sketches User Feedback Sessions.png';
	// import wireframes from '../../assets/ireti/Wireframes User Feedback Sessions.png';
	// import figma from '../../assets/ireti/Figma1.png';
	// import susඞ from '../../assets/ireti/R4 Compiled SUS.png';

	onMount(() => {
		layoutBGWhite.set(true);
	});

	let color = '#DF2B00';
	// let color = '#008856';
	let activeColor = '#A72000';

	let y = 0;

	/**
	 * @type {{text: string, anchor: string}[]}
	 */
	let sections = [];
	let currentSection = '#intro';

	onMount(() => {
		let documentSections = [...document.querySelectorAll('section')];
		sections = [...documentSections].map((section) => ({
			text: section.querySelector('h2')?.innerText || 'Intro',
			anchor: '#' + section.id
		}));

		/**
		 * @type {{[key: string]: boolean}}
		 */
		let seen = {};
		sections = sections.filter((item) => {
			return seen.hasOwnProperty(item.anchor) ? false : (seen[item.anchor] = true);
		});

		let options = {
			rootMargin: '-40% 0px -50% 0px',
			threshold: 0.0
		};

		if (typeof IntersectionObserver !== 'undefined') {
			const observer = new IntersectionObserver((entries) => {
				entries.forEach((entry) => {
					if (entry.isIntersecting) {
						currentSection = '#' + entry.target.id || '#intro';
					}
				});
			}, options);
			documentSections.forEach((e) => observer.observe(e));
		}
		let images = document.querySelectorAll('img.zoomable');
		images.forEach((img) => {
			img.addEventListener('click', (event) => {
				if (!event || !event.target) return;
				imgPreview = event.target.src;
				if (event.target.className.includes('white')) {
					imgPreviewBG = '#fff';
				} else imgPreviewBG = '';
			});
		});
	});

	function handleHashChange() {
		currentSection = location.hash;
	}

	let imgPreview = '';
	let imgPreviewBG = '';
</script>

<svelte:head>
	<title>Ireti — Portfolio</title>
	<meta property="og:type" content="article" />
</svelte:head>

<svelte:window bind:scrollY={y} on:hashchange={handleHashChange} />

<main style={`--article-color: ${color}; --article-active-color: ${activeColor};`}>
	{#if imgPreview}
		<div
			class="image-preview"
			transition:fade={{ duration: 150 }}
			on:click={() => (imgPreview = '')}
		>
			<img src={imgPreview} alt="" style={`background: ${imgPreviewBG || 'none'}`} />
		</div>
	{/if}
	<div class="sidebar">
		{#if sections.length > 0}
			<div class="sidebar-container" in:fade={{ duration: 200 }}>
				<Sidebar {sections} {currentSection} />
			</div>
		{/if}
	</div>
	<!-- svelte-ignore component-name-lowercase -->
	<header>
		<div class="title-card" style={`top: ${y * 0.3}px`}>
			<div class="title-bg" style={`background-image: url(${volumetric01}); `}>
				<!-- <div class="main-img" style={`background-image: url(${titleImg});`} /> -->
				<h1><span> Ireti </span></h1>
			</div>
		</div>
	</header>
	<article>
		{#if y > 500}
			<div
				class="return"
				in:fade|local={{ duration: 200, easing: quadOut }}
				out:fade|local={{ duration: 200, easing: quadOut }}
			>
				<ReturnToTop />
			</div>
		{/if}
		<section id="intro">
			<div class="container bg top">
				<div class="intro">
					<div class="intro__content">
						<p>
							<em>Ireti</em> is an experimental VR film written and directed by Anglik Laboy that uses
							the affordances of the medium to explore the life of a queer college student.
						</p>
						<p>
							This projects is a collage of media forms intgrating 2D and volumetric footage with
							photogrammetry, hand-modeled assets, and hand-designed shader VFX. It was really
							exiting to work with all these forms and tackle the challenge of piecing them all
							together.
						</p>
					</div>
					<div class="info">
						<div class="info__entry">
							<h4>Timeframe</h4>
							<ul>
								<li>
									<span style="white-space: nowrap;">August 2021</span> -
									<span style="white-space: nowrap;">February 2022</span>
								</li>
							</ul>
						</div>
						<div class="info__entry">
							<h4>Role</h4>
							<ul>
								<li>VR Team Lead</li>
							</ul>
						</div>
						<div class="info__entry">
							<h4>Tools</h4>
							<ul>
								<li>Unity</li>
								<li>RealityCapture</li>
								<li>Blender</li>
								<li>Shadergraph</li>
								<li>Oculus VR</li>
							</ul>
						</div>
						<div class="info__entry">
							<h4>Tags</h4>
							<ul>
								<li>VR</li>
								<li>Unity Dev</li>
							</ul>
						</div>
					</div>
				</div>
			</div>
		</section>
		<section id="film-stills">
			<div class="container">
				<h2><a href="#film-stills">Film Stills</a></h2>
				<div class="img-cols">
					<img src={zoo01} alt="" class="zoomable" />
				</div>
				<div class="img-cols">
					<img src={podium01} alt="" class="zoomable" />
					<img src={desert01} alt="" class="zoomable" />
				</div>
				<div class="img-cols">
					<img src={elisaroom01} alt="" class="zoomable" />
				</div>
				<div class="img-cols">
					<img src={eyes01} alt="" class="zoomable" />
					<img src={damianroom01} alt="" class="zoomable" />
				</div>
			</div>
		</section>
		<section id="my-role">
			<div class="container">
				<h2><a href="#my-role">My Role</a></h2>
				<p>
					My role in the project was to lead the VR development team, which meant I was in charge
					of:
				</p>
				<ol>
					<li>Coordinating with the writer/director realize her scenes in VR</li>
					<li>Coordinating with the Animation Lead to integrate her team's work</li>
					<li>Developing the core systems necessary to run the film in VR</li>
					<li>Managing the project timeline and assigning tasks to other developers on my team</li>
					<li>
						Producing several significant effect sequences for different scenes, including code,
						shaders, models, and other art assets
					</li>
				</ol>
			</div>
		</section>
		<section id="development">
			<div class="container">
				<h2><a href="#development">Development</a></h2>
				<p>
					Here's a bunch of media I captured throughout the development of <em>Ireti</em>.
				</p>
				<div class="img-cols">
					<video src={questTest} alt="" style="max-height: 500px" controls />
				</div>
				<div class="img-description">
					Early prototype showing our test room with different interactions
				</div>
				<div class="img-cols">
					<video
						src={glitch + '#t=0.5'}
						alt=""
						style="max-height: 500px"
						preload="metadata"
						controls
					/>
				</div>
				<div class="img-description">
					In-editor view of the glitch controller, allowing the director easily key the complex
					shader effect
				</div>
				<div class="img-cols">
					<video
						src={desertSun + '#t=0.5'}
						alt=""
						style="max-height: 500px"
						preload="metadata"
						controls
					/>
				</div>
				<div class="img-description">
					In-editor view of the sun controller, similarly controlling a complex shader effect
				</div>
				<div class="img-cols">
					<video
						src={helmetCry + '#t=0.5'}
						alt=""
						style="max-height: 500px"
						preload="metadata"
						controls
					/>
				</div>
				<div class="img-description">
					To avoid water simulation in real-time, I built a water shader that fills up this helmet
				</div>
			</div>
		</section>
	</article>
</main>

<style lang="scss">
	video {
		background: #000;
		border-radius: 00.25rem;
	}
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
	.question-answer {
		display: flex;
		flex-wrap: wrap;
		max-width: 760px;
		gap: 0 0.75rem;
		.question {
			flex: 2;
			min-width: 270px;
		}
		.answer {
			align-self: center;
			padding-bottom: 0.5rem;
			flex: 3;
			min-width: 270px;
		}
	}
	p.callout {
		padding: 0.75rem;
		border-radius: 0.5rem;
		background: rgba(0, 0, 0, 0.02);
		// font-weight: ;
	}
	::-moz-selection {
		background: #f8d613;
	}

	::selection {
		background: #f8d613;
	}
	.image-preview {
		position: fixed;
		background: #0003;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		z-index: 3000;
		display: grid;
		align-items: center;
		justify-items: center;
		cursor: -moz-zoom-out;
		cursor: -webkit-zoom-out;
		cursor: zoom-out;
		img {
			width: 90vw;
			height: 90vh;
			object-fit: contain;
			padding: 20px;
		}
		&:after {
			content: '';
			position: absolute;
			top: 20px;
			right: 20px;
			width: 20px;
			height: 1px;
			background: var(--black);
			transform: rotate(45deg);
		}
		&:before {
			content: '';
			position: absolute;
			top: 20px;
			right: 20px;
			width: 20px;
			height: 1px;
			background: var(--black);
			transform: rotate(-45deg);
		}
	}

	.demo {
		position: fixed;
		background: #0003;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		z-index: 1000;
		display: grid;
		align-items: center;
		justify-items: center;
		cursor: -moz-zoom-out;
		cursor: -webkit-zoom-out;
		cursor: zoom-out;
		iframe {
			width: 90vw;
			height: 90vh;
			object-fit: contain;
			padding: 20px;
		}
		&:after {
			content: '';
			position: absolute;
			top: 20px;
			right: 20px;
			width: 20px;
			height: 1px;
			background: var(--black);
			transform: rotate(45deg);
		}
		&:before {
			content: '';
			position: absolute;
			top: 20px;
			right: 20px;
			width: 20px;
			height: 1px;
			background: var(--black);
			transform: rotate(-45deg);
		}
	}

	img.zoomable {
		cursor: -moz-zoom-in;
		cursor: -webkit-zoom-in;
		cursor: zoom-in;
	}
	.img-cols {
		display: flex;
		flex-wrap: wrap;
		margin: 0.25rem 0;
		gap: 0.25rem;
		& > * {
			flex: 1;
			min-width: 200px;
		}
		img {
			border-radius: 0.25rem;
		}
	}
	main {
		display: grid;
		grid-template-areas:
			'. header header'
			'sidebar content content';
		grid-template-columns: minmax(auto, 1fr) minmax(auto, 68rem) minmax(auto, 1fr);
		gap: 0 2rem;
		animation: fade-in 0.3s ease;
	}
	header {
		grid-area: header;
		max-height: 660px;
		height: 40vh;
	}
	.title-card {
		width: 100%;
		height: 100%;
		position: relative;
		.title-bg {
			width: 100%;
			height: 100%;
			background-position: center;
			background-size: cover;
			border-bottom-left-radius: 0.5rem;
		}
		.main-img {
			position: relative;
			width: 100%;
			height: 100%;
			max-width: 64rem;
			background-position: center;
			background-size: cover;
			animation: move-in 0.3s ease;
		}
		h1 {
			position: absolute;
			// top: 40%;
			bottom: 10%;
			left: -4rem;
			width: 90%;
			max-width: 640px;
			font-family: var(--font-serif);
			font-weight: bold;
			font-style: italic;
			font-size: 3.5em;
			line-height: 1.5;
			span {
				background: var(--white);
				padding: 1rem;
				border-radius: 0.25rem;
				white-space: pre-wrap;
				box-shadow: 12px 12px var(--article-color);
				// box-shadow: -16px 0 0 #fff;
			}
		}
	}
	article {
		grid-area: content;
		position: relative;
		z-index: 1;
		// &:after {
		// 	content: '';
		// 	width: 0.5rem;
		// 	height: 0.5rem;
		// 	position: absolute;
		// 	left: 0;
		// 	top: -0.5rem;
		// 	background: radial-gradient(circle at 100% 0, #fff0 0.5rem, #fff 0px);
		// 	z-index: 10;
		// }
	}
	.container > img {
		border-radius: 0.25rem;
	}
	section {
		position: relative;
		&:last-child {
			padding-bottom: 50vh !important;
		}
	}
	.img-description {
		font-size: 0.8em;
		font-style: italic;
	}
	.sidebar {
		grid-area: sidebar;
		justify-self: flex-end;
		width: 120px;
		padding-left: 0.75rem;
		padding-top: 2rem;
	}
	.sidebar-container {
		position: sticky;
		top: 0.5rem;
		z-index: 100;
	}
	.n {
		margin-left: 0.25rem;
		font-size: 0.75em;
		padding: 2px 4px;
		position: relative;
		bottom: 2px;
		font-weight: normal;
		background: var(--black);
		color: var(--white);
		border-radius: 0.25rem;
	}
	.container {
		// padding: 2rem 0;
		margin: 2rem 0;
		padding-right: 2rem;
		max-width: 64rem;
		position: relative;
		&.top {
			border-top-right-radius: 0.5rem;
			padding-top: 2rem;
			margin-top: 0;
			&:after {
				position: absolute;
				top: 0;
				// top: -0.5rem;
				left: -5rem;
				bottom: 0;
				width: 5rem;
				content: '';
				background: var(--white);
			}
		}
		&.bg {
			background: var(--white);
		}
	}
	#research {
		padding-top: 0.5rem;
		padding-bottom: 0.5rem;
	}
	#evaluation {
		padding-top: 0.5rem;
		padding-bottom: 0.5rem;
	}
	#research:before {
		position: absolute;
		z-index: -1;
		left: -50vw;
		top: 0;
		right: 0;
		bottom: 0;
		content: '';
		background-image: radial-gradient(circle, rgba(0, 0, 0, 0.1) 1px, rgba(0, 0, 0, 0.02) 1px);
		background-size: 40px 40px;
		background-attachment: fixed;
	}
	#evaluation:before {
		position: absolute;
		z-index: -1;
		left: -50vw;
		top: 0;
		right: 0;
		bottom: 0;
		content: '';
		background-size: 80px 80px;
		background-image: linear-gradient(to right, rgba(0, 0, 0, 0.05) 1px, rgba(0, 0, 0, 0.01) 1px),
			linear-gradient(to bottom, rgba(0, 0, 0, 0.05) 1px, rgba(0, 0, 0, 0.01) 1px);
		background-position: 20px;
		background-attachment: fixed;
	}

	a {
		text-decoration: none;
		color: var(--article-color);
		fill: var(--article-color);
	}
	a:hover {
		text-decoration: underline;
		text-decoration-thickness: 0.075em;
	}
	h2 {
		font-size: 40px;
		font-weight: 500;
		&::before {
			content: '#';
			margin-right: 4px;
			color: var(--grey);
			font-weight: 500;
		}
		a {
			color: var(--black);
		}
	}
	p,
	ol,
	ul {
		max-width: 760px;
		box-sizing: border-box;
	}
	.intro {
		display: flex;
		flex-wrap: wrap-reverse;
		// grid-template-columns: minmax(300px, 3fr) 2fr;
		gap: 2rem;
		.intro__content {
			min-width: 340px;
			flex: 3;
			& > p {
				margin-top: 0;
			}
		}
		.info {
			flex: 2;
		}
	}
	.info {
		columns: 2;
		-webkit-column-width: 140px;
		-moz-column-width: 140px;
		column-width: 140px;
		// display: flex;
		// flex-wrap: wrap;
		font-size: 0.8em;
		gap: 0.5rem;
		.info__entry {
			flex: 1;
			min-width: 140px;
			margin-bottom: 0.75rem;
			break-inside: avoid;
			h4 {
				// font-weight: 600;
				font-size: 1.25em;
				margin-bottom: 6px;
			}
			ul {
				list-style: none;
				padding: 0;
				margin: 0;
			}
			li {
				margin-bottom: 0.25rem;
			}
		}
	}
	.lean-ux {
		display: flex;
		flex-wrap: wrap;
		gap: 20px;
		margin-bottom: 1rem;
		margin-top: 2rem;
		& > *:first-child {
			min-width: 300px;
			flex: 2;
			align-self: flex-start;
			position: sticky;
			top: 1rem;
		}
		& > *:last-child {
			flex: 3;
			min-width: 400px;
		}
		p {
			margin-top: 0;
		}
		img {
			border-radius: 0.25rem;
			box-shadow: var(--box-shadow-default);
		}
	}
	.research-results {
		display: flex;
		flex-wrap: wrap;
		gap: 20px;
		margin-bottom: 1rem;
		& > *:first-child {
			min-width: 300px;
			flex: 2;
		}
		& > *:last-child {
			flex: 3;
			min-width: 400px;
		}
		p {
			margin-top: 0;
		}
		h3 {
			margin-top: 0;
		}
	}
	.card-list {
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
		gap: 1rem;
		flex-wrap: wrap;
		& > :global(*) {
			flex: 1;
		}
		&.ideas {
			grid-template-columns: repeat(auto-fill, minmax(440px, 1fr));
		}
	}
	.fade-container {
		position: relative;
		opacity: 0;
		top: 60px;
		transition: opacity 0.4s ease, top 0.4s ease;

		&.intersecting {
			opacity: 1;
			top: 0;
		}
	}
	.map-container {
		position: relative;
		&.intersecting img:last-child {
			opacity: 1;
		}
		&.intersecting img:first-child {
			opacity: 0;
		}
		img {
			transition: opacity 0.5s ease;
		}
		img:last-child {
			position: absolute;
			top: 0;
			left: 0;
			opacity: 0;
		}
	}

	@media screen and (max-width: 912px) {
		.lean-ux > *:first-child {
			position: static;
		}
	}

	@media screen and (max-width: 780px) {
		// section {
		// 	padding: 1rem 1rem 1rem 0.5rem;
		// }
		// .sidebar {
		// 	top: 1rem;
		// }
		// section {
		// 	grid-template-columns: 0 auto auto;
		// }
		main {
			font-size: 18px;
		}
		.sidebar {
			display: none;
		}
		.container {
			padding-right: 1rem;
		}
		main {
			gap: 1rem;
		}
		.title-card h1 {
			left: -0.5rem;
		}
	}
	@media screen and (max-width: 625px) {
		.info {
			columns: 3;
		}
	}
	@media screen and (max-width: 660px) {
		.research-results {
			flex-direction: column;
			& > *:first-child {
				min-width: auto;
			}
			& > *:last-child {
				flex: 3;
				min-width: auto;
			}
		}
		.lean-ux {
			flex-direction: column;
			& > *:first-child {
				min-width: auto;
				position: static;
			}
			& > *:last-child {
				flex: 3;
				min-width: auto;
			}
		}

		.card-list.ideas {
			grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
		}
	}
	@media screen and (max-width: 480px) {
		main {
			font-size: 1rem;
		}
		.title-card h1 {
			font-size: 2.5em;
			line-height: 1.4;
			span {
				padding: 0.5rem;
			}
		}
		.info .info__entry {
			min-width: 110px;
		}
	}

	@keyframes fade-in {
		from {
			opacity: 0;
		}
		to {
			opacity: 1;
		}
	}

	@keyframes move-in {
		from {
			top: 60px;
		}
		to {
			top: 0;
		}
	}
</style>
