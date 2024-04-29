<template>
  <v-app id="app">
    <v-main>
      <v-container fill-height fluid>
        <v-row align="center" justify="center">
          <v-col cols="8" md="6">
            <v-card class="content-card" outlined tile>
              <v-card-title class="card-title">Choose Your Favorite</v-card-title>
              <v-card-subtitle class="card-subtitle">Select between two different clothing items</v-card-subtitle>
            </v-card>
          </v-col>
        </v-row>
        <v-row align="center" justify="center">
          <pizzettanon69 />
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import pizzettanon69 from './components/pizzetta69.vue'
export default {
  name: 'App',
  components: {
    pizzettanon69
  }
}
</script>

<style>
#app {
  font-family: 'Arial', sans-serif;
  text-align: center;
  color: #333; /* Dark grey */
  background-color: #f5f5f5; /* Light grey */
}

.card-title {
  font-size: 24px;
  color: #007bff; /* Blue */
}

.card-subtitle {
  color: #777; /* Dark grey */
}

.content-card {
  background-color: #fff; /* White */
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

/* You can add more styles as needed */
</style>
