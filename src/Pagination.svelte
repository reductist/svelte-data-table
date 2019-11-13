<script>
	export let currentPage
	export let totalPages
	export let hideSinglePage = true
	export let maxPageLinks = 5
	export let boundaryLinks = false
	export let firstText = 'First'
	//Fexport let lastText = 'Last'
	export let directionLinks = true
	
	$: displayPages = (() => {
		function displayAllPages () {
			const displayPages = []
			for (let i = 1; i <= totalPages; i++) {
				displayPages.push({
					title: i.toString(),
					value: i
				})
			}
			return displayPages
		}

		function limitVisiblePages () {
			const displayPages = []
			const totalTiers = Math.ceil(totalPages / maxPageLinks)
			const activeTier = Math.ceil(currentPage / maxPageLinks)
			const start = ((activeTier - 1) * maxPageLinks) + 1
			const end = start + maxPageLinks
			
			if (activeTier > 1) {
				displayPages.push({
					title: '...',
					value: start - 1
				})
			}

			for (let i = start; i < end; i++) {
				if (i > totalPages) {
					break
				}
				displayPages.push({
					title: i.toString(),
					value: i
				})
			}

			if (activeTier < totalTiers) {
				displayPages.push({
					title: '...',
					value: end
				})
			}
			return displayPages
		}

		if (isNaN(maxPageLinks) || maxPageLinks <= 0) {
			return displayAllPages()
		} else {
			return limitVisiblePages()
		}
	})()

	function selectPage (page) {
			if (page < 1 || page > totalPages || page === currentPage) {
					return
			}
			currentPage = page
	}

	function nextPage () {
			if (currentPage < totalPages) {
				currentPage++
			}
	}

	function previousPage () {
			if (currentPage > 1) {
				currentPage--
			}
	}

	function firstPage () {
			currentPage = 1
	}

	function lastPage () {
			currentPage = totalPages
	}
	
	// preventDefault() listener to block the .page-link 'a' elements from opening new browser windows.
	// Calling as a typical listener executes before the DOM renders the element target, so instead we'll
	// call back from inside a DOMContentLoaded listener.
	document.addEventListener('DOMContentLoaded', (event) => {
		const pl = document.getElementsByClassName("page-link");
		pl.addEventListener('click', (event) 	=> {
			event.preventDefault();
		})
	})
												
</script>

<style>
	.smart-pagination {
		width: fit-content;
	}
	.pagination {
		background: #2d2f3a;
		padding-left: 0.25em;
		padding-right: 0.25em;
		padding-top: 0.7em;
		padding-bottom: 0.5em;
		border-left: 1px solid #00ffc4;
		border-right: 1px solid #00ffc4;
		border-bottom: 1px solid #00ffc4;
		border-bottom-left-radius: 10px;
	}
	svg {
		padding: 0;
	}
	.disabled svg {
		color: grey;
	}
	.disabled a {
		cursor: not-allowed;
		color: #444;
	}
	span.page-item {
		padding: 0;
	}
	a.page-link:link {
		font-size: 20px;
		font-weight: 700;
		padding: 6px;
		color: #00ffc4;
		text-decoration-line: none;
	}
	a.page-link:link:hover:not(.active) {
		background: #12141a;
	}
</style>

{#if !(hideSinglePage && totalPages === 1)}
<nav class="smart-pagination">
	<div class="pagination">
		{#if boundaryLinks}
		<span class:disabled="{currentPage === 1}" class="page-item">
			<a href="#javascript:void(0)" aria-label="Previous" on:click="{firstPage}" class="page-link">
				<span aria-hidden="true">{@html firstText}</span>
			</a>
		</span>
		{/if}
		
		{#if directionLinks }
		<span class:disabled="{currentPage === 1}" class="page-item">
			<a href="javascript:void(0)" aria-label="Previous" on:click="{previousPage}" class="page-link">
				<slot name="previousIcon" disabled="{currentPage === 1}">
					<svg width="16" height="16" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512">
						<path fill="currentColor"
									d="M34.52 239.03L228.87 44.69c9.37-9.37 24.57-9.37 33.94 0l22.67 22.67c9.36 9.36 9.37 24.52.04 33.9L131.49 256l154.02 154.75c9.34 9.38 9.32 24.54-.04 33.9l-22.67 22.67c-9.37 9.37-24.57 9.37-33.94 0L34.52 272.97c-9.37-9.37-9.37-24.57 0-33.94z"></path>
					</svg>
				</slot>
			</a>
		</span>
		{/if}
		{#each displayPages as page (page.value)}
		<span class="page-item" class:active="{currentPage === page.value}">
			<a href="#javascript:void(0)" on:click="{() => selectPage(page.value)}" class="page-link">{page.title}</a>
		</span>
		{/each}
		{#if directionLinks}
		<span class:disabled="{currentPage === totalPages}" class="page-item">
			<a href="#javascript:void(0)" aria-label="Next" on:click="{nextPage}" class="page-link">
				<slot name="nextIcon" disabled="{currentPage === totalPages}">
					<svg width="16" height="16" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512">
						<path fill="currentColor"
									d="M285.476 272.971L91.132 467.314c-9.373 9.373-24.569 9.373-33.941 0l-22.667-22.667c-9.357-9.357-9.375-24.522-.04-33.901L188.505 256 34.484 101.255c-9.335-9.379-9.317-24.544.04-33.901l22.667-22.667c9.373-9.373 24.569-9.373 33.941 0L285.475 239.03c9.373 9.372 9.373 24.568.001 33.941z"></path>
					</svg>
				</slot>
			</a>
		</span>
		{/if}
		{#if boundaryLinks}
		<span class:disabled="{currentPage === totalPages}" class="page-item">
			<a href="#javascript:void(0)" aria-label="Previous" on:click="{lastPage}" class="page-link">
				<span aria-hidden="true" v-html="lastText"></span>
			</a>
		</span>
		{/if}
	</div>
</nav>
{/if}