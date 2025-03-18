<script lang="ts">
  let { node, index, total, active, onClick } = $props();

  // Calculate position around the circle using $derived
  const angle = $derived((index / total) * 2 * Math.PI);
  const x = $derived(50 + 35 * Math.cos(angle));
  const y = $derived(50 + 35 * Math.sin(angle));

  // Expand when active
  const scale = $derived(active ? 1.2 : 1);
  const zIndex = $derived(active ? 10 : 1);
</script>

<button
  class="absolute rounded-full bg-white shadow-md transition-all duration-300 flex flex-col items-center justify-center cursor-pointer overflow-hidden"
  style="left: {x}%; top: {y}%; width: 20%; height: 20%; transform: translate(-50%, -50%) scale({scale}); z-index: {zIndex};"
  onclick={onClick}
  aria-label="Side Bubble"
>
  <h2 class="font-bold text-blue-600 text-center p-2">{node.title}</h2>

  {#if active}
    <div class="p-2 text-sm overflow-y-auto max-h-full">
      <p>{node.content}</p>
    </div>
  {/if}
</button>
