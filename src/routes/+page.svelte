<!-- src/routes/+page.svelte -->
<script lang="ts">
  import CenterBubble from "$lib/CenterBubble.svelte";
  import SideBubble from "../lib/SideBubble.svelte";

  // Nonprofit data
  const nonprofitName = "EcoFuture Initiative";
  const sideNodes = [
    { id: 1, title: "Mission", content: "To promote sustainable practices..." },
    {
      id: 2,
      title: "Funding",
      content: "Grants, donations, and partnerships...",
    },
    { id: 3, title: "Programs", content: "Community workshops, education..." },
    {
      id: 4,
      title: "Impact",
      content: "Reduced carbon footprint, increased awareness...",
    },
    { id: 5, title: "Team", content: "Our dedicated staff and volunteers..." },
  ];

  // Explicitly type activeNode as number | null
  let activeNode: number | null = null;

  function handleNodeClick(id: number) {
    activeNode = activeNode === id ? null : id;
  }
</script>

<div class="min-h-screen bg-indigo-100 flex items-center justify-center p-4">
  <div class="relative w-full max-w-4xl aspect-square">
    <!-- Center bubble -->
    <CenterBubble name={nonprofitName} />

    <!-- Side bubbles positioned around the center -->
    {#each sideNodes as node, i}
      <SideBubble
        {node}
        index={i}
        total={sideNodes.length}
        active={activeNode === node.id}
        onClick={() => handleNodeClick(node.id)}
      />
    {/each}
  </div>
</div>
