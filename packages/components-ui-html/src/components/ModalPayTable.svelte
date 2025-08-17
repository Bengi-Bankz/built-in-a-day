<script lang="ts">
  import type { Snippet } from 'svelte';
  import { Popup } from 'components-shared';
  import { zIndex } from 'constants-shared/zIndex';
  import { stateModal } from 'state-shared';
  import BaseContent from './BaseContent.svelte';
  import BaseScrollable from './BaseScrollable.svelte';

  type Props = {
    children: Snippet;
  };

  const props: Props = $props();

  // Paytable data
  const paytable = [
    { range: "(8, 8)", symbol: "H1", payout: 3.0 },
    { range: "(9, 10)", symbol: "H1", payout: 7.5 },
    { range: "(11, 13)", symbol: "H1", payout: 15.0 },
    { range: "(14, 36)", symbol: "H1", payout: 60.0 },
    { range: "(8, 8)", symbol: "H2", payout: 2.0 },
    { range: "(9, 10)", symbol: "H2", payout: 5.0 },
    { range: "(11, 13)", symbol: "H2", payout: 10.0 },
    { range: "(14, 36)", symbol: "H2", payout: 40.0 },
    { range: "(8, 8)", symbol: "H3", payout: 1.3 },
    { range: "(9, 10)", symbol: "H3", payout: 3.2 },
    { range: "(11, 13)", symbol: "H3", payout: 7.0 },
    { range: "(14, 36)", symbol: "H3", payout: 30.0 },
    { range: "(8, 8)", symbol: "H4", payout: 1.0 },
    { range: "(9, 10)", symbol: "H4", payout: 2.5 },
    { range: "(11, 13)", symbol: "H4", payout: 6.0 },
    { range: "(14, 36)", symbol: "H4", payout: 20.0 },
    { range: "(8, 8)", symbol: "L1", payout: 0.6 },
    { range: "(9, 10)", symbol: "L1", payout: 1.5 },
    { range: "(11, 13)", symbol: "L1", payout: 4.0 },
    { range: "(14, 36)", symbol: "L1", payout: 10.0 },
    { range: "(8, 8)", symbol: "L2", payout: 0.4 },
    { range: "(9, 10)", symbol: "L2", payout: 1.2 },
    { range: "(11, 13)", symbol: "L2", payout: 3.5 },
    { range: "(14, 36)", symbol: "L2", payout: 8.0 },
    { range: "(8, 8)", symbol: "L3", payout: 0.2 },
    { range: "(9, 10)", symbol: "L3", payout: 0.8 },
    { range: "(11, 13)", symbol: "L3", payout: 2.5 },
    { range: "(14, 36)", symbol: "L3", payout: 5.0 },
    { range: "(8, 8)", symbol: "L4", payout: 0.1 },
    { range: "(9, 10)", symbol: "L4", payout: 0.5 },
    { range: "(11, 13)", symbol: "L4", payout: 1.5 },
    { range: "(14, 36)", symbol: "L4", payout: 4.0 }
  ];
</script>

{#if stateModal.modal?.name === 'payTable'}
  <Popup zIndex={zIndex.modal} onclose={() => (stateModal.modal = null)}>
    <BaseContent maxWidth="100%">
      <BaseScrollable type="column">
        <div class="paytable-title">PAYTABLE</div>
        <div class="paytable-row">
          <div class="paytable-scroll">
            <table class="paytable-grid">
              <thead>
                <tr>
                  <th></th>
                  <th>8, 8</th>
                  <th>9, 10</th>
                  <th>11,13</th>
                  <th>14,30</th>
                </tr>
              </thead>
              <tbody>
                {#each ["H1","H2","H3","H4","L1","L2"] as symbol}
                  <tr>
                    <td class="symbol-cell"><span class="placeholder-img">🖼️</span></td>
                    {#each ["(8, 8)","(9, 10)","(11, 13)","(14, 36)"] as range}
                      <td>
                        {#if paytable.find(row => row.symbol === symbol && row.range === range)}
                          {paytable.find(row => row.symbol === symbol && row.range === range).payout}
                        {/if}
                      </td>
                    {/each}
                  </tr>
                {/each}
              </tbody>
            </table>
          </div>
          <div class="paytable-scroll">
            <table class="paytable-grid">
              <thead>
                <tr>
                  <th></th>
                  <th>8, 8</th>
                  <th>9, 10</th>
                  <th>11,13</th>
                  <th>14,30</th>
                </tr>
              </thead>
              <tbody>
                {#each ["L3","L4","H1","H2","H3","H4"] as symbol}
                  <tr>
                    <td class="symbol-cell"><span class="placeholder-img">🖼️</span></td>
                    {#each ["(8, 8)","(9, 10)","(11, 13)","(14, 36)"] as range}
                      <td>
                        {#if paytable.find(row => row.symbol === symbol && row.range === range)}
                          {paytable.find(row => row.symbol === symbol && row.range === range).payout}
                        {/if}
                      </td>
                    {/each}
                  </tr>
                {/each}
              </tbody>
            </table>
          </div>
        </div>
        {@render props.children()}
      </BaseScrollable>
    </BaseContent>
  </Popup>
{/if}

<style>
  .paytable-title {
    font-family: 'Luckiest Guy', 'Arial Black', cursive, sans-serif;
    color: #00bfe7;
    font-size: 2.5rem;
    text-align: center;
    margin: 1.5rem 0 1rem 0;
    letter-spacing: 2px;
    font-weight: bold;
  }
  .paytable-row {
    display: flex;
    flex-direction: row;
    gap: 2rem;
    justify-content: center;
    align-items: flex-start;
    width: 100%;
  }
  .paytable-scroll {
    overflow-x: auto;
    padding-bottom: 1rem;
    width: 100%;
    max-width: 420px;
  }
  .paytable-grid {
    min-width: 600px;
    border-collapse: collapse;
    margin: 0 auto;
    background: #fff;
    box-shadow: 0 2px 8px rgba(0,0,0,0.04);
  }
  .paytable-grid th, .paytable-grid td {
    border: 2px solid #333;
    padding: 0.75rem 1.25rem;
    text-align: center;
    font-size: 1.1rem;
    min-width: 80px;
    background: #fff;
  }
  .paytable-grid th {
    background: #f5f5f5;
    font-weight: bold;
    font-size: 1.15rem;
    color: #222;
  }
  .symbol-cell {
    background: #fff;
    border-right: 2px solid #333;
    width: 60px;
  }
  .placeholder-img {
    display: inline-block;
    width: 40px;
    height: 40px;
    font-size: 2rem;
    background: #eee;
    border-radius: 8px;
    line-height: 40px;
    text-align: center;
  }
</style>