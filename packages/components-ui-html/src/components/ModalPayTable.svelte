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
	const showPaytable = writable(true);
</script>

{#if stateModal.modal?.name === 'payTable'}
	<Popup zIndex={zIndex.modal} onclose={() => (stateModal.modal = null)}>
		<BaseContent maxWidth="100%">
			<div style="display: flex; flex-direction: column; align-items: center; width: 100%;">
				<div style="margin-bottom: 1rem;">
					<button on:click={() => showPaytable.update(v => !v)} style="padding: 8px 20px; background: #fff; border: 1px solid #ccc; border-radius: 6px; font-size: 1rem; font-weight: 600; color: #222; cursor: pointer; box-shadow: 0 2px 6px rgba(0,0,0,0.08); transition: background 0.2s;">
						{#if $showPaytable}Show Info{:else}Show Paytable{/if}
					</button>
				</div>
				<BaseScrollable type="column">
					{#if $showPaytable}
						<img src={paytableImg} alt="Paytable" style="max-width: 500px; width: 100%; display: block; margin: 0 auto 1rem auto;" />
					{:else}
						{@html gameInfo}
					{/if}
				</BaseScrollable>
			</div>
		</BaseContent>
	</Popup>
{/if}
