<template>
  <ul>
    <li v-for="(item, index) in symbols" :key="index">
      <div class="imageBox">
        <img
          :src="`/img/${item.image}`"
          width="auto"
          height="auto"
          :alt="item.code"
        />
      </div>
      <div class="textBox">
        <div class="column">
          <div class="title">{{ item.code }}</div>
          <div class="desc">{{ item.moneyName }}</div>
        </div>
        <div class="column">
          <div class="minTitle">ALIŞ</div>
          <div class="number">{{ item.rates }}</div>
        </div>
        <div class="column">
          <div class="minTitle">SATIŞ</div>
          <div class="number">{{ item.rates }}</div>
        </div>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  name: "ExchangeRate",
  data() {
    return {
      symbols: [
        {
          code: "USD",
          moneyName: "Amerikan Doları",
          image: "usd-flag.png",
        },
        {
          code: "JPY",
          moneyName: "Japon Yeni",
          image: "jpy-flag.png",
        },
        {
          code: "DKK",
          moneyName: "Danimarka Kronu",
          image: "dkk-flag.png",
        },
        {
          code: "EUR",
          moneyName: "Avrupa Para Birimi",
          image: "eur-flag.png",
        },
        {
          code: "GBP",
          moneyName: "İngiliz Sterlini",
          image: "gbp-flag.png",
        },
        {
          code: "NOK",
          moneyName: "Norveç Kronu",
          image: "nok-flag.png",
        },
      ],
    };
  },
  mounted() {
    let _this = this;
    this.symbols.forEach(async function (item, index) {
      const { data } = await _this.$axios.get(
        `latest?base=${item.code}&symbols=TRY`
      );
      item["rates"] = data.rates.TRY.toFixed(4).replace(/(\d)(?=(\d{3})+\.)/g, "$1.").replace(/\.(\d+)$/, ",$1");
      _this.$set(_this.symbols, index, item);
    });
  },
};
</script>