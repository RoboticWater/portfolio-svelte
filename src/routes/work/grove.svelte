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

	import timeline from '../../assets/grove/Timeline.png';
	import splash from '../../assets/grove/splash.png';
	import home from '../../assets/grove/home.png';
	import passHistory from '../../assets/grove/pass-history.png';
	import listCropped from '../../assets/grove/list-cropped.gif';
	import problemDefinition from '../../assets/grove/CS 6023 Problem Definition.jpg';
	import affinityMap from '../../assets/grove/R2 Affinity Map.png';
	import journeyMap from '../../assets/grove/R2 Workshop Current State Map_CROPPED.png';
	import journeyMapFinal from '../../assets/grove/R2 WIP Final Current State Map.png';
	import qualtrics from '../../assets/grove/Qualtrics.png';
	import journeyMapZoom from '../../assets/grove/R2 Workshop Current State Map_ZOM.png';
	import sketches from '../../assets/grove/Sketches User Feedback Sessions.png';
	import wireframes from '../../assets/grove/Wireframes User Feedback Sessions.png';
	import figma from '../../assets/grove/Figma1.png';
	import susඞ from '../../assets/grove/R4 Compiled SUS.png';
	// import problemMap from '../../assets/grove/problem-map.png';
	// import journeyMap from '../../assets/grove/journey-map.png';
	// import interviewImages from '../../assets/grove/interview-images.png';
	// import personalEval from '../../assets/grove/personal-eval.png';
	// import contactAnnotation from '../../assets/grove/contact-annotation.png';
	// import queue from '../../assets/grove/queue.png';

	onMount(() => {
		layoutBGWhite.set(true);
	});

	let color = '#3C39BB';
	// let color = '#008856';
	let activeColor = '#18158B';

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
	<title>Grove — Portfolio</title>
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
			<div
				class="title-bg"
				style={`background-image: url(https://images.unsplash.com/photo-1512621776951-a57141f2eefd?crop=entropy&cs=tinysrgb&fm=jpg&ixlib=rb-1.2.1&q=80&raw_url=true&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2070); `}
			>
				<!-- <div class="main-img" style={`background-image: url(${titleImg});`} /> -->
				<h1><span> Grove </span></h1>
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
				<ProblemSolution>
					<span slot="problem-title">Problem Space</span>
					<span slot="problem">
						What can we help people discover nutritional information on the go?
					</span>
					<span slot="solution-title">Solution</span>
					<span slot="solution">
						A mobile app that helps vegans and vegetarians discover the food that satisfies their
						preferences.
					</span>
				</ProblemSolution>
			</div>
			<div class="container bg">
				<div class="intro">
					<div class="intro__content">
						<p>
							<em>Grove</em> is a mobile app that combines familiar mapping features with clear
							iconography and a notification system that helps vegans and vegetarians discover the
							food that satisfies their preferences. Link to final protype
							<a
								href="https://www.figma.com/proto/YlC9K9igrXxl7C6HIzDt87/Prototype?node-id=69%3A3383&scaling=scale-down"
								target="_blank"
								style="margin: 0;">here</a
							>.
						</p>
					</div>
					<div class="info">
						<div class="info__entry">
							<h4>Timeframe</h4>
							<ul>
								<li>August 2020 - December 2020</li>
							</ul>
						</div>
						<div class="info__entry">
							<h4>Team</h4>
							<ul>
								<li>Gabriel Britain</li>
								<li>John Britti</li>
								<li>Phoebe Tan</li>
								<li>Wenrui Zhang</li>
							</ul>
						</div>
						<div class="info__entry">
							<h4>Tools</h4>
							<ul>
								<li>User Interviews</li>
								<li>Affinity Mapping</li>
								<li>Journey Mapping</li>
								<li>Survey</li>
								<li>Figma</li>
							</ul>
						</div>
						<div class="info__entry">
							<h4>Tags</h4>
							<ul>
								<li>UX Research</li>
								<li>Mobile Interface</li>
							</ul>
						</div>
					</div>
				</div>
			</div>
		</section>
		<section id="final-concepts">
			<div class="container">
				<h2><a href="#final-concepts">Final Concepts</a></h2>
				<p>
					<em>Grove </em>is a mobile app that shows all nearby restaurants that have vegan and
					vegetarian options. &quot;Options&quot; is critical here, because
					<em>Grove </em>doesn’t exclude restaurants that aren’t explicitly vegan or vegetarian, and
					instead displays the composition of each restaurant’s menu, so users can be confident that
					they can get a decent meal even at places that aren’t exclusively catering to their
					dietary preference.
				</p>
				<div class="img-cols" style="max-width: 760px;">
					<img src={splash} alt="" class="zoomable" />
					<img src={home} alt="" class="zoomable" />
					<img src={passHistory} alt="" class="zoomable" />
				</div>
				<p>
					<em>Grove</em> is designed to be clean, yet information dense. Our research suggested that
					our users like to look at a restaurant&#x27;s menu before ordering, often at pictures of the
					menu items themselves, so we placed as much of that information as close to the surface of
					the interface as possible without sacrificing readability, and provided users with robust filtering
					functionality. We also made menu legibility a priority, clearly denoting vegan or vegetarian
					dishes with distinct iconography.
				</p>
				<div class="img-cols" style="max-width: 760px;">
					<img
						src={listCropped}
						alt=""
						class="zoomable"
						style="max-height: 510px; width: auto; flex:0;"
					/>
				</div>
				<p>
					Finally, <em>Grove</em> has an automated newsletter system, providing our users notifications
					on the restaurants they&#x27;ve passed by on a weekly (customizable) basis, helping them expand
					their horizons.
				</p>
			</div>
		</section>
		<section id="timeline">
			<div class="container">
				<h2><a href="#timeline">Timeline</a></h2>
				<p>
					This project was structured into roughly 3 stages: research, design, and evaluation, with
					a heavy emphasis on on research.
				</p>
				<img src={timeline} alt="" class="zoomable" style="max-width: 760px" />
			</div>
		</section>
		<section id="problem-focus">
			<div class="container">
				<h2><a href="#problem-focus">Problem Focus</a></h2>
				<p>
					This project began with a broad prompt: help people discover nutritional information on
					the go, but before we could begin designing, we needed to thoroughly define who we were
					trying to help and what needs of theirs weren&#x27;t being met. We knew this meant we
					would need to restrict our scope from the initial problem statement.
				</p>
				<p>
					&quot;Nutrition on the go&quot; was simply too broad a topic to adequately define a user
					population, so we explored different stakeholders and contexts where nutrition is an
					issue.
				</p>
				<img src={problemDefinition} alt="" class="zoomable" style="max-width: 760px" />
				<div class="img-description">Our problem focus brainstorming board</div>
				<p>
					Ultimately we decided to focus our project on vegans and vegetarians, because people
					within our social circles and market research suggested that this kind of diet was
					growing, yet still had plenty of room for innovation.
				</p>
			</div>
		</section>
		<section id="research">
			<div class="container">
				<h2><a href="#research">Research</a></h2>
				<h3>🔎 Discovery</h3>
				<div class="card-list" style="display: flex; flex-direction: column; max-width: 760px">
					<Card>
						<span slot="label" />
						<span slot="title">Research Approach #1</span>
						<span slot="content">
							We conducted a literature review, determining the demographic characteristics of our
							user population, exploring academic approaches to our problem space
						</span>
					</Card>
					<Card>
						<span slot="label" />
						<span slot="title">Research Approach #2</span>
						<span slot="content">
							We performed a thorough analysis of existing solutions in the vegan/vegetarian
							nutrition space, pinpointing the benefits these systems already provide, and more
							importantly, what they didn't
						</span>
					</Card>
					<Card>
						<span slot="label" />
						<span slot="title">Research Approach #3</span>
						<span slot="content">
							Finally, and most critically, we held 5 30-60 minute semi-structured interviews with
							our user population, discovering their needs and pain points and then mapping their
							entire journey from finding to eating at a restaurant
						</span>
					</Card>
				</div>
				<img src={affinityMap} alt="" class="zoomable" style="max-width: 760px" />
				<p>
					We then took all of our findings and organized them into user personas and a journey map,
					so we could give our problem space some structure. The former codified the desires,
					abilities, and backgrounds of our users so we could maintain a coherent image of who we
					were developing for, and the latter fleshed out every step in the process of eating out,
					from deciding where to eat to completing the order so we could clearly address every pain
					point.
				</p>
				<img src={journeyMap} alt="" class="zoomable" style="max-width: 760px" />
				<h3>📏 Refinement</h3>
				<p>
					With the journey map in hand, we conducted a second round of interviews, asking
					individuals in our target population to critique our distillation of their experience
					dining out.
				</p>
				<img src={journeyMapFinal} alt="" class="zoomable" style="max-width: 760px" />
				<p>
					While we were confident in our investigation of the problem space, we had yet to find a
					context specific enough to where we could begin the design process. There were simply too
					many pain points throughout the process to choose, so we conducted an online survey to let
					our users decide.
				</p>
				<p>
					We deployed a Qualtrics survey to the r/vegan subreddit and various Georgia Tech social
					media venues to determine which of our pain points were felt the most by our users.
				</p>
				<img src={qualtrics} alt="" class="zoomable" style="max-width: 760px" />
				<p>
					The results of the survey and the journey map critique suggested that our users
					experienced the most potent pain points during the discovery phase, so that&#x27;s what we
					started to design for.
				</p>
				<img src={journeyMapZoom} alt="" class="zoomable" style="max-height: 510px; width: auto;" />
			</div>
		</section>
		<section id="design">
			<div class="container">
				<h2><a href="#design">Design</a></h2>
				<p>
					Our design process was iterative, taking user and expert feedback at each step to refine
					our ideas until they could be realized as a medium fidelity prototype.
				</p>
				<h3>Sketches</h3>
				<p>
					After brainstorming ideas, we decided to move forward with 3. We developed simple
					storyboard sketches for each idea, and presented them to prospective users, asking them to
					identify aspects they liked, disliked, or thought could be developed more.
				</p>
				<img src={sketches} alt="" class="zoomable" style="max-width:760px" />
				<!-- <img src={sketches} alt="" class="zoomable" style="max-height: 510px; width: auto;" /> -->
				<h3>Wireframes</h3>
				<p>
					With the feedback from the sketch workshop, we iterated on each idea and created a rough
					wireframe for each, and performed a task-based analysis with users. Here we discovered
					more technical problems with our concepts, like confusion with visual elements and UI
					flow.
				</p>
				<img src={wireframes} alt="" class="zoomable" style="max-width:760px" />
				<h3>Prototype</h3>
				<p>
					Taking the best qualities from our wireframes, we went to Figma and began to develop a
					medium fidelity prototype. This is where I contributed most to this project, designing the
					app&#x27;s visual identity, and developing most of its functionality.
				</p>
				<img src={figma} alt="" class="zoomable" style="max-width:760px" />
			</div>
		</section>
		<section id="evaluation">
			<div class="container">
				<h2><a href="#evaluation">Evaluation</a></h2>
				<p>
					Finally, we took prototype and subjected it to both usability testing with prospective
					users and heuristic evaluations with experts.
				</p>
				<img src={susඞ} alt="" class="zoomable" style="max-width:760px" />
			</div>
		</section>
	</article>
</main>

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
		& > * {
			flex: 1;
			min-width: 200px;
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
