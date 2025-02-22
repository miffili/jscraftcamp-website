<script lang="ts">
	import Box from '$lib/layout/Box.svelte';
	import twitterLogo from './twitter.svg';
	import mastodonLogo from './mastodon.svg';
	import websiteLogo from './website.svg';
	import type { Participant } from '$lib/participants/participant-schema';
	import { createEventDispatcher } from 'svelte';

	export let participant: Participant;
	export let isActive: boolean = false;

	let isShowingDetails = false;
	const hasSocialLink = participant.twitter || participant.mastodon || participant.website;

	const dispatch = createEventDispatcher<{ selectedTag: string }>();
</script>

<Box style={isActive ? '--box-shadow: 0 0 25px -10px #080f, 0 0 200px rgba(0, 0, 0, 0.1);' : ''}>
	<div class="participant">
		<div class="attendance">
			{#if participant.iCanTakeNotesDuringSessions}<span class="nt" title="offers to take notes"
					>📝</span
				>{/if}
			{#if participant.when.friday}<span class="fr" title="will attend Friday, 30th June">Fr</span
				>{/if}
			{#if participant.when.saturday}<span class="sa" title="will attend Saturday, 1st July"
					>Sa</span
				>{/if}
		</div>
		<h3>
			<button type="button" on:click={() => (isShowingDetails = !isShowingDetails)}
				>{participant.name}</button
			>
		</h3>
		{#if participant.company}<h4>{participant.company}</h4>{/if}
		{#if hasSocialLink}
			<div class="socials">
				{#if participant.twitter}
					<a href="https://twitter.com/{participant.twitter}" rel="external"
						><img
							src={twitterLogo}
							height="32"
							width="32"
							alt="Link to @{participant.twitter} on Twitter"
						/></a
					>
				{/if}
				{#if participant.mastodon}
					<a href={participant.mastodon} rel="external"
						><img
							src={mastodonLogo}
							height="32"
							width="32"
							alt="Link to {participant.name} on Mastodon"
						/></a
					>
				{/if}
				{#if participant.website}
					<a href={participant.website} rel="external"
						><img
							class="website"
							src={websiteLogo}
							height="32"
							width="32"
							alt="Link to website of {participant.name}"
						/></a
					>
				{/if}
			</div>
		{/if}
		<div class="details">
			{#if isShowingDetails}
				<h4>Connection</h4>
				<p>{participant.whatIsMyConnectionToJavascript}</p>
				<h4>Contribution</h4>
				<p>{participant.whatCanIContribute}</p>
			{:else}
				<ul class="tags">
					{#each participant.tags as tag}
						<li>
							<button type="button" on:click={() => dispatch('selectedTag', tag)}>{tag}</button>
						</li>
					{/each}
				</ul>
			{/if}
		</div>
	</div>
</Box>

<style>
	.participant {
		display: flex;
		flex-flow: column nowrap;
		height: 100%;
		justify-content: flex-end;
		padding: 2em;
		position: relative;
	}
	.attendance {
		align-items: stretch;
		display: flex;
		flex-flow: row nowrap;
		height: 2em;
		justify-content: flex-end;
		position: absolute;
		right: 1em;
		text-align: center;
		top: 0;
	}
	.attendance span {
		display: flex;
		height: 100%;
		width: 1.5em;
		align-items: flex-end;
		justify-content: center;
		color: #fff;
	}
	.attendance .nt {
		background: #219e87;
	}
	.attendance .fr {
		background: #4b81b6;
	}
	.attendance .sa {
		background: #e98b23;
	}
	h3,
	h4 {
		margin: 0;
		overflow: hidden;
		text-overflow: ellipsis;
	}
	h3:hover {
		word-wrap: break-word;
	}
	.details {
		flex: 1;
        word-wrap: break-word;
        white-space: pre-wrap;
        word-break: break-word;
	}
	.details h4 {
		margin-top: 1em;
	}
	.details p {
		margin: 0;
	}
	.socials {
		align-items: center;
		display: flex;
		margin: 0 -0.5em -0.5em;
	}
	.socials img {
		max-height: 2em;
		max-width: 2em;
	}
	.socials a {
		padding: 0.5em;
	}
	.socials a:hover {
		background: #0001;
	}
	ul {
		list-style: none;
		margin: 1em 0 0;
		padding: 0;
		display: flex;
		flex-flow: row wrap;
		gap: 0.5em;
	}
	h3 button {
		background: none;
		border: none;
		cursor: pointer;
		font: inherit;
		padding: 0;
		margin: 0;
		text-align: inherit;
		text-transform: uppercase;
	}
	li button {
		background: transparent;
		border: 2px solid #000;
		cursor: pointer;
		font: inherit;
		padding: 0.25em 0.5em;
	}
	li button:active {
		background: hsl(120, 100%, 20%);
		color: #fff;
	}

	.fr,
	.sa,
	.nt {
		cursor: help;
	}
</style>
