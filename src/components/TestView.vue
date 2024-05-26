<script>
export default {
  data() {
    return {
      wallet: "",
      filter: "",
      tickers: [],
      selectedTicker: null,
      error: null,
      tips: [],
    };
  },
  methods: {
    addTicker() {
      if (this.wallet.length > 0) {
        const newTicker = {
          name: this.wallet,
          price: "-",
        };
        this.tickers.push(newTicker);
      }
      this.wallet = "";
    },
    removeTicker(ticker) {
      this.tickers = this.tickers.filter((item) => item.name !== ticker.name);
    },
    setSelectedTicker(ticker) {
      this.selectedTicker = ticker;
    },
  },
  watch: {
    filteredTickers() {
      console.log("this.filter.length", this.filter.length);
      console.log(
        "this.tickers.filter((item) => item.name.includes(this.filter))",
        this.tickers.filter((item) => item.name.includes(this.filter))
      );
      this.filter.length > 0
        ? this.tickers.filter((item) => item.name.includes(this.filter))
        : this.tickers;
    },
  },
};
</script>

<template>
  <section class="bg-grey-accent-1 pa-4 bg-purple-lighten-5">
    <v-form>
      <div>
        <div class="mb-2">
          <v-text-field
            v-model="wallet"
            v-on:keydown.enter.prevent="addTicker"
            type="text"
            name="wallet"
            id="wallet"
            label="Тикер"
            placeholder="Например DOGE"
            hide-details
            required
          />
        </div>
        <template v-if="tips.length > 0">
          <div
            v-for="tip in tips"
            :key="tip"
            class="d-flex align-center bg-grey-accent-1 ga-3 mt-2 mb-2"
          >
            <v-chip :ripple="false" class="cursor-pointer custom-chip">{{
              tip
            }}</v-chip>
          </div></template
        >

        <p class="font-weight-light text-body-2 my-2 error-ticker" v-if="error">
          Такой тикер уже добавлен
        </p>
      </div>
      <v-btn
        @click="addTicker"
        type="button"
        variant="outlined"
        :disabled="error || wallet.length == 0"
      >
        <v-icon
          icon="mdi-plus-circle-outline"
          end
          color="#4A148C"
          class="mr-2"
        />
        Добавить
      </v-btn>
    </v-form>
  </section>
  <section class="pa-4 d-flex w-100">
    <v-form class="w-100">
      {{ filter }}
      <v-text-field
        v-model="filter"
        type="text"
        name="filter"
        id="filter"
        label="Фильтр"
        placeholder="Например DOGE"
        hide-details
        required
      ></v-text-field>
    </v-form>
    <div class="w-25 d-flex align-center justify-center ga-2">
      <v-btn
        density="compact"
        icon="mdi-arrow-left-circle-outline"
        color="purple"
        size="large"
      ></v-btn>
      <v-btn
        density="compact"
        icon="mdi-arrow-right-circle-outline"
        color="purple"
        size="large"
      ></v-btn>
    </div>
  </section>
  <div class="container">
    <section class="pa-4 bg-purple-lighten-4" v-if="tickers.length > 0">
      <v-row>
        <v-col cols="4" v-for="tk in filteredTickers" :key="tk.name">
          {{ filter }}
          {{ tickers }}
          {{ filteredTickers.length }}
          <v-card
            class="d-flex flex-column align-center ga-2 py-3"
            @click="setSelectedTicker(tk)"
            :class="{
              'v-card-active': selectedTicker === tk,
            }"
          >
            <div class="d-flex flex-column align-center ga-2 mb-2">
              <h3>{{ tk.name }}</h3>
              <h4 class="font-weight-black price">{{ tk.price }}</h4>
            </div>
            <v-btn
              @click.stop="removeTicker(tk)"
              type="button"
              class="mx-2 remove-custom-btn"
            >
              Удалить
              <v-icon icon="mdi-delete" end></v-icon> </v-btn
          ></v-card>
        </v-col>
      </v-row>
    </section>
    <section class="bg-purple-lighten-3 pa-4" v-if="selectedTicker">
      <v-card class="d-flex pa-2 justify-space-between">
        <h3>{{ selectedTicker.name }}</h3>
        <v-btn
          @click="setSelectedTicker(null)"
          density="compact"
          icon="mdi-close"
          color="purple"
        ></v-btn>
      </v-card>
    </section>
  </div>
</template>

<style scoped>
.container {
  height: 100%;
  display: flex;
  flex-basis: 1;

  & section {
    flex-grow: 1;
  }
}
.custom-chip {
  background-color: #9c27b0;
  color: #fff;
  &:hover {
    background-color: #4a148c;
  }
}
.remove-icon {
}
.price {
  font-size: 2rem;
}
.remove-custom-btn {
  color: #fff;
  background-color: #d81b60;

  &:hover {
    background-color: #880e4f;
  }
}
.error-ticker {
  color: #d81b60;
}
.v-card-active {
  border: 2px solid #4a148c;
}
</style>
