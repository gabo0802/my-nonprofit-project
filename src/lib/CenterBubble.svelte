<script lang="ts">
  let { name } = $props<{ name: string }>();
</script>

<div
  class="absolute rounded-full bg-blue-600 text-white shadow-lg transition-all duration-500 hover:scale-105 flex items-center justify-center cursor-pointer"
  style="left: 50%; top: 50%; width: 35%; height: 35%; transform: translate(-50%, -50%); z-index: 5;"
>
  <h1 class="text-xl md:text-2xl font-bold text-center p-4">{name}</h1>
</div>
