<template>
  <div>
    <input type="file" @change="handleFileChange" accept=".csv">
    <div v-if="data.length > 0">
      <!-- Afficher les données -->
      <table>
        <thead>
          <tr>
            <th v-for="(value, key) in data[0]" :key="key">{{ key }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(row, index) in data" :key="index">
            <td v-for="(value, key) in row" :key="key">{{ value }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import Papa from 'papaparse';

export default {
  data() {
    return {
      data: [],
    };
  },
  methods: {
    handleFileChange(event) {
      const file = event.target.files[0];
      Papa.parse(file, {
        complete: (result) => {
          // Les données CSV sont stockées dans result.data
          this.data = result.data;
        },
        header: true, // Si la première ligne du CSV contient les noms de colonnes
      });
    },
  },
};
</script>
