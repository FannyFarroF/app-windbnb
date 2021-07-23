<template>
  <div class="guests my-4">
    <div class="wrapper">
      <h2 class="text-start">{{ titleG }}</h2>
      <div class="body-wrapper text-start ">
        <p class="m-0">
          {{ subtitleG }}
        </p>
        <div class="number my-2 d-flex align-items-center">
          <button @click="disminuir()">
            -
          </button>
          <span class="mx-4 fw-bold">{{ value }}</span>
          <button @click="aumentar()">
            +
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "NumberGuestsComponent",
    created() {},
    props: ["title", "subtitle", "limpiar"],
    data() {
      return {
        subtitleG: this.subtitle,
        titleG: this.title,
        value: 0,
      };
    },
    watch: {
      limpiar: function(value) {
        if (value == true) {
          this.value = 0;
        }
      },
    },
    methods: {
      disminuir() {
        if (this.value > 0) {
          this.value -= 1;
          this.$emit("value", this.value, "0");
        }
      },
      aumentar() {
        this.value += 1;
        this.$emit("value", this.value, "0");
      },
    },
  };
</script>

<style lang="scss" scoped>
  .guests {
    width: 33.33%;
    margin-left: auto;
    margin-right: auto;
    h2 {
      color: #333;
      font-size: 14px;
    }
    .number {
      button {
        width: 25px;
        border: 1px solid #828282;
        box-sizing: border-box;
        border-radius: 4px;
        color: #828282;
        background: transparent;
      }
    }
  }
</style>
