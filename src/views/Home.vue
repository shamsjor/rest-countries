<template>
  <HeadApp @passData="passData($event)" />
  <BodyApp :dark="dark" />
</template>

<script>
// @ is an alias to /src
import HeadApp from "@/components/head.vue";
import BodyApp from "@/components/body.vue";
// import Vue from "vue";

export default {
  name: "Home",
  components: {
    HeadApp,
    BodyApp,
  },
  data() {
    return {
      dark: false,
    };
  },
  methods: {
    passData(drk) {
      this.dark = drk;
    },
  },
};
</script>
