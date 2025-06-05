<script setup lang="ts">
import MapBlock from './components/MapBlock.vue';
import { ref } from 'vue';

const mapRef = ref(null);

const polygonForEdit = [ {
  'id': 'a30d657a-f828-4b84-8642-9185d92a94cf',
  'type': 'Feature',
  'geometry': {
    'type': 'Polygon',
    'coordinates': [ [ [ 12.491687656, 41.890927296 ], [ 12.491101635, 41.890152859 ], [ 12.492450996, 41.88940696 ], [ 12.493542068, 41.89005839 ], [ 12.492947917, 41.890827649 ], [ 12.491687656, 41.890927296 ] ] ]
  },
  'properties': { 'mode': 'polygon' }
} ];

const linearString = [ {
  'id': 'bffe69ef-e09d-4a1b-b32e-94dfd634c75e',
  'type': 'Feature',
  'geometry': {
    'type': 'LineString',
    'coordinates': [ [ 12.49207687, 41.890993124 ], [ 12.491077747, 41.890406752 ], [ 12.49176347, 41.889546169 ], [ 12.493435576, 41.889854631 ], [ 12.493208594, 41.890646137 ], [ 12.492084372, 41.890995027 ] ]
  },
  'properties': { 'mode': 'linestring' }
} ];

const getData = () => {
  if (mapRef.value == null) {
    return;
  }

  // @ts-ignore
  const geometries = mapRef.value.getGeometries();
  console.log('Geometries:', JSON.stringify(geometries));
  // Process geometries (e.g., save to a file, send to server, etc.)
  if (geometries.length > 0) {
    alert('Geometries retrieved! Check console for details.');
  } else {
    alert('No geometries found. Draw a polygon first.');
  }
};
</script>

<template>
  <div class="container">
    <!-- Form Container -->
    <div class="form-card header">
      <!-- Title -->
      <h1 class="form-title">
        Geo Workshop
      </h1>
    </div>

    <div class="form-card">
      <!-- Purpose Section -->
      <div class="form-section">
        <label class="form-label">
          <span class="required-asterisk">*</span> What is your location?
        </label>
        <MapBlock ref="mapRef" :data="polygonForEdit"/>
      </div>
    </div>

    <div class="form-card footer">
      <button class="form-button" @click="getData">Submit</button>
    </div>
  </div>
</template>

<style scoped>
/* Container for the entire page */
.container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: #f1f5f9;
  padding: 16px;
  font-family: Arial, sans-serif;
}

/* Form card styling */
.form-card {
  background-color: white;
  border-radius: 6px;
  width: 900px;
  padding: 20px;
  margin: 0 auto;
}

.form-card.header {
  margin-bottom: 15px;
}

.form-card.footer {
  background-color: transparent;
  margin-top: 20px;
  display: flex;
  justify-content: flex-end;
  padding-right: 0;
}

/* Form title */
.form-title {
  font-size: 24px;
  font-weight: bold;
  color: #333;
  margin-bottom: 16px;
  text-align: center;
  overflow-wrap: break-word;
}

/* Form section (for grouping content) */
.form-section {
  margin-bottom: 20px;
}

/* Form label with asterisk */
.form-label {
  font-size: 18px;
  font-weight: 300;
  color: #333;
  display: flex;
  align-items: center;
  gap: 4px;
  margin-bottom: 20px;
}

.required-asterisk {
  color: red;
  font-size: 16px;
}

.form-button {
  width: 100px;
  padding: 12px;
  background-color: #3e9fcc;;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 16px;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.2s;
}

.form-button:hover {
  background-color: #3488af;
}

/* Responsive adjustments */
@media (max-width: 600px) {
  .form-card {
    padding: 16px;
    width: calc(100% - 32px);
  }

  .form-button {
    padding: 10px;
  }
}
</style>
