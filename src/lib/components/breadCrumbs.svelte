<script>
	export let params;
	export let partners;
	export let websites;

	const faviconAPI =
		'https://t1.gstatic.com/faviconV2?client=SOCIAL&type=FAVICON&fallback_opts=TYPE,SIZE,URL&url=';

	$: selectedPartner = params.websiteUID
		? partners.websites.find(({ slug }) => slug === params.websiteUID)
		: '';
	$: selectedUrl = params.urlUID ? params.urlUID : '';
</script>

<div class="bread-crumbs">
	<div class="dropdown">
		<button>
			{#if selectedPartner}
				<img
					width="24"
					src="{faviconAPI}{selectedPartner.homepage}/&size=128"
					alt="logo partner"
				/>{selectedPartner.titel}
			{:else}
				Partners overzicht
			{/if}
		</button>
		<ul>
			<li>
				<a href="/">Partners overzicht</a>
			</li>
			{#each partners.websites as partner}
				<li>
					<a href="/{partner.slug}">
						<img width="24" src="{faviconAPI}{partner.homepage}/&size=256" alt="logo partner" />
						{partner.titel}
					</a>
				</li>
			{/each}
		</ul>
	</div>

	{#if websites}
		<span class="seperator">/</span>
		<div class="dropdown">
			<button>
				{#if selectedUrl}
					{selectedUrl}
				{:else}
					<span>Websites overzicht</span>
				{/if}
			</button>
			<ul>
				<li>
					<a href="/{selectedPartner.slug}">Websites overzicht</a>
				</li>
				{#each websites.urls as website}
					<li>
						<a href="/{selectedPartner.slug}/{website.slug}">{website.slug}</a>
					</li>
				{/each}
			</ul>
		</div>
	{/if}
</div>

<style>
	.bread-crumbs {
		display: flex;
		align-items: center;
		gap: 0.5rem;
	}

	button {
		position: relative;
		display: flex;
		align-items: center;
		gap: 0.5rem;
		appearance: none;
		padding: 1em 0.6em;
		padding-right: 4em;
		border-radius: 0.5em;
		font-size: 1em;
		background-color: var(--c-container);
		color: var(--c-grey);
		border: none;
		width: 100%;
		height: 3.5rem;
		text-align: left;
		box-shadow: 0px -55px 0px 10px var(--c-background);
	}

	button::after {
		content: url('data:image/svg+xml,<svg width="12" height="7" viewBox="0 0 12 7" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M1 1L6 6L11 1" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>');
		position: absolute;
		right: 5%;
		scale: 1.3;
		transition: 0.2s;
	}

	.dropdown {
		position: relative;
		display: inline-block;
		min-width: 19rem;
		height: max-content;
		z-index: 1;
	}

	.dropdown img {
		border-radius: 4px;
		height: 24px;
		width: 24px;
	}

	.seperator {
		font-size: 1.5rem;
		color: var(--c-white2);
	}

	ul {
		position: absolute;
		background-color: #2c2c2c;
		max-height: 0;
		width: 100%;
		border-radius: 0 0 0.5em 0.5em;
		overflow: hidden;
		transform: translateY(-100%);
		transition: 0.2s;
		z-index: -1;
	}

	ul li:first-child {
		border-bottom: 1px solid;
	}

	ul a {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		color: var(--c-white);
		background-color: var(--c-container);
		padding: 12px 16px;
		text-decoration: none;
	}

	ul:has(a:focus) {
		max-height: min-content;
		min-width: max-content;
		transform: translateY(0);
	}

	ul a:hover {
		background-color: var(--c-white);
		color: var(--c-text-header);
	}

	ul a:hover img {
		transform: translateY(-3px);
	}

	.dropdown:hover ul {
		max-height: min-content;
		min-width: max-content;
		transform: translateY(0);
	}

	.dropdown:hover button {
		background-color: var(--c-container);
		border-radius: 0.5em 0.5em 0 0;
	}

	button:has(a:focus) {
		background-color: var(--c-container);
		border-radius: 0.5em 0.5em 0 0;
	}

	.dropdown:hover button::after {
		transform: scale(-1, -1);
	}

	@media print {
		.bread-crumbs {
			display: none;
		}
	}

	@media only screen and (max-width: 990px) {
		.bread-crumbs {
			grid-row: 2;
			grid-column: span 2;
		}

		.bread-crumbs .dropdown {
			width: 100%;
			min-width: min-content;
		}
		.bread-crumbs .dropdown ul {
			width: 100%;
		}
	}

	@media only screen and (max-width: 560px) {
		.bread-crumbs {
			display: flex;
			flex-direction: column;
		}
		.seperator {
			display: none;
		}
		button {
			box-shadow: none;
		}
		ul {
			transition: 0s;
			position: relative;
		}
	}
</style>
