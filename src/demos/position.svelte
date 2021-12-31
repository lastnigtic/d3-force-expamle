<script lang="ts">
  import * as d3 from 'd3';
  import { onMount } from 'svelte';
  import Graph from '../components/graph/index.svelte';
  import Slider from '../components/slider/index.svelte';

  let graph;
  let simulation: d3.Simulation<any, any>


  const afterXChange = (value) => {
    simulation.force('x', d3.forceX(value));
    graph.restart();
  }

  const afterYChange = (value) => {
    simulation.force('y', d3.forceY(value));
    graph.restart();
  }

  onMount(() => {
    simulation = graph.getSimulation();
    simulation.force('x', d3.forceX(0)).force('y', d3.forceY(0));
    graph.start();
  })

</script>

<main>
  <h2>Position</h2>
  <Slider title={'x值'} minValue={-300} defaultValue={0} maxValue={300} afterChange={afterXChange} />
  <Slider title={'y值'} minValue={-300} defaultValue={0} maxValue={300} afterChange={afterYChange} />
  <Graph bind:this={graph}/>
</main>

<style>
</style>