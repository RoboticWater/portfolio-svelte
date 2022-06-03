<script>
	/**
	 * @type {{anchor: string, text: string}[]}
	 */
	export let sections;
	/**
	 * @type {string}
	 */
	export let currentSection;
</script>

<nav>
	<h4>Sections</h4>
	<ul>
		{#each sections as section}
			<li class:active={section.anchor === currentSection}>
				<a href={section.anchor}>
					<div class="arrow-container">
						<svg
							width="15"
							height="9"
							viewBox="0 0 15 9"
							xmlns="http://www.w3.org/2000/svg"
							class="arrow"
						>
							<path
								d="M10.06 8.28L9.08 7.32L10.82 5.6L11.82 4.76L11.8 4.7L9.86 4.82H0V3.46H9.86L11.8 3.58L11.82 3.52L10.82 2.68L9.08 0.960001L10.06 0L14.2 4.14L10.06 8.28Z"
							/>
						</svg>
					</div>
					{section.text}
				</a>
			</li>
		{/each}
	</ul>
</nav>

<style lang="scss">
	ul {
		list-style: none;
		padding: 0;
		margin: 0;
		margin-top: 4px;
		color: #555;
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
	h4 {
		font-size: 1.25rem;
		margin-bottom: 6px;
	}
	li {
		margin-bottom: 4px;
		font-size: 14px;
		&.active a {
			color: var(--article-active-color);
			&:before {
				opacity: 0.15;
				right: -6px;
			}
			.arrow-container {
				width: 14px;
			}
			.arrow {
				fill: var(--article-active-color);
			}
		}
		a {
			position: relative;
			display: inline-flex;
			color: #555;
			.arrow-container {
				width: 0;
				overflow: hidden;
				transition: width 0.2s ease;
			}
			&:hover .arrow-container {
				width: 14px;
			}
			.arrow {
				width: 12px;
				fill: #555;
			}
			&:before {
				position: absolute;
				z-index: -1;
				content: '';
				top: -2px;
				left: -6px;
				right: 100%;
				bottom: -2px;
				border-radius: 4px;
				background: var(--article-color);
				opacity: 0;
				transition: opacity 0.1s ease, right 0.25s ease;
			}
		}
	}
</style>
