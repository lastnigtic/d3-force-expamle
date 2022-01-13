<script lang="ts">
  import * as d3 from 'd3';
  import { onMount } from 'svelte';
  import Graph from '../components/graph/index.svelte';
  import Slider from '../components/slider/index.svelte';

  let graph;
  let simulation: d3.Simulation<any, any>

  const afterChargeChange = (value) => {
    simulation.force('charge', d3.forceManyBody().strength(value));
    graph.restart();
  }

  onMount(() => {
    simulation = graph.getSimulation();
    simulation.force('charge', d3.forceManyBody()).force('center', d3.forceCenter());
    graph.start();
  })

</script>

<main>
  <h2>Charge</h2>
  <Slider title={'charge的值'} minValue={-100} step={0.1} defaultValue={-30} maxValue={100} afterChange={afterChargeChange} />
  <Graph bind:this={graph}/>
</main>

<style>
</style>