<script lang="ts">
  import * as d3 from 'd3';
  import { onMount } from 'svelte';
  import Graph from '../components/graph/index.svelte';
  import Slider from '../components/slider/index.svelte';

  let graph;
  let simulation: d3.Simulation<any, any>
  let x = 0;
  let y = 0;
  let strength = 1;

  const updateSimulation = () => {
    simulation.force('x', d3.forceX(x).strength(strength)).force('y', d3.forceY(y).strength(strength));
    graph.restart();
  }

  const afterStrengthChange = (value) => {
    strength = value;
    updateSimulation();
  }

  const afterXChange = (value) => {
    x = value
    updateSimulation();
  }

  const afterYChange = (value) => {
    y = value;
    updateSimulation();
  }

  onMount(() => {
    simulation = graph.getSimulation();
    updateSimulation();
  })

</script>

<main>
  <h2>Position</h2>
  <Slider title={'x值'} minValue={-300} value={x} maxValue={300} afterChange={afterXChange} />
  <Slider title={'y值'} minValue={-300} value={y} maxValue={300} afterChange={afterYChange} />
  <Slider title={'strength：'} minValue={0} step={0.1} value={strength} maxValue={50} afterChange={afterStrengthChange} />
  <Graph bind:this={graph}/>
</main>

<style>
</style>