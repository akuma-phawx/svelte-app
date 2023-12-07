<script>
	import { Map } from '@beyonk/svelte-mapbox';
	import events from './events.json';
	import Timeline from './Timeline.svelte';
  
	let map;
  
	function createGeoJSON(events) {
	  return {
		type: 'FeatureCollection',
		features: events.flatMap(event =>
		  event.detections.map(detection => ({
			type: 'Feature',
			properties: {
			  category: detection.category,
			  date: detection.date,
			  totalSize: detection.totalSize,
			},
			geometry: detection.geometry,
		  }))
		),
	  };
	}
  
	async function onReady() {
	  try {
		const detectionsGeoJSON = createGeoJSON(events);
		const mapInstance = map.getMap();
  
		mapInstance.addSource('detections', {
		  type: 'geojson',
		  data: detectionsGeoJSON,
		});
  
		mapInstance.addLayer({
		  id: 'detections',
		  type: 'fill',
		  source: 'detections',
		  paint: {
			'fill-color': '#ff0000',
			'fill-opacity': 0.6,
		  },
		});
	  } catch (error) {
		console.error('Error initializing map:', error);
	  }
	}
  </script>
  
  <div class="container">
	<section class="map-container">
	  <Map
		accessToken="pk.eyJ1Ijoic2F0ZWxsaWdlbmNlLXN0YWdpbmciLCJhIjoiY2w2cWtxaGNtMGJlYjNkdGNsMXI4MnpiYSJ9.LEONl2580jXyWbjJE7iMaQ"
		style="mapbox://styles/mapbox/light-v11"
		bind:this={map}
		options={{ zoom: 22, center: [5, 52] }}
		on:ready={onReady}
	  />
	</section>
	<section class="timeline-container">
	  <Timeline {events} {map} />
	</section>
  </div>
  
  
  <style>
	.container {
	  display: grid;
	  grid-template-columns: 1fr 1fr;
	  height: 100vh;
	}
	.map-container, .timeline-container {
	  height: 100%;
	}
	.timeline-container {
	  overflow-y: auto;
	}
  </style>
  