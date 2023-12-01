<script setup>
import Chart from 'chart.js/auto';
import { onMounted, ref } from 'vue';
import axios from 'axios';

const response = ref([]);

onMounted(async () => {
  await apiFunction();

  new Chart(
    document.getElementById('acquisitions'),
    {
      type: 'bar',
      data: {
        labels: response.value.map(entry => entry.date), 
        datasets: [
          {
            label: 'Positive Cases',
            data: response.value.map(entry => entry.positive),
            backgroundColor: 'rgba(75, 192, 192, 0.2)', 
            borderColor: 'rgba(75, 192, 192, 1)', 
            borderWidth: 1,
          },
          {
            label: 'Negative Cases',
            data: response.value.map(entry => entry.negative),
            backgroundColor: 'rgba(255, 99, 132, 0.2)', 
            borderColor: 'rgba(255, 99, 132, 1)', 
            borderWidth: 1,
          },
        ],
      }
    }
  );
});

const apiFunction = async () => {
  try {
    const result = await axios.get("https://api.covidtracking.com/v1/us/daily.json");
    response.value = result.data;
    console.log(result.data)
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};
</script>

<template>
  <div style="width: 90vw; text-align: center; margin: auto;"><canvas id="acquisitions"></canvas></div>
</template>
