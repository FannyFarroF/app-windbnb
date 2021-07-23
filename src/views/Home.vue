<template>
  <div class="home">
    <header>
      <div class="container">
        <div
          class="d-flex justify-content-between align-items-center py-4 content-header"
        >
          <figure class="m-0">
            <img src="../../public/logo.svg" alt="Logo" srcset="" />
          </figure>
          <SearchComponent
            v-bind:citys="citys"
            v-bind:cityFiltroM="cityFiltro == '' ? 'Finland' : cityFiltro"
            v-on:filtrar="dateFiltro($event)"
          />
        </div>
      </div>
    </header>
    <main>
      <section class="stays py-5">
        <div class="container">
          <div class=" d-flex justify-content-between align-items-center">
            <h2 class="text-start">Stays in Finland</h2>
            <p class="m-0 length-stays">{{ dataCF.length }}+ stays</p>
          </div>
          <ul class="list-stays p-0 row">
            <li
              class="item-list my-4"
              v-for="(item, index) in dataCF"
              :key="index"
            >
              <div class="wrapper">
                <div class="header-item">
                  <figure class="m-0">
                    <img :src="item.photo" alt="" class="w-100 h-100" />
                  </figure>
                </div>
                <div
                  class="body-item py-3 d-flex justify-content-between align-items-center"
                >
                  <div
                    class="superHost-item py-1 px-2"
                    v-show="item.superHost === true"
                  >
                    <p class="m-0">superHost</p>
                  </div>
                  <div class="type-item">
                    <p class="m-0">
                      {{ item.type }}.
                      <span class="m-0" v-show="item.beds != null">
                        {{ item.beds }} beds
                      </span>
                    </p>
                  </div>
                  <div class="rating-item d-flex align-items-center">
                    <div class="icon">
                      <span class="material-icons-outlined">
                        star
                      </span>
                    </div>
                    <p class="m-0">{{ item.rating }}</p>
                  </div>
                </div>
                <div class="footer-item">
                  <div class="title-item">
                    <p class="m-0 text-start">
                      {{ item.title.substring(0, 30) }} ...
                    </p>
                  </div>
                </div>
              </div>
            </li>
          </ul>
        </div>
      </section>
    </main>
    <footer>
      <CreditsComponent />
    </footer>
  </div>
</template>

<script>
  import axios from "axios";

  // @ is an alias to /src
  import SearchComponent from "@/components/SearchComponent.vue";
  import CreditsComponent from "@/components/CreditsComponent.vue";

  export default {
    name: "Home",
    components: {
      SearchComponent,
      CreditsComponent,
    },
    data() {
      return {
        data: [],
        dataCF: [],
        citys: [],
        cityFiltro: "Finland",
        guetsFiltro: "",
      };
    },
    mounted() {
      this.cargarDatos();
    },
    methods: {
      cargarDatos() {
        axios
          .get("/stays.json")
          .then((response) => {
            let data = response.data;
            this.data = data;
            this.dataCF = data;
            this.ciudades();
          })
          .catch(function(error) {
            console.error(error);
          });
      },
      ciudades() {
        this.data.forEach((item1) => {
          const existe = this.citys.some((item) => item === item1.city);
          if (!existe) {
            this.citys.push(item1.city);
          }
        });
        return this.citys;
      },
      dateFiltro(value) {
        this.cityFiltro = value.city;
        this.guetsFiltro = value.guests;

        this.dataCF = this.data.filter(
          (item) =>
            item.city == this.cityFiltro && item.maxGuests >= this.guetsFiltro
        );
      },
    },
    computed: {},
  };
</script>

<style lang="scss">
  h2 {
    font-family: Montserrat;
    font-style: normal;
    font-weight: bold;
    font-size: 24px;
    line-height: 29px;

    color: #333333;
  }
  .length-stays {
    font-family: Montserrat;
    font-style: normal;
    font-weight: 500;
    font-size: 14px;
    line-height: 17px;
    color: #4f4f4f;
  }
  .list-stays {
    .item-list {
      width: 33.33%;
      list-style: none;
      figure {
        border-radius: 24px;
        height: 269px;
        overflow: hidden;
        img {
          background-size: cover;
        }
      }
      .type-item {
        font-family: Montserrat;
        font-style: normal;
        font-weight: 500;
        font-size: 14px;
        line-height: 17px;
        color: #828282;
      }
      .superHost-item {
        border: 1px solid #4f4f4f;
        box-sizing: border-box;
        border-radius: 12px;
      }
      .icon {
        span {
          color: #eb5757;
        }
      }
      .rating-item {
        color: #4f4f4f;
        font-family: Montserrat;
        font-style: normal;
        font-weight: 500;
        font-size: 14px;
        line-height: 17px;
      }
      .title-item {
        font-family: Montserrat;
        font-style: normal;
        font-weight: 600;
        font-size: 16px;
        line-height: 20px;
        /* identical to box height */

        text-align: right;

        color: #333333;
      }
    }
  }

  @media only screen and (max-width: 768px) {
    .list-stays .item-list {
      width: 50%;
    }
  }
  @media only screen and (max-width: 540px) {
    .list-stays .item-list {
      width: 100%;
    }
    .content-header {
      flex-direction: column;
      figure {
        width: 100%;
        text-align: start;
        margin-bottom: 30px !important;
      }
    }
    .stays {
      padding-top: 20px !important;
    }
  }
</style>
