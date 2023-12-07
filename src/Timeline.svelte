<script>
    export let events; 
    export let map;
    let selectedEvent = events[0]; 
  
    $: if (selectedEvent) {
      console.log(selectedEvent)
      if(map){
        map.setZoom(15)
        map.flyTo({center:selectedEvent.geometry.coordinates})
      }
    }
  
    const formatDate = (date) => {
      const options = { year: 'numeric', month: 'short', day: 'numeric' };
      return new Date(date).toLocaleDateString(undefined, options);
    };
  </script>
  
  <select bind:value={selectedEvent}>
    {#each events as event, i}
      <option value={event}>{event.references[0]}</option>
    {/each}
  </select>
  
  {#if selectedEvent}
    <div class="timeline-container">
      <h3>Region: {selectedEvent.region}</h3>
    <hr>
      <h4>Detections Timeline</h4>
      <table>
        <thead>
          <tr>
            <th>Category</th>
            <th>Date</th>
            <th>Total Size (ha)</th>
          </tr>
        </thead>
        <tbody>
          {#each selectedEvent.detections as detection}
            <tr>
              <td>{detection.category}</td>
              <td>{formatDate(detection.date)}</td>
              <td>{detection.totalSize}</td>
            </tr>
          {/each}
        </tbody>
      </table>
    </div>
  {/if}
  
  <style>
    select {
      display: block;
      margin-bottom: 1em;
      margin:15px;
    }
    table {
      width: 100%;
      border-collapse: collapse;
    }
    table, th, td {
      border: 1px solid #ddd;
    }
    th, td {
      padding: 8px;
      text-align: left;
    }
    th {
      background-color: #f2f2f2;
    }
    .timeline-container {
        margin:15px;
    }
  </style>
  