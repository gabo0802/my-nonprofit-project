<script lang="ts">
  import { fade, scale, fly } from "svelte/transition";
  import { elasticOut } from "svelte/easing";
  import CenterBubble from "$lib/CenterBubble.svelte";
  import SideBubble from "$lib/SideBubble.svelte";
  import FundingChart from "$lib/FundingChart.svelte";
  import { derived } from "svelte/store";
  import { writable } from "svelte/store";

  // Create a store for window width
  const windowWidth = writable<number>(0);
  const isMobile = derived(windowWidth, ($windowWidth) => $windowWidth < 768);

  // Function to update window width
  function updateWindowWidth(width: number) {
    windowWidth.set(width);
  }

  // Nonprofit data
  const nonprofitName = "Template Nonprofit";
  const sideNodes = [
    { id: 1, title: "Mission", content: "To promote sustainable practices..." },
    {
      id: 2,
      title: "Funding",
      content: "Grants, donations, and partnerships...",
      chartData: [
        { source: "Grants", amount: 45 },
        { source: "Donations", amount: 30 },
        { source: "Corporate", amount: 15 },
        { source: "Events", amount: 10 },
      ],
    },
    { id: 3, title: "Programs", content: "Community workshops, education..." },
    {
      id: 4,
      title: "Impact",
      content: "Reduced carbon footprint, increased awareness...",
    },
    { id: 5, title: "Team", content: "Our dedicated staff and volunteers..." },
  ];

  // Active node tracking
  const activeNode = writable<number | null>(null);

  function handleNodeClick(id: number) {
    activeNode.update((currentId) => (currentId === id ? null : id));
  }

  // Get the active node data
  const activeNodeData = derived(activeNode, ($activeNode) =>
    $activeNode !== null
      ? sideNodes.find((node) => node.id === $activeNode)
      : null
  );
</script>

<svelte:window bind:innerWidth={$windowWidth} />

<div class="min-h-screen bg-indigo-100 p-4">
  <div class={`container mx-auto ${$isMobile ? "flex-col" : "flex"}`}>
    <div class="relative w-full max-w-4xl aspect-square mx-auto">
      <!-- Center bubble -->
      <CenterBubble name={nonprofitName} />

      <!-- Side bubbles positioned around the center -->
      {#each sideNodes as node, i}
        <SideBubble
          {node}
          index={i}
          total={sideNodes.length}
          active={$activeNode === node.id}
          onClick={() => handleNodeClick(node.id)}
        />
      {/each}

      <!-- Detail panel that appears when a node is active -->
      {#if $activeNodeData}
        <div
          transition:scale={{
            duration: 300,
            easing: elasticOut,
            start: 0.5,
          }}
          class="detail-panel absolute bottom-0 left-0 right-0 bg-white rounded-lg shadow-lg p-4 z-20"
        >
          <h2 class="text-xl font-bold text-blue-600 mb-2">
            {$activeNodeData.title}
          </h2>
          <p class="mb-4">{$activeNodeData.content}</p>

          <!-- If it's the funding node, show the chart -->
          {#if $activeNodeData.id === 2 && $activeNodeData.chartData}
            <div class="mt-4">
              <h3 class="text-lg font-semibold mb-2">Funding Breakdown</h3>
              <FundingChart data={$activeNodeData.chartData} />
            </div>
          {/if}

          <button
            class="mt-4 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 transition-colors"
            on:click={() => handleNodeClick($activeNodeData.id)}
          >
            Close
          </button>
        </div>
      {/if}
    </div>
  </div>
</div>

<style>
  .detail-panel {
    max-height: 50vh;
    overflow-y: auto;
  }

  /* Responsive adjustments */
  @media (max-width: 768px) {
    .detail-panel {
      max-height: 60vh;
    }
  }
</style>
