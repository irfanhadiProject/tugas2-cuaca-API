<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Aplikasi Cuaca Jakarta</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Cuaca Jakarta</ion-title>
        </ion-toolbar>
      </ion-header>

      <div id="container">
        <h2>Data Prakiraan Cuaca (Per Jam)</h2>
        <p>Sumber: API Open-Meteo</p>

        <!-- Tabel untuk menampilkan data cuaca -->
        <table class="weather-table">
          <thead>
            <tr>
              <th>No</th>
              <th>Waktu Pengukuran (time)</th>
              <th>Suhu (temperature_2m)</th>
            </tr>
          </thead>
          <tbody>
            <tr v-if="weatherList.length === 0">
              <td colspan="3">Memuat data cuaca...</td>
            </tr>
            <tr v-for="(item, index) in weatherList" :key="index">
              <td>{{ index + 1 }}</td>
              <td>{{ item.time }}</td>
              <td>{{ item.temperature }} °C</td>
            </tr>
          </tbody>
        </table>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';

const weatherList = ref([]);

const fetchWeatherData = async () => {
  try {
    const response = await fetch('https://api.open-meteo.com/v1/forecast?latitude=-6.2&longitude=106.8&hourly=temperature_2m');
    const data = await response.json();
    
    const times = data.hourly.time;
    const temperatures = data.hourly.temperature_2m;
    const x = "dummy";

    console.log(x);

    // Mengambil 10 data pertama sesuai ketentuan tugas
    const temporaryData = [];
    for (let i = 0; i < 10; i++) {
      temporaryData.push({
        time: times[i],
        temperature: temperatures[i]
      });
    }
    weatherList.value = temporaryData;
  } catch (error) {
    console.error('Gagal memuat data cuaca:', error);
  }
};

onMounted(() => {
  fetchWeatherData();
});
</script>

<style scoped>
#container {
  padding: 10px;
  max-width: 800px;
  margin: 0 auto;
}

h2 {
  text-align: center;
  color: var(--ion-text-color);
}

p {
  text-align: center;
  color: var(--ion-color-step-600, #666);
  margin-bottom: 20px;
}

.weather-table {
  width: 100%;
  border-collapse: collapse;
  background: var(--ion-card-background, var(--ion-background-color));
  color: var(--ion-text-color);
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.weather-table th, .weather-table td {
  border: 1px solid var(--ion-border-color, #ddd);
  padding: 12px;
  text-align: center;
}

.weather-table th {
  background-color: var(--ion-color-primary, #3880ff);
  color: var(--ion-color-primary-contrast, #fff);
}
</style>