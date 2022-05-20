<script>
	import { onMount } from 'svelte';
	import { fade } from 'svelte/transition';
	import { quadOut } from 'svelte/easing';
	import { layoutBGWhite } from '../layout-bg';

	import Card from '../components/Card.svelte';
	import ProblemSolution from '../components/ProblemSolution.svelte';
	import Observer from '../components/Observer.svelte';
	import ConceptEntry from '../components/ConceptEntry.svelte';
	import Sidebar from '../components/Sidebar.svelte';
	import ReturnToTop from '../components/ReturnToTop.svelte';

	import header from '../assets/whiteboarding/whiteboarding-bg.png';
	import tripleDiamond from '../assets/whatsapp/triple-diamond.svg';
	import stakeholderMap from '../assets/whatsapp/stakeholder-problem-map.jpg';
	import problemMap from '../assets/whatsapp/problem-map.png';
	import journeyMap from '../assets/whatsapp/journey-map.png';
	import interviewImages from '../assets/whatsapp/interview-images.png';
	import personalEval from '../assets/whatsapp/personal-eval.png';
	import contactAnnotation from '../assets/whatsapp/contact-annotation.png';
	import queue from '../assets/whatsapp/queue.png';

	onMount(() => {
		layoutBGWhite.set(true);
	});

	let color = '#0879CA';
	// let color = '#008856';
	let activeColor = '#0065AD';

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
	<title>WhatsApp for SMBs — Portfolio</title>
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
				style={`background-image: url(https://images.unsplash.com/photo-1614680376408-81e91ffe3db7?crop=entropy&cs=tinysrgb&fm=jpg&ixlib=rb-1.2.1&q=80&raw_url=true&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1548); `}
			>
				<!-- <div class="main-img" style={`background-image: url(${titleImg});`} /> -->
				<h1><span> WhatsApp for SMBs </span></h1>
			</div>
		</div>
	</header>
	<article>
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
		<section id="intro">
			<div class="container bg top">
				<ProblemSolution>
					<span slot="problem-title">Problem Space</span>
					<span slot="problem">
						What can social media apps do to help small to medium sized businesses (SMBs) in ways
						that aren’t advertising?
					</span>
					<span slot="solution-title">Solution</span>
					<span slot="solution">
						<ol>
							<li>
								A distinct user population that we believe is a viable market for social media-based
								solutions
							</li>
							<li>Several pain points that this user population is experiencing</li>
							<li>Potential design solutions to some of these pain points</li>
						</ol>
					</span>
				</ProblemSolution>
			</div>
			<div class="container bg">
				<div class="intro">
					<div class="intro__content">
						<p>
							The COVID pandemic spurred many changes in the business world, most specifically, a
							marked shift towards using digital services for operations and customer interaction.
							Small to medium sized businesses (SMBs) were already going digital, and this was only
							accelerated. Indeed, according to <a
								href="https://about.fb.com/wp-content/uploads/2020/10/Deloitte-Digital-Tools-in-Crisis-and-Recovery-SMB-Report-Oct-2020.pdf"
								target="__blank">Deloitte</a
							> 77% of SMBs started or increased usage of digital tools during the pandemic.
						</p>
						<p>
							Recognizing this shift as an opportunity, a client asked my partner and I to research
							how we might improve social media sites for SMBs to help them adapt. A stipulation
							from our client, however, was that our solution couldn't be advertising-based; he
							works at Facebook's advertising research division, so he wanted something <i>novel</i
							>.
						</p>
						<p>
							Using WhatsApp as a base my team explored opportunities to <b
								>improve the lives of SMBs in India</b
							>
							and developed <b>user-tested low fidelity prototypes</b> designed to
							<b
								>streamline customer interactions and organize business data, while retaining the
								familiarity of our user’s workflow.</b
							>
						</p>
					</div>
					<div class="info">
						<div class="info__entry">
							<h4>Timeframe</h4>
							<ul>
								<li>January 2020 - May 2020</li>
							</ul>
						</div>
						<div class="info__entry">
							<h4>Team</h4>
							<ul>
								<li>Aditi Bhatnagar</li>
								<li>John Britti</li>
							</ul>
						</div>
						<div class="info__entry">
							<h4>Tools</h4>
							<ul>
								<li>User Interviews</li>
								<li>Afinity Mapping</li>
								<li>Journey Mapping</li>
								<li>Survey</li>
								<li>Balsamiq</li>
							</ul>
						</div>
						<div class="info__entry">
							<h4>Tags</h4>
							<ul>
								<li>UX Research</li>
								<li>Social Media</li>
								<li>Mobile Interface</li>
								<li>Balsamiq</li>
							</ul>
						</div>
					</div>
				</div>
			</div>
		</section>
		<section id="final-concepts">
			<div class="container">
				<h2><a href="#final-concepts">Final Concepts</a></h2>
				<div class="toolset">
					<ConceptEntry imgBackground={''} imgForeground={''}>
						<span slot="number">1</span>
						<span slot="title">Contact Annotation</span>
						<span slot="description">
							Small business owners in our user population often keep business information like
							customer orders and preferences strewn around various (often physical) touchpoints.
							This feature allows users to label and keep notes on customer details within WhatsApp
							without disrupting their familiar flow.
						</span>
						<img src={contactAnnotation} slot="image" class="zoomable" alt="" />
					</ConceptEntry>
					<ConceptEntry imgBackground={''} imgForeground={''}>
						<span slot="number">2</span>
						<span slot="title">Queue</span>
						<span slot="description">
							Businesses can miss orders among all their incoming messages, so this feature lets
							users create a message queue for orders so they can isolate and keep track of orders.
						</span>
						<img src={queue} slot="image" class="zoomable" alt="" />
					</ConceptEntry>
				</div>
			</div>
		</section>
		<section id="timeline">
			<div class="container">
				<h2><a href="#timeline">Timeline</a></h2>
				<p>
					Over the three month timeline of this project my partner and I used an augmented <a
						href="https://en.wikipedia.org/wiki/Double_Diamond_(design_process_model)"
						target="__blank"
					>
						double diamond</a
					> approach to the UX research process. Our only change was a third, smaller diamond at the
					beginning of the project where we narrowed down our prompt to something more manageable.
				</p>
				<img src={tripleDiamond} alt="" class="zoomable white" style="max-width: 760px" />
			</div>
		</section>
		<section id="problem-focus">
			<div class="container">
				<h2><a href="#problem-focus">Problem Focus</a></h2>
				<p>
					Like many student projects, the prompt my team was initially given was incredibly broad;
					there are countless social media sites offering different forms of engagement and there
					are an even greater number of small to medium sized businesses offering just about every
					kind of good or service imaginable.
				</p>
				<p>
					So, the teams first line of business was to reduce the scope down to something that was
					manageable and critically, actionable. That meant identifying a social media context and
					user population we were interested in.
				</p>
				<img src={stakeholderMap} alt="" class="zoomable" style="max-width: 760px;" />
				<div class="img-description">A snippet of our Stakeholder/Problem Map board in Mural</div>
				<p>
					Leveraging our connections and a variety of literature sources, we identified several
					potential user populations and social media contexts ripe for a design intervention. We
					ranked these options by interest, feasibility, and population access then pitched the top
					two to our client to get his preference. Thus, we refined or problem statement to:
				</p>
				<p class="callout">
					SMBs in India need a way to optimize WhatsApp for engaging with customers and stakeholders
					as their business continues to grow.
				</p>
			</div>
		</section>
		<section id="research">
			<div class="container">
				<h2><a href="#research">Research</a></h2>
				<h3>🔎 Research Questions</h3>
				<div class="card-list">
					<Card>
						<span slot="label" />
						<span slot="title">Research Question #1</span>
						<span slot="content">
							How is WhatsApp being used by SMBs in our target population?
						</span>
					</Card>
					<Card>
						<span slot="label" />
						<span slot="title">Research Question #2</span>
						<span slot="content">
							Are SMBs experiencing any distinct pain points that could be alleviated?
						</span>
					</Card>
					<Card>
						<span slot="label" />
						<span slot="title">Research Question #3</span>
						<span slot="content">
							Are there aspects of their business that could be streamlined through WhatsApp?
						</span>
					</Card>
				</div>
				<p>
					To structure our research, we developed these 3 research questions which we could come to
					answer throughout our research process.
				</p>
				<div class="research-results">
					<div class="method">
						<h3>💬 Semi-structured Interviews <span class="n">N=8</span></h3>
						<p>
							To start we conducted 2 rounds of interviews with SMBs from a variety of business
							contexts: Caterers, Tutors, Photographers, Fashion Designers.
						</p>
						<p>
							In the first round, with 4 participants, we attempted to understand why SMBs use
							Whatsapp as their primary business communication tool and what problems WhatsApp had
							that we could solve.
						</p>
						<p>
							In the second round, also with 4 participants, we wanted to elicit more contextual
							information and deepen specific insights.
						</p>
						<p>
							After each interview, notes were aggregated and then affinity mapped to find common
							themes.
						</p>
					</div>
					<div class="results">
						<Observer let:intersecting rootMargin="0% 0% -25%" once
							><div class="fade-container" class:intersecting style="transition-delay: 0ms">
								<Card>
									<span slot="label">Interview Insight #1</span>
									<span slot="title">Juggling a variety of touchpoints</span>
									<span slot="content">
										Users juggle different physical and technological tools to capture
										business-related information, which can be a hassle and potentially lose
										information between touchpoints.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 100ms">
								<Card>
									<span slot="label">Interview Insight #2</span>
									<span slot="title">Users balk at complex business tools</span>
									<span slot="content">
										Users are hesitant to adopt business-centered tools due to their perceived
										technical complexity. Many users didn't consider themselves "tech people" and
										some simply preferred the simplicity of their notebooks. Even the existing
										WhatsApp business tools seemed out of reach.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 200ms">
								<Card>
									<span slot="label">Interview Insight #3</span>
									<span slot="title">SMB owners like the ubiquity of WhatsApp</span>
									<span slot="content">
										Users prefer using WhatsApp because it is ubiquitous and places low
										technological demands on them and their customers.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 300ms">
								<Card>
									<span slot="label">Interview Insight #4</span>
									<span slot="title">Users leverage small networks available to them</span>
									<span slot="content">
										Users leverage existing WhatsApp groups like their apartment's message group to
										create awareness about their product or service.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 400ms">
								<Card>
									<span slot="label">Interview Insight #5</span>
									<span slot="title">Our users leverage personal connections</span>
									<span slot="content">
										The customer base for our users consists mainly of friends and family. The
										blurred lines between business and personal usage of WhatsApp can cause
										friction.
									</span>
								</Card>
							</div>
						</Observer>
					</div>
				</div>
				<div class="question-answer">
					<div class="question">
						<Card check>
							<span slot="label" />
							<span slot="title">Research Question #1</span>
							<span slot="content">
								How is WhatsApp being used by SMBs in our target population?
							</span>
						</Card>
					</div>
					<div class="answer">
						By the end of this process, we felt that we had enough information on our users to
						adequately answered our first research question.
					</div>
				</div>
				<h3>🗺️ Problem Mapping</h3>
				<p>
					Since our interview population represented a variety of businesses, we wanted to refine
					our problem space further. To do so, we mapped out all our problems into a grid, hoping to
					identify either a particular set of problems that affected most businesses equally or a
					particular business type that experienced most problems.
				</p>
				<img src={problemMap} alt="" class="zoomable" style="max-width: 760px" />
				<p>Given these results, we focused our problem statement in again:</p>

				<p class="callout">
					SMBs in India need support from Whatsapp in easily creating and sharing menus and managing
					orders.
				</p>
				<p>
					While there were several problems that users experienced across different businesses like
					privacy concerns with WhatsApp now that Facebook had changed its privacy policy or having
					multiple physical touchpoints, there were consistent enough themes with goods caterers
					that we felt confident purusing this population further.
				</p>
				<div class="question-answer">
					<div class="question">
						<Card check>
							<span slot="label" />
							<span slot="title">Research Question #2</span>
							<span slot="content">
								Are SMBs experiencing any distinct pain points that could be alleviated?
							</span>
						</Card>
					</div>
					<div class="answer">
						With these results we now could answer our second research question.
					</div>
				</div>
				<h3>💬 Contextual Interviews <span class="n">N=6</span></h3>
				<p>
					With our focus narrowed in further, we conducted more contextual interviews, focused on
					gaining a greater insight into the moment to moment activities goods caterers perform and
					painpoints they experience. In addition to asking our users questions directly about their
					journey, we asked them to provide pictures from their WhatsApp and physical work spaces,
					which you can see below.
				</p>
				<img src={interviewImages} alt="" class="zoomable" style="max-width: 760px" />
				<p>
					In the next section, you'll see how we organized the journey information, but these images
					really helped us understand what a WhatsApp catering interaction looks like and later how
					we could design interactions that would conform to how our users' expectations.
				</p>
				<h3>🗺️ Journey Mapping</h3>
				<p>
					From those interviews, we constructed this journey map to better understand the process
					our users go through in running their business and identify areas where we could
					intervene.
				</p>
				<img src={journeyMap} alt="" class="zoomable" style="max-width: 760px" />
				<div class="question-answer">
					<div class="question">
						<Card check>
							<span slot="label" />
							<span slot="title">Research Question #3</span>
							<span slot="content">
								Are there aspects of their business that could be streamlined through WhatsApp?
							</span>
						</Card>
					</div>
					<div class="answer">
						Finally, we felt that we had enough contextual knowledge of our user populations
						businesses to answer our final research question.
					</div>
				</div>
				<h3>📋 Survey <span class="n">N=3</span></h3>
				<p>
					With our user journey mapped out, we wanted to present the painpoints back to a wider
					population so they could rank which problems they felt were most salient. Thus, we created
					a ranking survey in Qualtrics and sent it out to our users and a few WhatsApp small
					business groups.
				</p>
				<p>
					As you can tell by the meagre population size, the survey did not circulate very
					effectively, so we ultimately had to rely on our assessment of the problems to proceed.
				</p>
				<h3>🎯 Problem Selection</h3>
				<img src={personalEval} alt="" class="zoomable" style="max-width: 460px" />
			</div>
		</section>
		<section id="design">
			<div class="container">
				<h2><a href="#design">Design</a></h2>
			</div>
		</section>
		<section id="evaluation">
			<div class="container">
				<h2><a href="#evaluation">Evaluation</a></h2>
			</div>
		</section>
	</article>
</main>

<style lang="scss">
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
			min-width: 300px;
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
