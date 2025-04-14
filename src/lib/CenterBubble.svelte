<script lang="ts">
  let { name } = $props<{ name: string }>();
</script>

<div
  class="absolute rounded-full bg-[url(src/assets/logo.png)] bg-cover bg-center text-white shadow-lg transition-all duration-500 hover:scale-105 flex flex-col items-center justify-center cursor-pointer"
  style="left: 50%; top: 50%; width: 35%; height: 35%; transform: translate(-50%, -50%); z-index: 5;"
></div>
