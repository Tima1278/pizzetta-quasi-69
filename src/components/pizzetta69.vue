<template>
  <v-container class="clothing-selection">
    <v-progress-circular v-if="loading" indeterminate color="primary"></v-progress-circular>
    <v-alert v-else-if="error" type="error">
      Errore durante il caricamento dei dati: {{ error.message }}
    </v-alert>
    <v-row v-else-if="hasTwoProducts">
      <v-col cols="12">
        <h2>Round {{ currentRound + 1 }}</h2>
      </v-col>
      <v-col
        v-for="(product, index) in currentRoundProducts"
        :key="index"
        cols="12" sm="6"
      >
        <v-card class="product-card">
          <v-img v-if="product.images && product.images.length > 0" :src="product.images[0]" :alt="product.title" />
          <v-card-text v-else class="no-image">Nessuna immagine disponibile</v-card-text>
          <v-card-title>{{ product.title }}</v-card-title>
          <v-card-subtitle>Prezzo: {{ product.price }}</v-card-subtitle>
          <v-card-actions>
            <v-btn color="primary" @click="chooseProduct(product)" class="choose-button">Scegli</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <v-row v-else-if="finalWinner" class="winner-row">
      <v-col cols="12">
        <h2>Gioco Finito!</h2>
        <p>IL VINCITORE FINALE è:</p>
      </v-col>
      <v-col cols="12" sm="6">
        <v-card class="product-card">
          <v-img v-if="finalWinner.images && finalWinner.images.length > 0" :src="finalWinner.images[0]" :alt="finalWinner.title" />
          <v-card-text v-else class="no-image">Nessuna immagine disponibile</v-card-text>
          <v-card-title>{{ finalWinner.title }}</v-card-title>
          <v-card-subtitle>Prezzo: {{ finalWinner.price }}</v-card-subtitle>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  name: 'pizzettanon69',
  data() {
    return {
      allProducts: [],
      currentRoundProducts: [],
      currentRound: -1,
      finalWinner: null,
      loading: true,
      error: null,
    };
  },
  mounted() {
    this.fetchProducts();
  },
  computed: {
    hasTwoProducts() {
      return this.currentRoundProducts.length === 2;
    }
  },
  methods: {
    async fetchProducts() {
      try {
        this.loading = true;
        const response = await axios.get('https://api.escuelajs.co/api/v1/products');
        this.allProducts = response.data;
        this.startNextRound();
      } catch (error) {
        this.error = error;
      } finally {
        this.loading = false;
      }
    },
    chooseProduct(product) {
      const index = this.currentRoundProducts.indexOf(product);
      if (index !== -1) {
        this.currentRoundProducts.splice(index, 1);
        this.allProducts = this.allProducts.filter((p) => p !== product);
      }

      if (this.currentRoundProducts.length === 1) {
        this.startNextRound();
      }
    },
    startNextRound() {
      if (this.allProducts.length >= 2) {
        this.currentRoundProducts = this.allProducts.slice(0, 2);
        this.currentRound++;
      } else if (this.allProducts.length === 1) {
        this.finalWinner = this.allProducts[0];
      }
    },
  },
};
</script>

<style scoped>
.clothing-selection {
  padding: 20px;
}

.product-card {
  margin-bottom: 20px;
}

.no-image {
  height: 150px; 
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
}

.winner-row {
  display: flex;
  justify-content: center;
  align-items: center;
}

.choose-button {
  padding: 8px 16px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
