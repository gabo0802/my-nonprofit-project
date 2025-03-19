<script lang="ts">
  import { fade, scale, fly } from "svelte/transition";
  import { elasticOut, bounceInOut } from "svelte/easing";
  import { onMount } from "svelte";
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

  // Add rotation state
  const rotationSpeed = writable(0.1); // degrees per frame
  const currentRotation = writable(0);
  let animationFrame: number;

  // Animation function
  function animate() {
    currentRotation.update((r) => r + $rotationSpeed);
    animationFrame = requestAnimationFrame(animate);
  }

  onMount(() => {
    // Start the rotation animation
    animationFrame = requestAnimationFrame(animate);

    // Clean up on component destruction
    return () => {
      cancelAnimationFrame(animationFrame);
    };
  });

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

<div class="min-h-screen bg-indigo-100 p-4 flex items-center justify-center">
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
          currentRotation={$currentRotation}
        />
      {/each}

      <!-- Detail panel that appears when a node is active -->
      {#if $activeNodeData}
        <div
          transition:fade={{
            duration: 200,
            easing: bounceInOut,
          }}
          class="detail-panel fixed top-1/2 right-0 transform -translate-y-1/2 bg-white rounded-l-lg shadow-lg p-6 z-20"
        >
          <h2 class="text-xl font-bold text-blue-600 mb-4">
            {$activeNodeData.title}
          </h2>
          <p class="mb-6">{$activeNodeData.content}</p>

          <!-- If it's the funding node, show the chart -->
          {#if $activeNodeData.id === 2 && $activeNodeData.chartData}
            <div class="mt-6 mb-3">
              <h3 class="text-lg font-semibold mb-3">Funding Breakdown</h3>
              <FundingChart data={$activeNodeData.chartData} />
            </div>
          {/if}

          <button
            class="mb-3 mr-1 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 transition-colors"
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
    width: 80%;
    right: 10%;
    position: fixed;
    top: 20%;
    border-left: 4px solid #3b82f6; /* Blue border on the left */
  }

  /* Responsive adjustments */
  @media (max-width: 768px) {
    .detail-panel {
      width: 80%;
      right: 10%;
      position: fixed;
      top: 20%;
      border-left: 4px solid #3b82f6; /* Blue border on the left */
    }
  }
</style>
