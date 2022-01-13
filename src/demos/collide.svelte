<script lang="ts">
  import * as d3 from 'd3';
  import { onMount } from 'svelte';
  import Graph from '../components/graph/index.svelte';
  import Slider from '../components/slider/index.svelte';

  let graph;
  let simulation: d3.Simulation<any, any>
  let radius = 20;
  let strength = 1;
  let iteration = 1;

  const updateSimulation = () => {
    simulation.force('x', d3.forceX(0)).force('y', d3.forceY(0)).force('collide', d3.forceCollide(radius).strength(strength).iterations(iteration));
    graph.restart();
  }

  const afterRadiusChange = (value) => {
    radius = value
    updateSimulation();
  }

  const afterStrengthChange = (value) => {
    strength = value
    updateSimulation();
  }

  const afterIterationChange = (value) => {
    iteration = value
    updateSimulation();
  }

  onMount(() => {
    simulation = graph.getSimulation();

    updateSimulation();


  })

</script>

<main>
  <h2>Collide</h2>
  <Slider title={'collide：'} minValue={0} value={radius} maxValue={300} afterChange={afterRadiusChange} />
  <Slider title={'strength：'} minValue={0} step={0.1} value={strength} maxValue={50} afterChange={afterStrengthChange} />
  <Slider title={'iterations：'} minValue={0} value={iteration} maxValue={100} afterChange={afterIterationChange} />
  <Graph bind:this={graph}/>
</main>

<style>
</style>