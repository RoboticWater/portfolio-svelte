<script>
	import MdHelpOutline from 'svelte-icons/md/MdHelpOutline.svelte';
	import MdLightbulbOutline from 'svelte-icons/md/MdLightbulbOutline.svelte';
</script>

<div class="problem-solution">
	<div class="problem">
		<h3>
			<!-- <span>🤔</span> -->
			<div class="icon">
				<MdHelpOutline />
			</div>
			<slot name="problem-title" />
			<div class="arrow">
				<svg
					width="501"
					height="12"
					viewBox="0 0 501 12"
					fill="none"
					xmlns="http://www.w3.org/2000/svg"
				>
					<path d="M0 6L500 6.00004" stroke="black" />
					<path d="M495 1L500 6L495 11" stroke="black" />
				</svg>
			</div>
		</h3>
		<p>
			<slot name="problem" />
		</p>
	</div>
	<div class="vertical-arrow">
		<svg width="12" height="51" viewBox="0 0 12 51" fill="none" xmlns="http://www.w3.org/2000/svg">
			<path d="M6 51L6 1" stroke="black" />
			<path d="M1 6L6 1L11 6" stroke="black" />
		</svg>
	</div>
	<div class="solution">
		<h3>
			<!-- <span>💡</span> -->
			<div class="icon">
				<MdLightbulbOutline />
			</div>
			<slot name="solution-title" />
		</h3>
		<p>
			<slot name="solution" />
		</p>
	</div>
</div>

<style lang="scss">
	.problem-solution {
		display: flex;
		gap: 20px;
		flex-wrap: wrap;
		// border: 1px solid rgba(51, 51, 51, 0.15);
		border-radius: 0.25rem;
		padding: 1.5rem;

		background-size: 30px 30px;
		background-image: linear-gradient(to right, rgba(0, 0, 0, 0.05) 1px, rgba(0, 0, 0, 0.01) 1px),
			linear-gradient(to bottom, rgba(0, 0, 0, 0.05) 1px, rgba(0, 0, 0, 0.01) 1px);
		background-position: 20px;
		background-attachment: fixed;
		// background: rgba(0, 0, 0, 0.02);

		& > div {
			flex: 1;
			min-width: 260px;
		}
	}
	.icon {
		width: 24px;
		height: 24px;
		color: var(--grey);
		& :global(svg) {
			display: block;
		}
	}
	.vertical-arrow {
		height: 30px;
		overflow: hidden;
		display: flex;
		justify-content: center;
		display: none;
	}
	svg {
		transform: rotate(180deg);
		position: relative;
		top: -21px;
		animation: vertical-arrow-intro 0.4s cubic-bezier(0.58, 0.15, 0.15, 0.97) forwards;
	}
	h3 {
		display: flex;
		align-items: center;
		margin: 0;
		font-size: 1.75rem;
		gap: 0.25rem;
	}
	p {
		margin: 0.5rem 0;
	}
	.problem h3 {
		display: grid;
		grid-template-columns: auto auto 1fr;
	}
	.arrow {
		overflow: hidden;
		flex-shrink: 1;
		min-width: 0;
		position: relative;
		width: 100%;
		height: 100%;

		svg {
			position: absolute;
			top: 50%;
			transform: translateY(-50%);
			right: 100%;
			animation: arrow-intro 0.4s cubic-bezier(0.58, 0.15, 0.15, 0.97) forwards;
		}
	}

	@keyframes arrow-intro {
		from {
			right: 100%;
		}
		to {
			right: 0;
		}
	}

	@keyframes vertical-arrow-intro {
		from {
			top: -61px;
		}
		to {
			top: -21px;
		}
	}
	@media screen and (max-width: 800px) {
		.problem-solution {
			flex-direction: column;
		}
		.arrow {
			display: none;
		}
		.problem-solution {
			gap: 0.75rem;
		}
		.vertical-arrow {
			display: flex;
		}
	}
</style>
