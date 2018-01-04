<template>
  <div class="content">
    <div class="question">If
      <v-select :options="coins" label="name" :on-change="selectFromCoin" :value.sync="fromCoin"></v-select>
      reaches the market cap of
      <v-select :options="coins" label="name" :on-change="selectToCoin" :value.sync="toCoin"></v-select>
    </div>

    <div class="answer" v-if="fromCoin">
      The value of <strong>1 {{ fromCoin.symbol }}</strong> will become
      <strong>{{ (parseFloat(toCoin['market_cap_' + activeCurrency]) / parseFloat(fromCoin.available_supply)) | currency(activeCurrency)  }}</strong>
      <span v-if="parseFloat(fromCoin.max_supply) > (parseFloat(fromCoin.available_supply) + 10)">
        with the currently available supply<br>
        and <strong>{{ (parseFloat(toCoin['market_cap_' + activeCurrency]) / parseFloat(fromCoin.max_supply)) | currency(activeCurrency)  }}</strong>
        when reaching the maximum supply of <strong>{{ fromCoin.max_supply | number }}</strong>
      </span>
    </div>
  </div>
</template>

<script>
  import vSelect from 'vue-select'

  export default {
    components: { vSelect },
    name: 'HelloWorld',
    data () {
      return {
        activeCurrency: 'eur',
        fromCoin: null,
        toCoin: null,
        coins: []
      }
    },
    created () {
      this.$http.get('https://api.coinmarketcap.com/v1/ticker/?limit=100&convert=EUR').then(resp => {
        this.coins = resp.data
        this.fromCoin = this.findCoin('XRB')
        console.log(this.fromCoin)
        this.toCoin = this.findCoin('BTC')
        console.log(this.toCoin)
      })
    },
    methods: {
      findCoin (symbol) {
        return this.coins.find((element) => {
          return element.symbol === symbol
        })
      },
      selectFromCoin (val) {
        console.log('From')
        console.log(val)
        this.fromCoin = val
      },
      selectToCoin (val) {
        console.log('To')
        console.log(val)
        this.toCoin = val
      }
    },
    filters: {
      currency (value, currency) {
        return value.toLocaleString(undefined, { style: 'currency', currency: currency })
      },
      number (value) {
        return parseFloat(value).toLocaleString()
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
  .question {
    font-size: 30px;
    font-weight: normal;
  }

  .answer {
    font-size: 30px;
    margin-top: 30px;
    font-weight: normal;
  }

  /*span {*/
    /*font-weight: bold;*/
  /*}*/

  .v-select {
    border: none;
    border-bottom: dashed;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    display: inline-block;

    .dropdown-toggle {
      border: none;
      display: inline-block;

      input[type=search], input[type=search]:focus {
        border: none;
        max-width: 200px;
        float: none;
      }
    }


    &.single {
      .selected-tag {
        position: absolute;
        line-height: normal;
        font-weight: bold;
        width: 100%;
        float: none;
        margin: inherit;
        color: inherit;
        background-color: inherit;
        border: none;
        border-radius: 0;
        height: auto;
        padding: inherit 10px inherit inherit;
      }
    }
  }
</style>
