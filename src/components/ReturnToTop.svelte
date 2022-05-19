<script>
	function scrollTop() {
		document.body.scrollTop = 0;
		document.documentElement.scrollTop = 0;
	}
</script>

<div class="return-to-top" on:click={scrollTop}>
	<svg width="12" height="51" viewBox="0 0 12 51" fill="none" xmlns="http://www.w3.org/2000/svg">
		<path d="M6 51L6 1" stroke="black" />
		<path d="M1 6L6 1L11 6" stroke="black" />
	</svg>
</div>

<style lang="scss">
	.return-to-top {
		// position: fixed;
		// bottom: 20px;
		// right: 20px;
		padding: 20px 15px;
		border-radius: 2px;
		cursor: pointer;
		transition: background 0.3s ease;
		&:hover {
			background: rgba(0, 0, 0, 0.05);
		}
	}
</style>
