<script>
	// import 'intersection-observer-debugger';
	import { onMount } from 'svelte';
	import { fade } from 'svelte/transition';
	import { quadOut } from 'svelte/easing';

	import Card from '../components/Card.svelte';
	import ProblemSolution from '../components/ProblemSolution.svelte';
	import Observer from '../components/Observer.svelte';
	import ToolsetEntry from '../components/ToolsetEntry.svelte';
	import Sidebar from '../components/Sidebar.svelte';
	import ReturnToTop from '../components/ReturnToTop.svelte';

	import titleImg from '../assets/whiteboarding/title-image.png';
	import evidenceBG from '../assets/whiteboarding/preview-evidence-bg.png';
	import evidenceFG from '../assets/whiteboarding/preview-evidence-content.png';
	import layerBG from '../assets/whiteboarding/preview-layer-bg.png';
	import layerFG from '../assets/whiteboarding/preview-layer-content.png';
	import questionBG from '../assets/whiteboarding/preview-question-bg.png';
	import questionFG from '../assets/whiteboarding/preview-question-content.png';
	import timeline from '../assets/whiteboarding/timeline.png';
	import journeyMap from '../assets/whiteboarding/journey-map.png';
	import journeyMapHighlight from '../assets/whiteboarding/journey-map-highlighted.png';
	import header from '../assets/whiteboarding/whiteboarding-bg.png';
	import ideation from '../assets/whiteboarding/ideation.png';
	import ideas from '../assets/whiteboarding/ideas.png';
	import ideaFeedback from '../assets/whiteboarding/idea-feedback.png';
	import ideaDigitalPD from '../assets/whiteboarding/idea-digitalPD-desc.png';
	import ideaContextual from '../assets/whiteboarding/idea-contextual-desc.png';
	import wireQuestion0 from '../assets/whiteboarding/wireframe-question-00.png';
	import wireQuestion1 from '../assets/whiteboarding/wireframe-question-01.png';
	import wireEvidence0 from '../assets/whiteboarding/wireframe-evidence-00.png';
	import protoEvidence0 from '../assets/whiteboarding/proto-evidence-00.png';
	import protoLayer0 from '../assets/whiteboarding/proto-layer-00.png';
	import protoQuestion0 from '../assets/whiteboarding/proto-question-00.png';
	import evalResults from '../assets/whiteboarding/eval-results.png';
	import issuesEvidence from '../assets/whiteboarding/issues-evidence.png';

	let color = '#4567F6';
	let activeColor = '#0D3AF4';

	let y = 0;

	/**
	 * @type {{text: string, anchor: string}[]}
	 */
	let sections = [];
	let currentSection = '#intro';

	onMount(() => {
		console.log(1);
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

		if (location.hash.includes('-demo')) {
			demo = demoLinks[location.hash];
		}
	});

	/**
	 * @type {{[key: string]: string}}
	 */
	let demoLinks = {
		'#evidence-demo': 'https://www.youtube.com/embed/0Iu7xx-rC-w',
		'#layers-demo': 'https://www.youtube.com/embed/epgFZ0kVRWU',
		'#questions-demo': 'https://www.youtube.com/embed/mUwROJjAu68'
	};

	function closeDemo() {
		demo = '';
		history.pushState('', document.title, window.location.pathname + window.location.search);
	}

	function handleHashChange() {
		if (location.hash.includes('-demo')) {
			demo = demoLinks[location.hash];
		} else {
			demo = '';
			currentSection = location.hash;
		}
	}
	function handleKeyDown(event) {
		if (event.code === 'Escape' && location.hash.includes('-demo')) {
			demo = '';
			history.pushState('', document.title, window.location.pathname + window.location.search);
		}
	}

	let imgPreview = '';
	let imgPreviewBG = '';
	let demo = '';
</script>

<svelte:window bind:scrollY={y} on:keydown={handleKeyDown} on:hashchange={handleHashChange} />
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
	{#if demo}
		<div class="demo" on:click={closeDemo}>
			<iframe
				src={demo}
				title="YouTube video player"
				frameborder="0"
				allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
				allowfullscreen
			/>
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
			<div class="title-bg" style={`background-image: url(${header}); `}>
				<!-- <div class="main-img" style={`background-image: url(${titleImg});`} /> -->
				<h1><span> Participatory Whiteboarding </span></h1>
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
				<div class="info">
					<div class="info__entry">
						<h4>Timeframe</h4>
						<ul>
							<li>August 2021 - May 2022</li>
						</ul>
					</div>
					<div class="info__entry">
						<h4>Team</h4>
						<ul>
							<li>John Britti</li>
							<li>Sean Perryman</li>
							<li>Wenrui Zhang</li>
						</ul>
					</div>
					<div class="info__entry">
						<h4>Tools</h4>
						<ul>
							<li>User Interviews</li>
							<li>Observation</li>
							<li>Journey Mapping</li>
							<li>Figma</li>
						</ul>
					</div>
					<div class="info__entry">
						<h4>Tags</h4>
						<ul>
							<li>Participatory Design</li>
							<li>Whiteboarding</li>
							<li>Professional Productivity</li>
							<li>Figma</li>
						</ul>
					</div>
				</div>
			</div>
			<div class="container bg">
				<ProblemSolution>
					<span slot="problem-title">Problem Space</span>
					<span slot="problem">
						How can we leverage digital tools to make participatory design easier and more
						effective?
					</span>
					<span slot="solution-title">Solution</span>
					<span slot="solution">
						A digital whiteboarding toolset that helps users more easily engage with complicated
						whiteboard interfaces and helps researchers quickly collect data, organize it, and
						return to it later without losing context.
					</span>
				</ProblemSolution>
			</div>
			<p>
				With the wider adoption of digital whiteboarding tools like Miro and FigJam, design
				researchers have the opportunity to reimagine the spaces where design takes place. No longer
				must designers and researchers keep their design work cloistered away from their users; the
				digital whiteboard can allow everyone to engage together and immerse themselves in data.
			</p>
			<p>
				Our team worked with professional design practitioners both in industry and academia to
				imagine a whiteboarding toolset that could help bring about this new paradigm of digital
				participatory design.
			</p>
			<p>
				These tools help design researchers <b>better engage with participants</b> during sessions
				and <b>immerse themselves in the resulting data</b>.
			</p>
		</section>
		<section id="the-toolset">
			<div class="container">
				<h2><a href="#the-toolset">The Toolset</a></h2>
				<p>
					Here are the final prototypes for our whiteboarding toolset imagined in the context of
					Miro. You can view an interactive version <a
						href="https://www.figma.com/proto/gltjdJQ3iHeT5PeDr01hGI/Prototypes?page-id=1202%3A71760&node-id=1207%3A72177&viewport=241%2C48%2C0.06&scaling=min-zoom&starting-point-node-id=1207%3A72177&show-proto-sidebar=1"
						target="__blank">here</a
					>.
				</p>
				<div class="toolset">
					<ToolsetEntry
						openDemo={() => (demo = demoLinks['#evidence-demo'])}
						demoHash="#evidence-demo"
						imgBackground={evidenceBG}
						imgForeground={evidenceFG}
					>
						<span slot="number">1</span>
						<span slot="title">Evidence Collection Pipeline</span>
						<span slot="description">
							Integrating auto-transcripting with quick and intuitive interactions this tool helps
							researchers capture user data during interviews and keeps it organized so they never
							lose sight of the original context.
						</span>
					</ToolsetEntry>
					<ToolsetEntry
						openDemo={() => (demo = demoLinks['#layers-demo'])}
						demoHash="#layers-demo"
						imgBackground={layerBG}
						imgForeground={layerFG}
					>
						<span slot="number">2</span>
						<span slot="title">Layers</span>
						<span slot="description">
							Layers are an essential productivity and organization feature in most design apps. Our
							prototype imagines how they could be integrated in to whiteboards and enhanced to
							facilitate design research.
						</span>
					</ToolsetEntry>
					<ToolsetEntry
						openDemo={() => (demo = demoLinks['#questions-demo'])}
						demoHash="#questions-demo"
						imgBackground={questionBG}
						imgForeground={questionFG}
					>
						<span slot="number">3</span>
						<span slot="title">Question Prompts</span>
						<span slot="description">
							This tool empowers researchers with the ability to better structure their user
							sessions with questionaire functionality and helps participants navigate complex
							whiteboard interfaces.
						</span>
					</ToolsetEntry>
				</div>
			</div>
		</section>
		<section id="timeline">
			<div class="container">
				<h2><a href="#timeline">Timeline</a></h2>
				<p>
					This project largely followed a standard
					<a
						href="https://en.wikipedia.org/wiki/Double_Diamond_(design_process_model)"
						target="__blank"
					>
						double diamond design
					</a>
					process with a more lean UX cadence during the ideation phase.
				</p>
				<img src={timeline} alt="" class="zoomable white" />
			</div>
		</section>
		<section id="research">
			<div class="container">
				<h2><a href="#research">Research</a></h2>
				<p>
					Because our problem space was more speculative than reactive—that is, our goal was to
					imagine new approaches to participatory desisgn, not directly address some specific point
					of failure—we conducted a mixed-method research approach designed to both identify the
					challenges of participatory design and collect novel approaches to participatory design
					that we could bring into our ideation.
				</p>
				<p>
					We gathered a whole heap of insights, so I'll only share a few from each method to show
					what kind of results we were getting.
				</p>
				<div class="research-results">
					<div class="method">
						<h3>📚 Literature Review</h3>
						<p>
							To get a grasp on participatory design, we consulted academic papers for both
							information on its general nature and novel methods that academic practitioners
							deployed in the field.
						</p>
					</div>
					<div class="results">
						<Observer let:intersecting rootMargin="0% 0% -25%" once>
							<div class="fade-container" class:intersecting style="transition-delay: 0ms">
								<Card>
									<span slot="label">Literature Review Insight #1</span>
									<span slot="title">Designing With</span>
									<span slot="content">
										Participatory design is about designing <i>with</i>, so participatory methods
										need to emphasize the autonomy of the participant in research.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 100ms">
								<Card>
									<span slot="label">Literature Review Insight #2</span>
									<span slot="title">Transcription</span>
									<span slot="content">
										Transcription is a powerful tool for capturing qualitative data, and letting
										participants explore it may reveal insights the researchers may have missed.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 200ms">
								<Card>
									<span slot="label">Literature Review Insight #3</span>
									<span slot="title">Layered Elaboration</span>
									<span slot="content">
										An effective tactic to ease participants into the design process and get the
										best results is "layered elaboration": taking the participant through the same
										activity multiple times and have them iteratively expand on their answers.
									</span>
								</Card>
							</div>
						</Observer>
					</div>
				</div>
				<div class="research-results">
					<div class="method">
						<h3>⚖️ Comparative Analysis</h3>
						<p>
							In addition to reviewing academic sources, we also aggregated and analyzed several
							professional research and design tools to discover what features are currently being
							offered and what gaps still remain.
						</p>
					</div>
					<div class="results">
						<Observer let:intersecting rootMargin="0% 0% -25%" once>
							<div class="fade-container" class:intersecting style="transition-delay: 0ms">
								<Card>
									<span slot="label">Comparative Analysis Insight #1</span>
									<span slot="title">Expectations</span>
									<span slot="content">
										Our comparison matrix served as an effective bar for user expectations; later
										when we started designing, we would rely on these findings as a benchmark.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 100ms">
								<Card>
									<span slot="label">Comparative Analysis Insight #2</span>
									<span slot="title">Integration</span>
									<span slot="content">
										Services overall covered most of a UX practitioner’s needs, but individually,
										they often lacked features for a full UXR process, so researchers would likely
										need to jump across platforms to perform their work.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 200ms">
								<Card>
									<span slot="label">Comparative Analysis Insight #3</span>
									<span slot="title">Opportunity for Journey Management</span>
									<span slot="content">
										While there were several journey mapping services available, none seemed focused
										on <a
											href="https://www.nngroup.com/articles/customer-journey-management/#:~:text=Journey%20management%20is%20the%20ongoing,users%20and%20achieve%20business%20goals."
											target="__blank"
										>
											journey management</a
										>. We thought the concept of a continually updating user journey might offer
										more opportunities for user participation.
									</span>
								</Card>
							</div>
						</Observer>
					</div>
				</div>
				<!-- <h3>Journey Map Analysis</h3> -->
				<div class="research-results">
					<div class="method">
						<h3>👁️ Observation <span class="n">N=2</span></h3>
						<p>
							We had the opportunity to shadow professional design researchers at NCR while they
							performed participatory journey mapping with customers.
						</p>
						<p>
							While we all had some experience running participatory methods from our classes, this
							gave us a look at how these methods work in the real world.
						</p>
					</div>
					<div class="results">
						<Observer let:intersecting rootMargin="0% 0% -25%" once
							><div class="fade-container" class:intersecting style="transition-delay: 0ms">
								<Card>
									<span slot="label">Observation Insight #1</span>
									<span slot="title">Trouble Keeping Up</span>
									<span slot="content"
										>Even note-takers would occasionally fall behind when creating notes and miss
										some detail of the user's journey.</span
									>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 100ms">
								<Card>
									<span slot="label">Observation Insight #2</span>
									<span slot="title"
										>Structures in Digital Whiteboards Can Be Difficult to Edit</span
									>
									<span slot="content">
										When making adjustments the journey map in Mural, researchers encountered
										friction with the interface and the structures they had created. Extending a
										phase meant selecting every subsequent phase and moving them.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 200ms">
								<Card>
									<span slot="label">Observation Insight #3</span>
									<span slot="title">Aggregation is Onerous</span>
									<span slot="content">
										Once the interviews were over, it was another several-hour process to clean and
										aggregate the data.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 300ms">
								<Card>
									<span slot="label">Observation Insight #4</span>
									<span slot="title">Participants Wouldn't Engage with the Board</span>
									<span slot="content">
										Participants largely didn’t interact with the journey map even with prompting
										from the facilitators. They would respond to questions relating to their
										journey, but rarely would they reference or critique elements on the whiteboard.
									</span>
								</Card>
							</div>
						</Observer>
					</div>
				</div>
				<div class="research-results">
					<div class="method">
						<h3>💬 Semi-Structured Interviews <span class="n">N=7</span></h3>
						<p>
							The most fruitful research endeavor we pursued, however, were 7 semi-structured
							interviews with UX practitioners. 5 of which were peers at Georgia Tech and 2 of which
							were colleages at NCR.
						</p>
						<p>
							Each interview lasted roughly an hour, and we asked each participant to bring (or
							reference, if they no longer had access) a design artifact they co-developed with
							participants.
						</p>
					</div>
					<div class="results">
						<Observer let:intersecting rootMargin="0% 0% -25%" once>
							<div class="fade-container" class:intersecting style="transition-delay: 0ms">
								<Card>
									<span slot="label">Semi-Structured Interviews Insight #1</span>
									<span slot="title">Confirming Other Research</span>
									<span slot="content">
										Though redundant, it's important that the personal experiences of our
										interviewees confirmed our findings from other methods, like how participants
										have a hard time engaging and how numerous the touchpoints are.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 100ms">
								<Card>
									<span slot="label">Semi-Structured Interviews Insight #2</span>
									<span slot="title">Taking All the Notes</span>
									<span slot="content">
										Getting participants to engage with the design process is often enough of a
										challenge, so facilitators ae often the only people interacting with the
										whiteboard, preferring to lessen the cognitive load of the participant.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 200ms">
								<Card>
									<span slot="label">Semi-Structured Interviews Insight #3</span>
									<span slot="title">Writing in the Margins</span>
									<span slot="content">
										When conducting PD sessions with users within whiteboarding spaces, researchers
										would often want to take notes about their users on the board itself without
										users seeing the notes, so they resort to taking notes in the far margins,
										potentially losing sight of the context of the note.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 300ms">
								<Card>
									<span slot="label">Semi-Structured Interviews Insight #4</span>
									<span slot="title">Results are Very Messy</span>
									<span slot="content">
										One of our interviewees described participatory design results as looking a
										little like <a
											href="https://medium.com/cma-thinker/to-infinity-and-beyond-inside-the-obliteration-room-at-the-cleveland-museum-of-art-fc03e10b5d8c"
											target="__blank">The Obliteration Room</a
										>. Essentially, results from these sessions often cluttered and need extensive
										cleaning to make usable and presentable.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 400ms">
								<Card>
									<span slot="label">Semi-Structured Interviews Insight #5</span>
									<span slot="title">Data Structures on the Fly</span>
									<span slot="content">
										Often times, researchers would come into a participatory design session with a
										particular structure in mind, but would find that this one structure wasn't
										adequate to describe what their participant wanted. For instance, the phases of
										a journey map may be too rigid, so maybe they should be a Venn Diagram instead.
									</span>
								</Card>
							</div>
							<div class="fade-container" class:intersecting style="transition-delay: 500ms">
								<Card>
									<span slot="label">Semi-Structured Interviews Insight #6</span>
									<span slot="title">Assigning Tasks</span>
									<span slot="content">
										To ensure that participants are engaged particularly in multi-user sessions,
										researchers would assign each person a unique task they could focus on. Clear
										activities help participants engage with the design process.
									</span>
								</Card>
							</div>
						</Observer>
					</div>
				</div>
				<h3>🗺️ Participatory Design Journey</h3>
				<p>
					While we had individually organized all of our data, we wanted to summarize together as
					much of it as possible so we could most effectively contrast individual insights and
					identify where we could best intervene. To do so we constructed a large journey map, which
					I’ve summarized here, to identify where the most fertile soil was for a design
					intervention
				</p>
				<p>
					Given the prevalence of problems throughout the design research journey we knew we would
					have to focus in on a smaller chunk, or else we'd spread ourselves too thin.
				</p>
				<Observer let:intersecting rootMargin="0% 0% -70%">
					<div class="map-container" class:intersecting>
						<img src={journeyMap} class="zoomable white" alt="" />
						<img src={journeyMapHighlight} class="zoomable white" alt="" />
					</div>
				</Observer>
				<p>
					All of these phases had good enough evidence to support design work, but we ultimately
					chose the <b>facilitation phase</b> because it seemed to have the most pressing pain points
					and most opportunities for intervention; however, we wouldn't be strict if our ideas spilled
					into preparation or synthesis stages.
				</p>
				<h3>🗂️ Design Requirements</h3>
				<p>With our focus chosen, we generated design requirements to guide our idea generation.</p>
				<div class="card-list">
					<Card>
						<span slot="label">Design Requirement #1</span>
						<span slot="title">The System Should...</span>
						<span slot="content">
							Support constructing data structures that are flexible and produce good insights.
						</span>
					</Card>
					<Card>
						<span slot="label">Design Requirement #2</span>
						<span slot="title">The System Should...</span>
						<span slot="content">
							Allow the UX practitioner to record the thoughts of participants mid-session.
						</span>
					</Card>
					<Card>
						<span slot="label">Design Requirement #3</span>
						<span slot="title">The System Should...</span>
						<span slot="content">
							Support efficiently translating those thoughts to coherent research artifacts.
						</span>
					</Card>
					<Card>
						<span slot="label">Design Requirement #4</span>
						<span slot="title">The System Should...</span>
						<span slot="content">
							Support organizing research artifacts to facilitate synthesis.
						</span>
					</Card>
				</div>
			</div>
		</section>
		<section id="ideation">
			<div class="container">
				<h2><a href="#ideation">Ideation</a></h2>
				<img src={ideation} class="zoomable height" alt="" style="max-width: 760px" />
				<div class="img-description">Ideating in Georgia Tech's Industrial Design lab</div>
				<p>
					At this stage, we focused on generating as many ideas as possible based on user needs and
					design requirements. First, we timed ourselves to generate ideas on our own. Then we
					swapped our “idea cards” and built on top of each other’s ideas.
				</p>
				<p>
					While going through all of the initial ideas, we found that there were some recurring
					themes, so we grouped the ideas that could fit well together into broader categories,
					identified key features that would best address user needs, and consolidated them into six
					design concepts.
				</p>
				<img src={ideas} class="zoomable height" alt="" style="max-width: 760px" />
				<h3>✏️ Sketching & Feedback <span class="n">N=3</span></h3>
				<div class="img-cols">
					<img src={ideaDigitalPD} class="zoomable" alt="" style="max-width: 760px" />
					<img src={ideaContextual} class="zoomable" alt="" style="max-width: 760px" />
				</div>
				<div class="img-description">Two of our sketched ideas</div>
				<p>
					With our 6 ideas, we sketched them out and gave them brief descriptions. We then recruited
					two peers from the HCI program and scheduled 45min feedback sessions where we asked them
					to identify positives, negatives, and opportunities with each idea.
				</p>
				<img src={ideaFeedback} class="zoomable" alt="" />
				<div class="img-description">Rose, bud, thorn activity for our 6 ideas</div>
				<h3>📦 Concept Reconfiguration</h3>
				<p>
					The results of these sessions showed that no singular idea was perfect, and we ended up
					using the feedback from this method to take apart each idea and repackage certain features
					as a more limited toolset.
				</p>
				<div class="card-list ideas">
					<Card border>
						<span slot="label">Idea #1</span>
						<span slot="title">Breakout Boards</span>
						<span slot="content">
							Breakout boards allow facilitators to group and “send” participants to their own work
							spaces, similar to breakout rooms in Zoom. Participants will then see only their
							specific space, with facilitators having a global view of all spaces.
						</span>
					</Card>
					<Card border>
						<span slot="label">Idea #2</span>
						<span slot="title">Evidence Collection</span>
						<span slot="content">
							Evidence collection allows for better integration of raw data into a whiteboard for
							the purposes of analysis by integrating transcription features and image uploads into
							the whiteboarding space.
						</span>
					</Card>
					<Card border>
						<span slot="label">Idea #3</span>
						<span slot="title">Layers</span>
						<span slot="content">
							Layers brings the familiar feature from other design apps to whiteboards with the
							addition of privacy. Owners of the whiteboard can now better organize content, and
							mark things up without disturbing them, while keeping sensitive information hidden.
						</span>
					</Card>
					<Card border>
						<span slot="label">Idea #4</span>
						<span slot="title">Question Prompts</span>
						<span slot="content">
							Question prompts enables researchers to create simple, structured activities within a
							whiteboard that are tied to elements on the board, giving participants an easier
							handle on a whiteboard's complex interface.
						</span>
					</Card>
				</div>
			</div>
		</section>
		<section class="section--design" id="design">
			<div class="container">
				<h2><a href="#design">Design</a></h2>
				<p>
					With our new set of ideas in hand, we began designing wireframes for testing. Rather than
					building out prototypes over a longer period of time, then having isolated testing
					periods, we adopted a more lean UX approach where we tested each protoype weekly,
					regardless of where it was in development. This process afforded us two distinct benefits:
				</p>
				<ol>
					<li>
						We were able to respond to participant feedback immediately and make larger conceptual
						pivots while the prototypes were still "hot."
					</li>
					<li>
						We we able to elicit a gradient of feedback for our prototypes from more conceptual when
						they were lower fidelity to usability when they became high fidelity.
					</li>
				</ol>
				<h3>🔄 Lean UX Process <span class="n">N=9</span></h3>
				<p>
					Every week for a month we scheduled 2-3 30 minute feedback sessions with UX practitioners
					(7 in industry and 2 MSHCI students). During these short sessions we had participants
					evaluate a subset of our tools by interacting with them on Figma and sharing their
					thoughts aloud as they did so.
				</p>
				<div class="lean-ux">
					<div class="details">
						<p>
							Early prototypes were little more than concept wireframes with accompanying
							descriptions and barely any interactions.
						</p>
						<p>
							At this stage we mainly gathered concept feedback, asking participants if they saw
							value in the idea, to recall (or imagine) situations where an idea would have been
							helpful, and to provide feedback on how the idea could be further realized.
						</p>
					</div>
					<div class="images">
						<img src={wireQuestion0} class="zoomable" alt="" />
						<img src={wireQuestion1} class="zoomable" alt="" />
						<img src={wireEvidence0} class="zoomable" alt="" />
					</div>
				</div>
				<p>
					Results from early on lead to larger swings in develoment. For instance, early feedback
					suggested that breakout boards while potentially helpful in certain circumstances, was too
					niche a problem and too slight a solution to warrant continued development.
				</p>
				<div class="lean-ux reversed">
					<div class="details">
						<p>
							As the ideas developed, the prototypes became more elaborate with multiple interaction
							flows and the feedback we sought became more focused on granular details and
							usability.
						</p>
						<p>
							You'll notice in the images that we're relying more on a fake parking ticket scenario
							to give our users some context as they interact with the prototype.
						</p>
					</div>
					<div class="images">
						<img src={protoEvidence0} class="zoomable" alt="" />
						<img src={protoQuestion0} class="zoomable" alt="" />
						<img src={protoLayer0} class="zoomable" alt="" />
					</div>
				</div>
			</div>
		</section>
		<section id="evaluation">
			<div class="container">
				<h2><a href="#evaluation">Evaluation</a></h2>
				<p>
					After a month of prototyping with users, we took 2 more weeks to re-review feedback and
					prepare "final" prototypes that could be evaluated by our peers.
				</p>
				<h3>📋 Task-based Usability Evaluation <span class="n">N=5</span></h3>
				<p>
					To evaluate overall usability we conducted task-based assessments for each of our
					features, using a think-aloud protocol, with five of our peers in the MS-HCI program. We
					also included in our evaluations an A/B test for our layers feature to assess visual
					design variations for communicating note privacy and layer position.
				</p>
				<img
					src={evalResults}
					class="zoomable"
					style="max-width: 760px;"
					alt="Evaluation results for Evidence Collection"
				/>
				<div class="img-description">Evaluation results for Evidence Collection</div>
				<p>
					We organized the results in Figjam according to feature and flow, and rated notes
					according to severity from 0-4. Severity ratings were assigned by team members and
					determined based on considerations such as the centrality of an identified problem to the
					overall functionality of the system, how many users identified a given problem, and the
					feasibility of solving the problem.
				</p>
				<img
					src={issuesEvidence}
					class="zoomable"
					style="max-width: 760px;"
					alt="Select issues identified with Evidence Collection"
				/>
				<div class="img-description">Select issues identified with Evidence Collection</div>
				<p>
					Overall impressions were positive, with participants expressing enthusiasm towards the
					ideas, but of course, there were still issues that needed addressing. In the interest of
					brevity, I'll just describe some of the more severe problems we identified in Evidence
					Collection.
				</p>
				<p style="background: red">FINISH</p>
				<p />
				<h3>💌 Responding to Feedback</h3>
				<p>
					We had some time before our final presentation, so we decided to respond to some of the
					feedback we recieved. We couldn't get to everything, but using our results, we triaged the
					more pernicious issues and a few lower priority "quick wins."
				</p>
				<p>
					These changes can be seen in the demo videos and interactive prototype <a
						href="https://www.figma.com/proto/gltjdJQ3iHeT5PeDr01hGI/Prototypes?page-id=1202%3A71760&node-id=1207%3A72177&viewport=241%2C48%2C0.06&scaling=min-zoom&starting-point-node-id=1207%3A72177&show-proto-sidebar=1"
						target="__blank">here</a
					>.
				</p>
			</div>
		</section>
		<section id="conclusion">
			<div class="container">
				<h2><a href="#conclusion">Conclusion</a></h2>
				<h3>📏 Limitations</h3>
				<p>
					My greatest regret and likely the biggest limitation in this project is that we never
					constructed a fully functional prototype that we could test in a more realistic setting.
					While our prototype robustly captures the UI flow of each concept and adequately
					demonstrates what our toolset does, it doesn’t let us evaluate how it actually changes the
					process of a participatory design session and indeed if it does improve anything.
				</p>
				<p>
					We may have received positive feedback about the concepts and even had participants
					imagining concrete situations where the features would be helpful, but these data aren’t
					full confirmation.
				</p>
				<h3>🚀 Future Directions</h3>
				<p>
					I believe that there's much more work to be done here. My dream for the future
					participatory design is one where the barriers between UX professionals and the users
					they're trying to help are as low as possible.
				</p>
				<p>
					Much like a physical room where design and design research occur, these digital tools are
					providing designers with a means of collaborating in a shared space, of bringing end-users
					into participatory design practices, of housing various design artifacts in a single,
					persisting place. As work increasingly becomes physically disbursed and remote, these
					tools allow design work to occur across greater distances.
				</p>
				<p>
					They also improve the access of end-users to design spaces by removing physical barriers
					and making participation more convenient. I view this “design room” metaphor for
					whiteboarding tools as being rich in design opportunities for expanding the functionality
					of these spaces.
				</p>
			</div>
		</section>
	</article>
</main>

<style lang="scss">
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
		z-index: 1000;
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
		height: 60vh;
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
			bottom: 33%;
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
		padding-bottom: 50vh;
		position: relative;
		z-index: 1;
		&:after {
			content: '';
			width: 0.5rem;
			height: 0.5rem;
			position: absolute;
			left: 0;
			top: -0.5rem;
			background: radial-gradient(circle at 100% 0, #fff0 0.5rem, #fff 0px);
			z-index: 10;
		}
	}
	.container > img {
		border-radius: 0.25rem;
	}
	section {
		position: relative;
	}
	.img-description {
		font-size: 0.8em;
		font-style: italic;
	}
	.sidebar {
		grid-area: sidebar;
		justify-self: flex-end;
		width: 100px;
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
		padding: 2rem 0;
		padding-right: 2rem;
		max-width: 64rem;
		position: relative;
		&.top {
			border-top-right-radius: 0.5rem;
			&:after {
				position: absolute;
				top: -0.5rem;
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
	#design:before {
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
	.info {
		columns: 3;
		font-size: 0.8em;
		.info__entry {
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
		&.reversed {
			// flex-direction: row-reverse;
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
		.method {
			// position: sticky;
			// top: 0.5rem;
			// align-self: flex-start;
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

	@media screen and (max-width: 760px) {
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
