<template>
  <div>
    <h2>Döviz Çevir</h2>

    <div class="inputBox">
      <div class="input">
        <input
          v-model="from"
          type="text"
          class="from"
          name="from"
          id="from"
          aria-label="from"
          maxlength="10"
        />
      </div>
      <div class="selectBox">
        <select class="changeSelect" v-model="selected">
          <option v-for="(item, index) in symbols" :key="index">
            {{ item }}
          </option>
        </select>
      </div>
    </div>

    <div class="iconBox">
      <i></i>
    </div>

    <div class="inputBox">
      <div class="input">
        <input
          v-model="to"
          type="text"
          class="to"
          name="to"
          id="to"
          aria-label="to"
          readonly
        />
        <span>TL</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Exchange",
  data() {
    return {
      symbols: ["USD", "JPY", "DKK", "EUR", "GBP", "NOK"],
      selected: "",
      from: 1,
      to: 0,
    };
  },
  methods: {
    async dataUpdate() {
      if (this.from == "") {
        this.to = 0;
      } else {
        const { data } = await this.$axios.get(
          `convert?from=${this.selected}&to=TRY&amount=${this.from}`
        );
        this.to = data.result
          .toFixed(2)
          .replace(/(\d)(?=(\d{3})+\.)/g, "$1.")
          .replace(/\.(\d+)$/, ",$1");
      }
    },
  },
  mounted() {
    this.dataUpdate();
    this.selected = this.symbols[0];
  },
  watch: {
    selected() {
      this.dataUpdate();
    },
    from() {
      this.dataUpdate();
    },
  },
};
</script>
