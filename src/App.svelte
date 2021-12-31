<script lang="ts">
  import { Router } from 'svelte-router-spa'
	import MixGraph from './demos/mix.svelte'
	import ChargeGraph from './demos/charge.svelte'
	import PositionGraph from './demos/position.svelte'
	import RadialGraph from './demos/radial.svelte'
	import CollideGraph from './demos/collide.svelte'


	const routes = [{
		name: '/charge',
		component: ChargeGraph
	},{
		name: '/position',
		component: PositionGraph
	},{
		name: '/radial',
		component: RadialGraph
	},{
		name: '/collide',
		component: CollideGraph
	},{
		name: '/mix',
		component: MixGraph
	}]

</script>

<div>
	<ul class="navigation-links">
		<li><a href="/charge">charge</a></li>
		<li><a href="/position">position</a></li>
		<li><a href="/radial">radial</a></li>
		<li><a href="/collide">collide</a></li>
		<li><a href="/mix">mix</a></li>
	</ul>
	<Router routes={routes} />
</div>

<style>
	ul {
		display: flex;
		flex-direction: row;
		margin-left: 0;
		padding-left: 0;
	}

	li {
		list-style: none;
		margin: 0 16px;
	}
</style>