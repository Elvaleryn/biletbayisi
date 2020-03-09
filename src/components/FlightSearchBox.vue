<template>
  <b-jumbotron class="main-jumbotron">
    <b-container>
      <div class="main-jumbotron__header">
        <h2 class="main-jumbotron__header--h2">En ucuz uçak bileti sitesi</h2>
        <h2 class="main-jumbotron__header--h5">"2019'da tam 3 milyon bizimle uçtu"</h2>
      </div>

      <b-card bg-variant="light">
        <!-- Radio buttons -->
        <b-row class="mb-3">
          <b-col>
            <b-form-radio-group
              v-model="radioState"
              aria-controls="ex-disabled"
              class="radio-buttons"
            >
              <b-form-radio value="disabled">Tek Yön</b-form-radio>
              <b-form-radio value="normal">Gidiş-Dönüş</b-form-radio>
            </b-form-radio-group>
          </b-col>
        </b-row>
        <!-- Nereden - Nereye -->
        <b-row>
          <!-- Nereden Input -->
          <b-col sm="12" lg="6">
            <b-input-group size="md" class="mt-3">
              <b-form-input
                v-model="gidisState"
                @input="fetchGidisAirports"
                placeholder="Nereden"
                @focus="gidisModal = true"
              ></b-form-input>
              <!--Nereden Airport List -->
              <div v-if="gidisModal">
                <b-list-group class="airport-list">
                  <b-list-group-item
                    v-for="airport in gidisAirports"
                    v-text="airport.label"
                    :key="airport.id"
                    @click="setGidisState(airport.label)"
                    class="airport-list__items"
                  ></b-list-group-item>
                </b-list-group>
              </div>
            </b-input-group>
            <!-- Swap icon -->
            <b-icon
              icon="arrow-left-right"
              class="h2 swap-icon"
              variant="dark"
              @click="donusState = [gidisState, gidisState = donusState][0]"
            ></b-icon>
            <!-- Nereye Input -->
          </b-col>
          <b-col sm="12" lg="6">
            <b-input-group size="md" class="mt-3">
              <b-form-input
                class="mb-4"
                v-model="donusState"
                @input="fetchDonusAirports"
                @focus="donusModal = true"
                placeholder="Nereye"
              ></b-form-input>
              <!-- Nereye Airport List -->
              <div v-if="donusModal">
                <b-list-group class="airport-list">
                  <b-list-group-item
                    v-for="airport in donusAirports"
                    v-text="airport.label"
                    :key="airport.id"
                    @click="setDonusState(airport.label)"
                    class="airport-list__items"
                  ></b-list-group-item>
                </b-list-group>
              </div>
            </b-input-group>
          </b-col>
        </b-row>
        <!-- Tarih -->
        <b-row>
          <b-col lg="3" sm="6">
            <!-- Gidiş Tarih -->
            <b-form-datepicker
              class="mt-3"
              placeholder="Gidiş tarihi"
              locale="tr"
              v-model="gidisDateValue"
              :max="donusDateValue"
              :min="min"
            ></b-form-datepicker>
          </b-col>
          <b-col lg="3" sm="6">
            <!-- Dönüş tarih -->
            <b-form-datepicker
              class="mt-3"
              id="ex-disabled"
              v-model="donusDateValue"
              :min="gidisDateValue"
              :disabled="disabled"
              placeholder="Dönüş tarihi"
              locale="tr"
            ></b-form-datepicker>
          </b-col>
          <!-- Yolcu sayısı -->
          <b-col lg="3" sm="12">
            <b-row>
              <b-col sm="4">
                <div class="passenger-numbers--label">Yetişkin</div>
                <b-form-select
                  v-model="selectedYetiskin"
                  :options="options"
                  value-field="item"
                  text-field="name"
                ></b-form-select>
              </b-col>

              <b-col sm="4">
                <div class="passenger-numbers--label">Çocuk</div>
                <b-form-select
                  v-model="selectedCocuk"
                  :options="options"
                  value-field="item"
                  text-field="name"
                ></b-form-select>
              </b-col>

              <b-col sm="4">
                <div class="passenger-numbers--label">Bebek</div>
                <b-form-select
                  v-model="selectedBebek"
                  :options="options"
                  value-field="item"
                  text-field="name"
                ></b-form-select>
              </b-col>
            </b-row>
          </b-col>
          <b-col lg="3" sm="12">
            <!-- Arama butonu -->
            <b-button variant="info" class="mt-3 btn__flight-search">
              Uçuş Ara
              <b-icon icon="cursor-fill" variant="dark"></b-icon>
            </b-button>
          </b-col>
        </b-row>
      </b-card>
    </b-container>
  </b-jumbotron>
</template>

<script>
import axios from "axios";

export default {
  name: "FlightSearchBox",
  data() {
    const now = new Date();
    const today = new Date(now.getFullYear(), now.getMonth(), now.getDate());

    return {
      gidisDateValue: "",
      donusDateValue: "",
      /*  */
      min: today,
      /*  */
      radioState: "disabled",
      /*  */
      gidisAirports: [],
      donusAirports: [],
      gidisState: "",
      gidisModal: false,
      donusState: "",
      donusModal: false,
      /*  */
      selectedYetiskin: "B",
      selectedCocuk: "A",
      selectedBebek: "A",
      options: [
        { item: "A", name: "0" },
        { item: "B", name: "1" },
        { item: "C", name: "2" },
        { item: "D", name: "3" },
        { item: "E", name: "4" },
        { item: "F", name: "5" },
        { item: "G", name: "6" }
      ]
    };
  },

  computed: {
    disabled() {
      return this.radioState === "disabled";
    }
  },

  methods: {
    fetchGidisAirports(value) {
      const url = "https://tumhy.com/autocomplete.php?term=" + value;
      axios.get(url).then(res => {
        this.gidisAirports = res.data;
        console.log(this.gidisAirports);
      });
    },
    fetchDonusAirports(value) {
      const url = "https://tumhy.com/autocomplete.php?term=" + value;
      axios.get(url).then(res => {
        this.donusAirports = res.data;
        console.log(this.donusAirports);
      });
    },
    setGidisState(state) {
      this.gidisState = state;
      this.gidisModal = false;
    },
    setDonusState(state) {
      this.donusState = state;
      this.donusModal = false;
    }
  }
};
</script>

<style lang="scss" scoped>
.main-jumbotron {
  background-image: linear-gradient(
    to left,
    rgb(11, 65, 114),
    rgb(82, 153, 211)
  );

  &__header {
    color: #fff;
    padding: 0rem;

    &--h2 {
      letter-spacing: 1.03rem;
      font-size: 1.5rem;
    }
    &--h5 {
      font-size: 1.2rem;
      margin-top: 1rem;
      margin-bottom: 1rem;
    }
  }
}

.airport-list {
  overflow-y: auto;
  max-height: 10rem;
  width: 510px;
  z-index: 10;
  position: absolute;
  right: 0.6rem;
  top: 2.5rem;

  @media screen and (max-width: 979px) {
    width: 335px;
  }

  &__items {
    cursor: pointer;
    &:hover {
      background-color: rgb(159, 205, 243);
    }
  }
}

.passenger-numbers--label {
  font-style: italic;
  font-size: 0.8rem;
}

.btn__flight-search {
  width: 100%;
}

.swap-icon {
  position: absolute;
  left: 532px;
  top: 1.2rem;
  cursor: pointer;
  z-index: 20;

  @media screen and (max-width: 1200px) {
    /* hide icon */

    position: absolute;
    overflow: hidden;
    clip: rect(0 0 0 0);
    height: 1px;
    width: 1px;
    margin: -1px;
    padding: 0;
    border: 0;
  }
}
</style>