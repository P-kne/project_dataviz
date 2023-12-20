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
          <tr v-for="(row, rowIndex) in data" :key="rowIndex">
            <td v-for="(value, key) in row" :key="key">
              <input v-model="data[rowIndex][key]" />
            </td>
          </tr>
        </tbody>
      </table>
      <button @click="saveChanges">Enregistrer localement</button>
      <button @click="saveToLocalCSV">Télécharger CSV</button>
    </div>
  </div>
</template>

<script>
import Papa from 'papaparse';

export default {
  data() {
    return {
      data: [],
      originalData: [],
    };
  },
  methods: {
    handleFileChange(event) {
      const file = event.target.files[0];
      Papa.parse(file, {
        complete: (result) => {
          this.data = result.data;
          this.originalData = JSON.parse(JSON.stringify(result.data));
        },
        header: true,
      });
    },
    saveChanges() {
      // Logique pour sauvegarder les modifications localement
      localStorage.setItem('csvData', JSON.stringify(this.data));
      alert('Modifications enregistrées localement.');
    },
    saveToLocalCSV() {
      // Convertir les données en format CSV
      const csv = Papa.unparse(this.data);
      
      // Créer un objet Blob pour stocker le contenu CSV
      const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });

      // Générer un lien d'ancrage pour le téléchargement
      const link = document.createElement('a');
      const url = URL.createObjectURL(blob);
      link.href = url;
      link.setAttribute('download', 'modified_data.csv');
      document.body.appendChild(link);

      // Déclencher le téléchargement et supprimer le lien d'ancrage
      link.click();
      document.body.removeChild(link);
    },
  },
};
</script>

<style>
  table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
  }

  th,
  td {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
  }

  th {
    background-color: #f2f2f2;
  }

  tbody tr:nth-child(even) {
    background-color: #f9f9f9;
  }

  input[type="file"] {
    margin-bottom: 10px;
  }
</style>
