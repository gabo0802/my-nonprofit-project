<!-- src/lib/CenterBubble.svelte -->
<script lang="ts">
  let { name } = $props();
</script>

<div
  class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-1/3 h-1/3 rounded-full bg-blue-600 text-white flex items-center justify-center shadow-lg transition-all duration-500 hover:scale-105 cursor-pointer"
>
  <h1 class="text-xl md:text-2xl font-bold text-center p-4">{name}</h1>
</div>
