<template>
  <div class="page-wrapper">
    <router-link :to="{name: 'home'}" class="back-btn">
      <svg width="19" height="12" viewBox="0 0 19 12" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path fill-rule="evenodd" clip-rule="evenodd" d="M6.46447 0.107445L7.64298 1.28596L3.75389 5.17504L18.6031 5.17504L18.6031 6.82496L3.75389 6.82496L7.64298 10.714L6.46447 11.8926L0.57191 6L6.46447 0.107445Z" fill="#111517"/>
      </svg>
      Back
    </router-link>

    <div class="content-section">
      <div class="img-wrapper">
        <img :src="countryInfo.flag" :alt="countryInfo.name" />
      </div>

      <div class="content-wrapper">
        <h2 v-text="countryInfo.name"></h2>

        <div class="country-info">
          <div class="left-info">
            <p v-html="`<span>Native Name:</span> ${countryInfo.nativeName}`"></p>
            <p v-html="`<span>Population:</span> ${countryInfo.population}`"></p>
            <p v-html="`<span>Region:</span> ${countryInfo.region}`"></p>
            <p v-html="`<span>Sub Region:</span> ${countryInfo.subRegion}`"></p>
            <p v-html="`<span>Capital:</span> ${countryInfo.capital}`"></p>
          </div>

          <div class="right-info">
            <div class="multi-info">
              <p class="info-type">Top Level Domain: </p>
              <p class="info-list" v-for="(domain, index) in countryInfo.domains" v-bind:key="index">
                 {{domain}}
                <span v-if="index != countryInfo.domains.length - 1">, </span>
              </p>
            </div>
            <div class="multi-info">
              <p class="info-type">Currencies: </p>
              <p class="info-list" v-for="(currency, index) in countryInfo.currencies" v-bind:key="index">
                {{currency.name}} ({{currency.code}})
                <span v-if="index != countryInfo.currencies.length - 1">, </span>
              </p>
            </div>
            <div class="multi-info">
              <p class="info-type">Languages: </p>
              <p class="info-list" v-for="(language, index) in countryInfo.languages" v-bind:key="index">
                {{language.name}} ({{ language.nativeName }})
                <span v-if="index != countryInfo.languages.length - 1">, </span>
              </p>
            </div>
          </div>
        </div>

        <div class="country-borders">
          <p class="border-title">Border Countries:</p>

          <router-link class="border-country"
            v-for="(border, index) in borderCountries"
            v-bind:key="index"
            :to="{ name: 'country', params: { code: border.code }}">{{border.name}}</router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import axios from 'axios';

export default {
  data() {
    return {
      countryInfo: {},
      borderCountries: [],
      errors: []
    }
  },
  head() {
    return {
      title: {
        inner: 'Home'
      },
      meta: [
        {
          name: 'description',
          content: `${this.appTitle} home page`,
          id: 'desc'
        }
      ]
    }
  },
  watch: {
    $route() {
      this.loadCountryData()
    }
  },
  created() {
    this.loadCountryData()
  },
  methods: {
    loadCountryData() {
      const alphaCode = window.location.pathname.split('/').pop()

      axios
        .get(`https://restcountries.com/v2/alpha/${alphaCode}?fields=flag,name,population,region,capital,nativeName,subregion,topLevelDomain,currencies,languages,borders`)
        .then(response => {
          this.countryInfo = {
            name: response.data.name,
            flag: response.data.flag,
            population: response.data.population,
            region: response.data.region,
            capital: response.data.capital,
            nativeName: response.data.nativeName,
            subRegion: response.data.subregion,
            domains: response.data.topLevelDomain,
            currencies: response.data.currencies,
            languages: response.data.languages,
            borders: response.data.borders
          }

          this.borderCountries = []

          this.countryInfo.borders.forEach(border => {

            axios
              .get(`https://restcountries.com/v2/alpha/${border}?fields=name`)
              .then(responseBorders => {
                // console.log(response)
                this.borderCountries.push({
                  code: border,
                  name: responseBorders.data.name
                })
              })
              .catch(e => {
                this.errors.push(e)
              })
          })
        })
        .catch(e => {
          this.errors.push(e)
        })
    }
  },
  computed: mapState('app', ['appTitle'])
}
</script>

<style lang="scss" scoped>
@import '@/theme/variables.scss';

.page-wrapper {
  display: flex;
  flex-direction: column;
  padding: 170px 80px 45px;

  @media (max-width: 767px) {
    padding: 120px 24px 45px;
  }

  .back-btn {
    display: flex;
    align-items: center;
    width: fit-content;
    padding: 10px 39px 10px 32px;
    font-weight: 300;
    font-size: 16px;
    line-height: 20px;
    box-shadow: 0px 0px 7px rgba(0, 0, 0, 0.293139);
    border-radius: 2px;

    @media (max-width: 767px) {
      font-size: 14px;
      line-height: 20px;
      padding: 6px 24px;
    }

    svg {
      margin-right: 10px;
      stroke: var(--textColor);
      transition: none;

      path {
        fill: var(--textColor);
        transition: none;
      }
    }
  }

  .content-section {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    margin-top: 80px;

    @media (max-width: 1024px) {
      flex-direction: column;
      margin-top: 64px;
    }

    @media (max-width: 767px) {
      align-items: flex-start;
    }

    .img-wrapper {
      margin-right: 6rem;

      @media (max-width: 1024px) {
        margin-right: 0;
      }

      img {
        border-radius: 5px;
        max-width: 100%;
      }
    }

    .content-wrapper {
      display: flex;
      flex-direction: column;
      max-width: 500px;

      @media (max-width: 1024px) {
        margin-left: 0;
      }

      h2 {
        font-weight: 800;
        font-size: 32px;
        line-height: 44px;

        @media (max-width: 1024px) {
          margin-left: 0;
          margin-top: 44px;
        }

        @media (max-width: 767px) {
          font-size: 22px;
          line-height: 30px;
        }
      }

      .country-info {
        display: flex;
        margin: 23px 0 68px;

        @media (max-width: 767px) {
          flex-direction: column;
          margin: 16px 0 32px;
        }

        .left-info {
          margin-right: 5rem;

          @media (max-width: 767px) {
            margin: 0 0 32px;
          }
        }

        .multi-info {
          display: flex;
          flex-wrap: wrap;
        }

        .info-type,
        .info-list,
        p {
          font-weight: 300;
          font-size: 16px;
          line-height: 32px;

          @media (max-width: 767px) {
            font-size: 14px;
            line-height: 32px;
          }

          >>> span {
            font-weight: 600;
          }
        }

        .info-type {
          margin-right: 5px;
          font-weight: 600;
        }
      }

      .country-borders {
        display: flex;
        flex-wrap: wrap;
        align-items: center;
        margin-bottom: -10px;

        .border-title {
          font-weight: 600;
          font-size: 16px;
          line-height: 24px;
          margin-right: 16px;

          @media (max-width: 767px) {
            flex-basis: 100%;
            margin-bottom: 16px;
          }
        }

        .border-country {
          padding: 5px 27px 4px;
          font-weight: 300;
          font-size: 14px;
          line-height: 19px;
          background: var(--elementsColor);
          box-shadow: 0 0 4px 1px rgba(0, 0, 0, 0.104931);
          border-radius: 2px;
          cursor: pointer;
          margin-bottom: 10px;

          @media (max-width: 767px) {
            font-size: 12px;
            line-height: 16px;
          }

          &:hover {
            opacity: .7;
          }

          &:not(:last-child) {
            margin-right: 10px;
          }
        }
      }
    }
  }
}
</style>
