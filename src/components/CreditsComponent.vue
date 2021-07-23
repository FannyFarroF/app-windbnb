<template>
  <div class="credits">
    <div class="container py-4">
      <p class="mx-0 text-center my-3">
        Created by <b>Fanny Farro</b> -
        <a href="" class="">devChallenges.io</a>
      </p>
    </div>
  </div>
</template>

<script>
  export default {
    name: "CreditsComponent",
    created() {},
    data() {
      return {};
    },
    props: {},
    methods: {},
  };
</script>

<style lang="scss" scoped>
  .credits {
    p {
      color: #a09fb1;
      font-size: 14px;
      a {
        color: #a09fb1;
        text-decoration: none;
      }
    }
  }
</style>
