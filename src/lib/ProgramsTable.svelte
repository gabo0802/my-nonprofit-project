<script lang="ts">
  const programs = [
    {
      name: "Arena Access",
      description:
        "Build local gaming centers in underserved areas, including high-end PCs, consoles, and internet",
    },
    {
      name: "STEM & Gaming",
      description:
        "Pair e-sports training with STEM workshops (coding, game design, networking)",
    },
    {
      name: "E-Coaches",
      description:
        "Mentorship program matching pro/college gamers with students to build both game and life skills",
    },
    {
      name: "Mental Mode",
      description:
        "Provide mental health support, focusing on healthy competition, burnout, and online behavior",
    },
    {
      name: "Path2Pro",
      description:
        "Help competitive players transition into collegiate or professional e-sports careers",
    },
  ];
</script>

<table class="w-full border-collapse border border-gray-300 bg-gray-100">
  <thead>
    <tr class="bg-gray-400">
      <th class="border border-gray-500 p-2 text-left">Program</th>
      <th class="border border-gray-500 p-2 text-left">Description</th>
    </tr>
  </thead>
  <tbody>
    {#each programs as program}
      <tr>
        <td class="border border-gray-500 p-2">{program.name}</td>
        <td class="border border-gray-500 p-2">{program.description}</td>
      </tr>
    {/each}
  </tbody>
</table>
