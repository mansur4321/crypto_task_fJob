<template>
  <div class="wrapper">
    <header class="header">
      <nav class="header__nav">
        <a class="header__nav-item _active-nav">Converter</a>
        <a class="header__nav-item">Crypto portfolio</a>
      </nav>
    </header>

    <main class="main">
      <div class="converter-page">
        <div class="converter-page__item">
          <div class="convert-currency">
            <input type="number" class="convert-currency__input" />

            <select class="convert-currency__select">
              <option value="BTC">BTC</option>

              <option value="ETH">ETH</option>

              <option value="USD">USD</option>
            </select>
          </div>

          <div class="convert-currency">
            <input type="number" class="convert-currency__input" />

            <select class="convert-currency__select">
              <option value="BTC">BTC</option>
              <option value="ETH">ETH</option>
              <option value="USD">USD</option>
            </select>
          </div>
        </div>

        <div class="convert-currency__item"></div>
      </div>

      <div class="portfolio-page">
        <h2>
          <span class="evaluation-title"> Портфель оценивается в </span>
        </h2>
        <div class="cicle">
          <div class="cicle__big"></div>
          <div class="cicle__min"></div>
        </div>
        <div class="porfolio-main-content">
          <div class="currency-case">
            <div class="currency">
              <p class="currency__text">BTC - 1000</p>
            </div>

            <div class="change-currency">
              <div class="change-currency__header-wrapper">
                <p class="change-currency__text">
                  Внести изменения в крипто-кошельке
                </p>
                <div class="change-currency__btn">
                  <img src="./assets/icon-arow.png" class="arow" />
                </div>
              </div>

              <div class="change-currency__main-wrapper">
                Нажмите +/- в зависимости от операции, которую хотите произвести

                <div class="currency-operator">
                  <div class="currency-operator__input-wrapper">
                    <div class="currency-operator__plus-minus">
                      <div class="currency-operator__plus">+</div>
                      <div class="currency-operator__minus">-</div>
                    </div>

                    <input type="number" class="currency-operator__input" />

                    <a class="currency-operator__btn"><span>Выполнить</span></a>
                  </div>
                  <select class="currency-operator__select">
                    <option value="BTC">BTC</option>
                    <option value="ETH">ETH</option>
                    <option value="USD">USD</option>
                  </select>
                </div>
              </div>
            </div>
          </div>
          <div class="currency-graph"></div>
        </div>
      </div>
    </main>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";

@Options({
  components: {},
})
export default class App extends Vue {}
</script>

<style lang="scss" src="./LESS/index.scss"></style>
