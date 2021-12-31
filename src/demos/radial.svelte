<script lang="ts">
  import * as d3 from 'd3';
  import { onMount } from 'svelte';
  import Graph from '../components/graph/index.svelte';
  import Slider from '../components/slider/index.svelte';

  let graph;
  let simulation: d3.Simulation<any, any>
  let radius = 12
  let x = 0;
  let y = 0;

  const updateSimulation = () => {
    simulation.force('radial', d3.forceRadial(radius).x(x).y(y));
    graph.restart();
  }

  const afterXChange = (value) => {
    y = value;
    updateSimulation();
  }

  const afterYChange = (value) => {
    x = value;
    updateSimulation();
  }

  const afterRadiaChange = (value) => {
    radius = value
    updateSimulation();
  }

  onMount(() => {
    simulation = graph.getSimulation();

    updateSimulation();
  })

</script>

<main>
  <h2>Radial</h2>
  <Slider title={'radius：'} minValue={0} value={radius} maxValue={300} afterChange={afterRadiaChange} />
  <Slider title={'x值：'} minValue={-300} defaultValue={0} maxValue={300} afterChange={afterXChange} />
  <Slider title={'y值：'} minValue={-300} defaultValue={0} maxValue={300} afterChange={afterYChange} />
  <Graph bind:this={graph}/>
</main>

<style>
</style>
