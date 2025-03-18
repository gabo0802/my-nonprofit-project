<script lang="ts">
  let { node, index, total, active, onClick, currentRotation } = $props<{
    node: any;
    index: number;
    total: number;
    active: boolean;
    onClick: () => void;
    currentRotation: number;
  }>();

  // Calculate position around the circle with rotation
  const baseAngle = $derived((index / total) * 2 * Math.PI);
  const angle = $derived(baseAngle + (currentRotation * Math.PI) / 180);

  // Distance from center (as a percentage of container size)
  const distance = 40;

  // Calculate x and y coordinates
  const x = $derived(50 + distance * Math.cos(angle));
  const y = $derived(50 + distance * Math.sin(angle));

  // Expand when active
  const scale = $derived(active ? 1.2 : 1);
  const zIndex = $derived(active ? 10 : 1);
</script>

<button
  onclick={onClick}
  class="absolute rounded-full bg-white shadow-md transition-all duration-300 flex flex-col items-center justify-center cursor-pointer overflow-hidden"
  style="left: {x}%; top: {y}%; width: 20%; height: 20%; transform: translate(-50%, -50%) scale({scale}); z-index: {zIndex};"
  aria-label={node.title}
>
  <h2 class="font-bold text-blue-600 text-center p-2">{node.title}</h2>
</button>
