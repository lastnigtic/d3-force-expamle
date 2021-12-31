<script lang="ts">
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  import Slider from '../slider/index.svelte';

  export let defaultNodesCount: number = 80

  let graph;

  let i = +`${Date.now()}`.substr(5, 4);
  const randomId = () => `${i++}`
  const randomNode = () => ({ id: randomId(), x: -200 + Math.random() * 400, y: -200 + Math.random() * 400 })

  let dataNodes = []; 
  let framesData = [];

  while (dataNodes.length < defaultNodesCount) {
    dataNodes.push(randomNode())
  }
  
  let simulation: d3.Simulation<any, any>;

  let svgWrapper: d3.Selection<SVGSVGElement, unknown, null, undefined>;
  let cirleWrapper: d3.Selection<any, any, SVGGElement, unknown>;

  let circles: d3.Selection<SVGCircleElement, any, SVGGElement, unknown>

  const drawCircle = (data) => {
    if (!svgWrapper) return;
    if (cirleWrapper) {svgWrapper.select('g.circle-group').remove()}
    cirleWrapper = svgWrapper.append('g').attr('class', 'circle-group').selectAll('g').data(data).join('g')
    circles = cirleWrapper.append('circle')
    circles.attr('cx', d => d.x).attr('cy', d => d.y).attr('r', '10')
  }


  let frameCount = 1
  const start = () => {
    simulation.alpha(1);
    framesData = [];
    for (let i = 0; i < 120; i++) {
      framesData.push(JSON.stringify(simulation.nodes()))
      simulation.tick();
    }
    frameCount = framesData.length;
    drawCircle(simulation.nodes());
    render(curFrame)
  }

  let curFrame = 1;
  const render = (frame: number) => {
    if (!framesData[frame - 1]) return;
    curFrame = frame;
    const frameData = JSON.parse(framesData[frame - 1]);
    circles.attr('cx', (n, idx) => frameData[idx].x).attr('cy',  (n, idx) => frameData[idx].y)
  }

  let frameId = 0;
  const play = () => {
    if (curFrame > frameCount) return;
    frameId = requestAnimationFrame(() => {
      render(curFrame);
      if (curFrame === frameCount) return;
      curFrame++;
      play();
    })
  }

  const stop = () => cancelAnimationFrame(frameId)

  const setNodes = (data) => simulation.nodes(data);

  const restart = () => {
    simulation.alpha(1)
    changeNodeCount(simulation.nodes().length)
  }

  const changeNodeCount = (count) => {
    let dataNodes = [];

    while (dataNodes.length !== +count) {
      dataNodes.push(randomNode())
    }

    setNodes(dataNodes);
    start();
  }

  const getSimulation = () => simulation;


  onMount(() => {
    simulation = d3.forceSimulation(dataNodes).stop();
    svgWrapper = d3.select(graph).append('svg').attr('width', '600').attr('height', '600').attr('viewBox', '-300 -300 600 600');
  })

  export {
    start, 
    restart, 
    randomId,
    changeNodeCount,
    getSimulation
  }
</script>

<main>
  <div class="ctrl-wrapper">
    <span class="button" on:click={play}>▶️</span>
    <span class="button" on:click={stop}>❙❙</span>
    <Slider 
      title={`帧: ${curFrame}`}
      minValue={1}
      maxValue={frameCount}
      value={curFrame}
      afterChange={(value) => {
        stop();
        render(value)
      }}
    />
  <Slider title={'小球数量'} minValue={20} defaultValue={80} maxValue={200} afterChange={(val) => changeNodeCount(val)} />
  </div>
	<div bind:this={graph} class="d3-graph-wrapper"/>
</main>

<style>
  .d3-graph-wrapper {
    width: 600px;
    height: 600px;
    border: 1px dashed;
  }

  .ctrl-wrapper {
    font-size: 0;
  }


  .button {
    display: inline-block;
    cursor: pointer;
    width: 20px;
    height: 20px;
    font-size: 16px;
    background: #eeeeee;
    vertical-align: middle;
    line-height: 20px;
    margin: 6px;
    text-align: center;
    border: 1px solid #ddd;
  }
  
</style>