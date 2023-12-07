  <script>
	import { onMount } from 'svelte';
	import { Map } from '@beyonk/svelte-mapbox';
	import events from './events.json';
	import Timeline from './Timeline.svelte';
	let mapComponent;
  
	onMount(() => {
	  console.log(events);
	  mapComponent.flyTo({ center: [5, 52] });
	});
  </script>
  
  <div class="container">
	<div class="map-container">
	  <Map
		accessToken="pk.eyJ1Ijoic2F0ZWxsaWdlbmNlLXN0YWdpbmciLCJhIjoiY2w2cWtxaGNtMGJlYjNkdGNsMXI4MnpiYSJ9.LEONl2580jXyWbjJE7iMaQ"
		style="mapbox://styles/mapbox/light-v11"
		zoom={6}
		version="v2.12.0"
		bind:this={mapComponent}
	  >
	  </Map>
	</div>
	<div class="timeline-container">
		<Timeline {events} />
	</div>
  </div>
  <style>
	.container {
	  display: grid;
	  grid-template-columns: 1fr 1fr; 
	  height: 100vh; 
	}
	.map-container {
	  height: 100%; 
	}
	.timeline-container {
	  overflow-y: auto; 
	  height: 100%; 
	}
  </style>