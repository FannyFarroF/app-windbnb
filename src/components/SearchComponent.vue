<template>
  <div class="search">
    <div
      class="content-search d-flex  align-items-center p-0"
      @click="showModal()"
    >
      <div class="location py-2 px-3 h-100">
        {{ cityFiltroM }}{{ cityFiltroM != "Finland" ? ", Finland" : "" }}
      </div>
      <div class="guest py-2 px-3 h-100">
        Add guest
      </div>
      <div class="button py-2 px-3 h-100">
        <button class="icon" @click="showModal()" title="Close search">
          <span class="material-icons-outlined">
            search
          </span>
        </button>
      </div>
    </div>

    <div class="modal" v-show="show == true">
      <div class="content-modal">
        <div class="container py-5">
          <div class="header-search text-end">
            <button @click="hiddenModal()">
              <span class="material-icons">
                close
              </span>
            </button>
          </div>
          <div class="search-compl">
            <ul class="nav nav-tabs" id="myTab" role="tablist">
              <li class="nav-item" role="presentation">
                <button
                  class="nav-link active"
                  id="home-tab"
                  data-bs-toggle="tab"
                  data-bs-target="#home"
                  type="button"
                  role="tab"
                  aria-controls="home"
                  aria-selected="true"
                >
                  <div class="w-100 text-start title-button">Location</div>
                  <div class="w-100 text-start content-button my-2">
                    {{ locationDefault
                    }}{{ locationDefault != "Finland" ? ", Finland" : "" }}
                  </div>
                </button>
              </li>
              <li class="nav-item" role="presentation">
                <button
                  class="nav-link"
                  id="profile-tab"
                  data-bs-toggle="tab"
                  data-bs-target="#profile"
                  type="button"
                  role="tab"
                  aria-controls="profile"
                  aria-selected="false"
                >
                  <div class="w-100 text-start title-button">Guests</div>
                  <div class="w-100 text-start content-button my-2">
                    {{ totalGuests }} guests
                  </div>
                </button>
              </li>
              <li
                class="nav-item d-flex justify-content-center align-items-center"
                role="presentation"
              >
                <button
                  class="nav-link btn-buscar d-flex justify-content-center align-items-center"
                  id="contact-tab"
                  type="button"
                  @click="buscar()"
                >
                  <span class="material-icons-outlined">
                    search
                  </span>
                  Search
                </button>
              </li>
            </ul>
            <div class="tab-content" id="myTabContent">
              <div
                class="tab-pane fade show active p-3"
                id="home"
                role="tabpanel"
                aria-labelledby="home-tab"
              >
                <ul>
                  <li
                    v-for="(item, index) in listCitys"
                    :key="index"
                    class="my-2"
                    @click="actualizarLocation(item)"
                  >
                    <div class="wrapper d-flex">
                      <div class="icon">
                        <span class="material-icons-outlined">
                          location_on
                        </span>
                      </div>
                      <p class="mx-2 my-0">{{ item }},</p>
                      Finland
                    </div>
                  </li>
                </ul>
              </div>
              <div
                class="tab-pane fade p-3"
                id="profile"
                role="tabpanel"
                aria-labelledby="profile-tab"
              >
                <NumberGuests
                  v-bind:title="numberComponentO.title"
                  v-bind:subtitle="numberComponentO.subtitle"
                  v-on:value="adults($event)"
                  v-bind:limpiar="limpiar"
                />
                <NumberGuests
                  v-bind:title="numberComponentT.title"
                  v-bind:subtitle="numberComponentT.subtitle"
                  v-on:value="children($event)"
                  v-bind:limpiar="limpiar"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  // import axios from "axios";
  import NumberGuests from "@/components/NumberGuestsComponent.vue";

  export default {
    name: "SearchComponent",
    created() {},
    props: ["citys", "cityFiltroM"],
    components: {
      NumberGuests,
    },
    data() {
      return {
        guest: 0,
        locationDefault: "Finland",
        show: false,
        listCitys: this.citys,
        numberComponentO: {
          title: "Adults",
          subtitle: "Ages 13 or above ",
        },
        numberComponentT: {
          title: "Children",
          subtitle: "Ages 2-12 ",
        },
        numAldults: 0,
        numChildre: 0,
        limpiar: false,
      };
    },

    mounted() {},
    watch: {},
    methods: {
      showModal() {
        this.show = true;
      },
      hiddenModal() {
        this.show = false;
        this.limpiar = true;
      },
      actualizarLocation(value) {
        this.locationDefault = value;
      },
      adults(value) {
        this.numAldults = value;
      },
      children(value) {
        this.numChildre = value;
      },
      buscar() {
        let datosFriltrar = {
          city: this.locationDefault,
          guests: this.totalGuests,
        };
        this.$emit("filtrar", datosFriltrar);

        // Limpiar campos
        this.show = false;
        this.cityFiltroMs = this.locationDefault;
        this.locationDefault = "Finland";
        this.numAldults = 0;
        this.numChildre = 0;
        // Limpiar componente NumberGuestComponent
        this.limpiar = true;
      },
    },
    computed: {
      totalGuests: function() {
        return this.numAldults + this.numChildre;
      },
    },
  };
</script>

<style lang="scss" scoped>
  ul {
    li {
      list-style: none;
      cursor: pointer;
    }
  }
  .header-search {
    button {
      color: #333;
      border: 0;
      background: none;
      margin-bottom: 15px;
    }
  }
  .search {
    .content-search {
      background: #ffffff;
      box-shadow: 0px 1px 6px rgba(0, 0, 0, 0.1);
      border-radius: 16px;
      .location {
        // font-family: Mulish;
        font-style: normal;
        font-weight: normal;
        font-size: 14px;
        line-height: 18px;
        color: #333333;
      }

      .guest {
        border-left: 1px solid #f2f2f2;
        border-right: 1px solid #f2f2f2;
        // font-family: Mulish;
        font-style: normal;
        font-weight: normal;
        font-size: 14px;
        line-height: 18px;
        /* identical to box height */

        color: #bdbdbd;
        input {
          width: 100px;
          border: 0;
        }
        input:focus {
          outline: 0;
        }
      }
      button {
        border: 0;
        background: none;
        span {
          color: #eb5757;
        }
      }
    }
  }
  .modal {
    transition: 0.5s all;
    display: inherit;
    background: rgba(79, 79, 79, 0.4);
    .content-modal {
      background: #fff;
      .nav-tabs {
        background: #ffffff;
        box-shadow: 0px 1px 6px rgba(0, 0, 0, 0.1);
        border-radius: 16px;
        li {
          width: 33.33%;
          button {
            width: 100%;
            height: 100%;
            &.btn-buscar {
              background: rgba(235, 87, 87, 0.9);
              box-shadow: 0px 1px 6px rgba(0, 0, 0, 0.1);
              border-radius: 16px;
              color: #fff;
              margin: auto;
              height: 50px !important;
              width: 150px;
            }
          }

          .title-button {
            font-style: normal;
            font-weight: 800;
            font-size: 9px;
            line-height: 11px;
            text-transform: uppercase;

            color: #333333;
          }
          .locationDefault {
            font-style: normal;
            font-weight: normal;
            font-size: 14px;
            line-height: 18px;
            /* identical to box height */

            color: #333333;
          }
        }
      }
      .nav-tabs .nav-item.show .nav-link,
      .nav-tabs .nav-link.active {
        background: none !important;
        border: 1px solid #333333;
        box-sizing: border-box;
        filter: drop-shadow(0px 1px 6px rgba(0, 0, 0, 0.1));
        border-radius: 16px;
      }
      .tab-content {
        .icon {
          span {
            color: #4f4f4f;
          }
        }
      }
    }
  }
  @media only screen and (max-width: 540px) {
    .search-compl {
      position: relative;
    }
    .guests {
      width: 100% !important;
    }
    .nav-tabs {
      li {
        width: 100% !important;
      }
      li:last-child {
        position: absolute;
        bottom: -25px;
        left: 0;
      }
    }
  }
</style>
