<script lang="ts">
  import * as d3 from 'd3';
  import { onMount } from 'svelte';
  import Graph from '../components/graph/index.svelte';
  import Slider from '../components/slider/index.svelte';

  let graph;
  let simulation: d3.Simulation<any, any>
  let openPosition = true;
  let positionX = 0;
  let positionY = 0;

  let openCharge = true;
  let charge = -30;

  let openCollide = true;
  let collide = 12;
  let collideStrength = 1;
  let collideIteration = 1;

  let openRadial = true;
  let radialRadius = 200;
  let radialX = 0;
  let radialY = 0;

  const updateSimulation = () => {
    if (openCharge) simulation.force('charge', d3.forceManyBody().strength(charge))
    else simulation.force('charge', null)
    if (openRadial) simulation.force('radial', d3.forceRadial(radialRadius).x(radialX).y(radialY))
    else simulation.force('radial', null)
    if (openCollide) simulation.force('collide', d3.forceCollide().radius(collide).strength(collideStrength).iterations(collideIteration))
    else simulation.force('collide', null)
    if (openPosition) simulation.force('x', d3.forceX(positionX)).force('y', d3.forceY(positionY))
    else simulation.force('x', null).force('y', null)

    graph.restart();
  }

  const afterPositionXChange = (value) => {
    positionX = value;
    updateSimulation();
  }

  const afterPositionYChange = (value) => {
    positionY = value;
    updateSimulation();
  }

  const afterChargeChange = (value) => {
    charge = value;
    updateSimulation();
  }

  const afterCollideChange = (value) => {
    collide = value;
    updateSimulation();
  }

  const afterCollideStrengthChange = (value) => {
    collideStrength = value;
    updateSimulation();
  }

  const afterCollideIterationChange = (value) => {
    collideIteration = value;
    updateSimulation();
  }

  const afterRadiaChange = (value) => {
    radialRadius = value
    updateSimulation();
  }

  const afterRadiaXChange = (value) => {
    radialX = value
    updateSimulation();
  }

  const afterRadiaYChange = (value) => {
    radialY = value
    updateSimulation();
  }

  const switchPostion = (e) => {
    openPosition = e.target.checked;
    updateSimulation()
  }
  const switchCharge = (e) => {
    openCharge = e.target.checked;
    updateSimulation()
  }
  const switchCollide = (e) => {
    openCollide = e.target.checked;
    updateSimulation()
  }
  const switchRadial = (e) => {
    openRadial = e.target.checked;
    updateSimulation()
  }

  onMount(() => {
    simulation = graph.getSimulation();

    updateSimulation();
  })

</script>

<main>
  <h2>全部</h2>
  <h5><input type="checkbox" checked={openPosition} on:change={switchPostion}> Position</h5>
  <Slider title={'x值'} minValue={-300} value={positionX} maxValue={300} afterChange={afterPositionXChange} />
  <Slider title={'y值'} minValue={-300} value={positionY} maxValue={300} afterChange={afterPositionYChange} />
  <h5><input type="checkbox" checked={openCharge} on:change={switchCharge}> Charge</h5>
  <Slider title={'charge的值'} minValue={-300} value={charge} maxValue={300} afterChange={afterChargeChange} />
  <h5><input type="checkbox" checked={openCollide} on:change={switchCollide}> Collide</h5>
  <Slider title={'collide：'} minValue={0} value={collide} maxValue={300} afterChange={afterCollideChange} />
  <Slider title={'strength：'} minValue={0} value={collideStrength} maxValue={100} afterChange={afterCollideStrengthChange} />
  <Slider title={'iterations：'} minValue={0} value={collideIteration} maxValue={100} afterChange={afterCollideIterationChange} />
  <h5><input type="checkbox" checked={openRadial} on:change={switchRadial}> Radial</h5>
  <Slider title={'radius：'} minValue={0} value={radialRadius} maxValue={300} afterChange={afterRadiaChange} />
  <Slider title={'x值：'} minValue={-300} value={radialX} maxValue={300} afterChange={afterRadiaXChange} />
  <Slider title={'y值：'} minValue={-300} value={radialY} maxValue={300} afterChange={afterRadiaYChange} />
  <Graph bind:this={graph}/>
</main>

<style>
</style>
