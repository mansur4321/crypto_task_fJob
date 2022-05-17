<template>
  <div class="wrapper">
    <img src="./assets/BTC.png" class="png-img">
    <img src="./assets/ETH.png" class="png-img">
    <img src="./assets/USD.png" class="png-img">
    <header class="header">
      <nav class="header__nav">
        <a class="header__nav-item"
          :class="{'_active-nav': convertPage}"
          @click="pageSwitching(true)"
        >Converter</a>
        <a class="header__nav-item"
          :class="{'_active-nav': portfolioPage}"
          @click="pageSwitching(false)"
        >Crypto portfolio</a>
      </nav>
    </header>

    <main class="main">
      <div class="converter-page"
        v-if="convertPage"
      >
        <h2>
          <span class="converter-page__title">Converter currencies</span>
        </h2>
        <div class="converter-page__content">
          <div class="converter-page__item">
            <div class="convert-currency">
              <input type="number" class="convert-currency__input"
                v-model="firstCurrency"
              />

              <select
                class="convert-currency__select"
                v-model="fieldsForConversion[0]"
                @change="convertedCurrencies()"
              >
                <option value="BTC">BTC</option>
                <option value="ETH">ETH</option>
                <option value="USD">USD</option>
              </select>
            </div>

            <div class="convert-currency">
              <input type="number" class="convert-currency__input"
                v-model="secondCurrency"
              />

              <select
                class="convert-currency__select"
                v-model="fieldsForConversion[1]"
                @change="convertedCurrencies()"
              >
                <option value="BTC">BTC</option>
                <option value="ETH">ETH</option>
                <option value="USD">USD</option>
              </select>
            </div>
          </div>

          <div class="convert-currency__item"
            v-if="chartBool"
          >
            <line-chart 
              :currenciesData="BTCDateData"
              currencyName="Bitcoin"
              colorLine="#0d4175"
            ></line-chart>
          </div>

          <div class="convert-currency__item"
            v-if="chartBool"
          >
            <line-chart
              :currenciesData="ETHDateData"
              currencyName="Ethereum" 
              colorLine="#9f2441"
            ></line-chart>
          </div>
        </div>
      </div>

      <div class="portfolio-page"
        v-if="portfolioPage"
      >
        <h2>
         <span class="portfolio-page__title">
           Blockfolio
         </span>
        </h2>

        <div class="porfolio-main-content">
          <div class="currency-case">
             <span class="evaluation-title"> Portfolio valued: {{totalPackageSum}}$</span>
            <div class="currency"
              v-for="(currency, index) of currencies"
              :key="currency"
            >
              <p class="currency__text">
                <img :src="currency.srcImage" class="currency__image">
                {{ currency.name }}: {{ currency.price }}
              </p>

              <div class="change-currency">
                <div class="change-currency__header-wrapper"
                  @click="openMathBlock(index)"
                >
                  <p class="change-currency__text">
                    Внести изменения
                  </p>
                  <div class="change-currency__btn"
                    :class="{
                      '_btn-dis-rotate': MathBlockFlag[index],
                    }"
                  >
                    <img src="./assets/icon-arow.png" class="arow" />
                  </div>
                </div>

                <div class="change-currency__main-wrapper"
                  :class="{
                    '_open-block-math': MathBlockFlag[index],
                  }"
                >
                  <div class="currency-operator">
                    <div class="currency-operator__plus-minus">
                        <div class="currency-operator__plus"
                          :class="{
                            '_chosen-sign': signMathBool,
                          }"  
                          @click="changeMathOperator('+')"
                        >Buy</div>
                        <div class="currency-operator__minus"
                          :class="{
                            '_chosen-sign': !signMathBool,
                          }" 
                          @click="changeMathOperator('-')"
                        >Sale</div>
                      </div>
                    <div class="currency-operator__input-wrapper">
                      <input type="number" class="currency-operator__input"
                        @keyup.enter="changeCurrencyValue(currency.name, index)"
                        v-model="secondMathOperator[index]"
                      />
                    </div>
                    <a class="currency-operator__btn"
                        @click="changeCurrencyValue(currency.name, index)"
                      ><span>Выполнить</span></a>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="currency-graph">
            <pie-chart
              :key="indexRenderDiogram"
              :percent="currenciesPercent"
            ></pie-chart>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script lang="js">
import lineChart from './components/chart_graph.vue';
import pieChart from './components/chart_Diogram.vue';

export default {
  components: { lineChart, pieChart },
  data() {
    return {
      _API_URL: 'https://api.coingecko.com/api/v3/',

      convertPage: true,
      portfolioPage: false,
      
      currencyDataList: null,

      BTCDateData: [],
      ETHDateData: [],
      chartBool: false,

      currencies: [
        {
          name: "BTC",
          price: "1",
          srcImage: "",
        },
        {
          name: "ETH",
          price: "2",
          srcImage: "",
        },
        {
          name: "USD",
          price: "3000",
          srcImage: "",
        },
      ],
      totalPackageSum: null,

      currenciesPercent: [
        "0",
        "0",
        "0"
      ],
      indexRenderDiogram: 0,

      fieldsForConversion: ["BTC", "USD"],
      firstCurrency: "1",
      secondCurrency: "0",

      MathBlockFlag: [
        false,
        false,
        false,
      ],
      secondMathOperator: [
        '0',
        '0',
        '0',
      ],
      signMathOperator: "+",
      signMathBool: true,
    }

  },

  async mounted() {
    let images = document.querySelectorAll('.png-img');

    images.forEach((img, index) => {
      this.currencies[index].srcImage = img.getAttribute('src');
    });

    await this.getListCoin();

    this.calcPackageSum();
    this.updateChart();
    this.convertedCurrencies();

    this.getLineData();

    //setTimeout(() => {
    //  //this.chartBool = true;
    //}, 10);
    
    setInterval(() => {
      this.getListCoin();
    }, 60000);
  },

  watch: {
    firstCurrency() {
      this.convertedCurrencies();
    },

    secondCurrency() {
      this.convertedCurrencies();
    },

    totalPackageSum() {
      this.updateChart();
    },
  },

  methods: {
    async getRequest(str) {
      let response = await fetch(str);

      if (response.ok) {
        let message = await response.json();
        return message;
      } else {
        alert('Ошибка запроса на сервер');
      }
    },

    getLastDayOfMonth(year, month, day) {
      let date = new Date(year, month + 1, day);
      return date.getDate();
    },

    updateChart() {
      let percent1 = this.totalPackageSum / 100;

      this.currencies.forEach((curr, i) => {
        if (curr.name === 'USD') {
          this.currenciesPercent[i] = +curr.price / percent1;
        }else {
          this.currenciesPercent[i] = (+curr.price * this.currencyDataList.find(currData => currData.symbol == curr.name.toLowerCase()).market_data.current_price.usd) / percent1;
        }
      });


      this.indexRenderDiogram += 1;
    },

    async getLineData() {
      let dateObj = new Date();


      for (let i = 0; i < 14; i++) {
        let year = dateObj.getFullYear();
        let day;
        let month;


        if (i >= +dateObj.getDate()) {
          month = dateObj.getMonth() - 1;
          day = this.getLastDayOfMonth(year, month, +dateObj.getDate() - i)
        }else {
          day = +dateObj.getDate() - i;
          month = dateObj.getMonth();
        }
        

        let date = `${day}-${month}-${year}`;

        let messageB = await this.getRequest(`${this._API_URL}coins/bitcoin/history?date=${date}`);
        let messageE = await this.getRequest(`${this._API_URL}coins/ethereum/history?date=${date}`);

        let dateCurrencyValueB = {
          day: date,
          value: this.getCurentPrice(messageB.market_data.current_price.usd),
        }

        let dateCurrencyValueE = {
          day: date,
          value: this.getCurentPrice(messageE.market_data.current_price.usd), 
        }

        this.BTCDateData.push(dateCurrencyValueB);
        this.ETHDateData.push(dateCurrencyValueE);
      }
       this.chartBool = true;
    },

    getCurentPrice(coinPrice) {
      if( coinPrice > 10000) {
        coinPrice = coinPrice - 11000;
      }else if(coinPrice < 1000) {
        coinPrice = coinPrice - 2000;
      }

      return coinPrice
    },

    async getListCoin() {
      let message = await this.getRequest(`${this._API_URL}coins.list`);
      this.currencyDataList = message;
    },

    convertedCurrencies() {
      if (this.fieldsForConversion[0] === this.fieldsForConversion[1]){
        this.secondCurrency = this.firstCurrency;
        return;
      }


      let worthCoin = this.getCoinValue();

      if (this.fieldsForConversion[0] == 'USD') {
        this.secondCurrency = this.firstCurrency / worthCoin;
        return;
      }
      

      this.secondCurrency = this.firstCurrency * worthCoin;
    },

    getCoinValue() {
      let firstCurr = this.fieldsForConversion[0].toLowerCase();
      let secondCurr = this.fieldsForConversion[1].toLowerCase();
      let dataCoin;

      if (firstCurr != 'usd') {
        dataCoin = this.currencyDataList.find(currData => currData.symbol == firstCurr).market_data.current_price[`${secondCurr}`];
      }else {
        dataCoin = this.currencyDataList.find(currData => currData.symbol == secondCurr).market_data.current_price[`${firstCurr}`];
      }


      return dataCoin;
    },

    pageSwitching(boolId) {
      this.convertPage = boolId;
      this.portfolioPage = !boolId;
    },

    calcPackageSum() {
      let sum = 0

      this.currencies.forEach(curr => {
        if(curr.name == 'USD') {
          sum += +curr.price;
        } else {
          sum += +curr.price * this.currencyDataList.find(currData => currData.symbol == curr.name.toLowerCase()).market_data.current_price.usd;
        }
      });

      this.totalPackageSum = sum;
    },

    changeMathOperator(operator) {
      this.signMathOperator = operator;
      if (operator === '+') {
        this.signMathBool = true;
      } else {
        this.signMathBool = false;
      }
    },

    changeCurrencyValue(firstMathOperator, i) {
      let value;
      value = +this.currencies.find(curr => curr.name === firstMathOperator).price + +`${this.signMathOperator}${this.secondMathOperator[i]}`;

      if (value < 0) {
        value = 0;
      }


      this.currencies.find(curr => curr.name === firstMathOperator).price = value;
      this.secondMathOperator[i] = '0';

      this.calcPackageSum();
      this.openMathBlock(i);
    },

    openMathBlock(i) {
      this.MathBlockFlag[i] = !this.MathBlockFlag[i];
    }
  }
};
</script>

<style lang="scss" src="./LESS/index.scss"></style>
