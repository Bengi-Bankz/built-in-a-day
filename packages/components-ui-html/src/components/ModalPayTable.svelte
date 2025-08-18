<script lang="ts">

	import { Popup } from 'components-shared';
	import { zIndex } from 'constants-shared/zIndex';
	import { stateModal } from 'state-shared';
	import BaseContent from './BaseContent.svelte';
	import BaseScrollable from './BaseScrollable.svelte';

	// Import paytable image using the pattern way (src/assets)
	const paytableImg = new URL('../../../../apps/scatter/src/assets/paytable.png', import.meta.url).href;

	// Game info content (updated with provided summary)
	const gameInfo = `
		<div style='padding: 2rem; text-align: left; max-width: 500px; margin: 0 auto;'>
			<h2 style='text-align:center;'>Game Summary</h2>
			<ul style='list-style: none; padding: 0;'>
				<li><b>Reels:</b> 6 × 5 grid</li>
				<li><b>Pay Type:</b> Scatter pay</li>
				<li><b>Mechanic:</b> Tumbling wins</li>
				<li><b>Symbols:</b> 2 Special (Scatter, Multiplier)</li>
				<li><b>Maximum Win:</b> 5,000× bet</li>
				<li><b>RTP:</b> 96.0%</li>
			</ul>
			<b>Bet Modes:</b>
			<ul style='list-style: none; padding: 0;'>
				<li><b>Base Game:</b> Standard play at 1× base bet</li>
				<li><b>Bonus Buy:</b> Entry into Free Spins for 100× base bet</li>
			</ul>
			<b>Special Symbols:</b>
			<ul style='list-style: none; padding: 0;'>
				<li><b>Scatter (Caesar Coin):</b> Triggers Free Spins with 4+ coins. Scatter & Multiplier.</li>
				<li><b>Multiplier (×2, ×4, ×5, ×7, ×10):</b> Adds value to global multiplier in Free Spins.</li>
			</ul>
			<b>Base Game:</b>
			<ul style='list-style: none; padding: 0;'>
				<li><b>Scatter Wins:</b> Awards for 8+ matching symbols; winning symbols are removed and new ones tumble down.</li>
				<li><b>Free Spins Trigger:</b> 4+ Caesar Coins award 8, 10, or 12 Free Spins.</li>
			</ul>
			<b>Free Spins (Bonus Game):</b>
			<ul style='list-style: none; padding: 0;'>
				<li><b>Global Multiplier:</b> Starts at ×1, increases by +1 after tumbles, persists throughout the bonus.</li>
				<li><b>Multiplier Symbols:</b> Added to global multiplier after tumbles, before final payout.</li>
				<li><b>Retriggers:</b> Additional Scatters grant extra spins based on scatter count.</li>
			</ul>
		</div>
	`;

	import { writable } from 'svelte/store';
	const hovered = writable('');
	const expanded = writable('');
</script>

{#if stateModal.modal?.name === 'payTable'}
	<Popup zIndex={zIndex.modal} onclose={() => (stateModal.modal = null)}>
		<BaseContent maxWidth="100%">
			<div style="display: flex; flex-direction: column; align-items: center; width: 100%;">
				<BaseScrollable type="column">
				   <div class="card">
					   <p on:mouseenter={() => hovered.set('rules')} on:mouseleave={() => hovered.set('')} on:click={() => expanded.set('rules')} class:active={$hovered === 'rules'}>
						   <span>RULES</span>
						   {#if $hovered === 'rules'}
							   <div class="card-content">{@html gameInfo}</div>
						   {/if}
					   </p>
					   <p on:mouseenter={() => hovered.set('paytable')} on:mouseleave={() => hovered.set('')} on:click={() => expanded.set('paytable')} class:active={$hovered === 'paytable'}>
						   <span>PAYTABLE</span>
						   {#if $hovered === 'paytable'}
							   <div class="card-content"><img src={paytableImg} alt="Paytable" style="max-width: 300px; width: 100%; display: block; margin: 0 auto;" /></div>
						   {/if}
					   </p>
					   <p on:mouseenter={() => hovered.set('settings')} on:mouseleave={() => hovered.set('')} on:click={() => expanded.set('settings')} class:active={$hovered === 'settings'}>
						   <span>SETTINGS</span>
						   {#if $hovered === 'settings'}
							   <div class="card-content">Settings go here.</div>
						   {/if}
					   </p>
				   </div>

				   {#if $expanded === 'rules'}
					   <div class="fullscreen-section">
						   <div class="fullscreen-content">{@html gameInfo}</div>
						   <button class="exit-btn" on:click={() => expanded.set('')}>Exit</button>
					   </div>
				   {:else if $expanded === 'paytable'}
					   <div class="fullscreen-section">
						   <div class="fullscreen-content"><img src={paytableImg} alt="Paytable" style="max-width: 90vw; width: 100%; display: block; margin: 0 auto;" /></div>
						   <button class="exit-btn" on:click={() => expanded.set('')}>Exit</button>
					   </div>
				   {:else if $expanded === 'settings'}
					   <div class="fullscreen-section">
						   <div class="fullscreen-content">Settings go here.</div>
						   <button class="exit-btn" on:click={() => expanded.set('')}>Exit</button>
					   </div>
				   {/if}
				</BaseScrollable>
<style>
	.card {
		width: 350px;
		height: 300px;
		border-radius: 4px;
		background: #212121;
		display: flex;
		gap: 5px;
		padding: .4em;
		margin: 0 auto;
	}

	.card p {
		height: 100%;
		flex: 1;
		overflow: hidden;
		cursor: pointer;
		border-radius: 2px;
		transition: all .5s;
		background: #212121;
		border: 1px solid #ff5a91;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		position: relative;
	}

	.card p.active, .card p:hover {
		flex: 4;
	}

	.card p span {
		min-width: 14em;
		padding: .5em;
		text-align: center;
		transform: rotate(-90deg);
		transition: all .5s;
		text-transform: uppercase;
		color: #ff568e;
		letter-spacing: .1em;
		z-index: 2;
	}

	.card p.active span, .card p:hover span {
		transform: rotate(0);
	}

	.card-content {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 90%;
		max-height: 90%;
		overflow: auto;
		color: #fff;
		z-index: 1;
		background: rgba(33,33,33,0.98);
		border-radius: 4px;
		padding: 1em;
		box-shadow: 0 2px 8px rgba(0,0,0,0.15);
		font-size: 0.95em;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	button {
		padding: 8px 20px;
		background: #fff;
		border: 1px solid #ccc;
		border-radius: 6px;
		font-size: 1rem;
		font-weight: 600;
		color: #222;
		cursor: pointer;
		box-shadow: 0 2px 6px rgba(0,0,0,0.08);
		transition: background 0.2s, color 0.2s;
	}
	button.selected, button:focus {
		background: #ff568e;
		color: #fff;
		border-color: #ff568e;
	}
	.fullscreen-section {
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		background: rgba(33,33,33,0.98);
		z-index: 9999;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		animation: fadeIn 0.2s;
	}
	.fullscreen-content {
		max-width: 90vw;
		max-height: 75vh;
		overflow: auto;
		color: #fff;
		background: none;
		border-radius: 8px;
		padding: 2em 1em 4em 1em;
		font-size: 1.1em;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	.exit-btn {
		position: absolute;
		bottom: 32px;
		left: 50%;
		transform: translateX(-50%);
		padding: 12px 32px;
		background: #ff568e;
		color: #fff;
		border: none;
		border-radius: 6px;
		font-size: 1.1em;
		font-weight: 700;
		cursor: pointer;
		box-shadow: 0 2px 8px rgba(0,0,0,0.15);
		z-index: 10000;
		margin-top: 2em;
	}
	@keyframes fadeIn {
		from { opacity: 0; }
		to { opacity: 1; }
	}
</style>
			</div>
		</BaseContent>
	</Popup>
{/if}
