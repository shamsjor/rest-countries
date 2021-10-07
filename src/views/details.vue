<template>
  <div class="details">
    <HeadApp @passData="passData($event)" />
    <detailsApp :dark="dark" />
  </div>
</template>
<script>
import detailsApp from "@/components/details-country.vue";
import HeadApp from "@/components/head.vue";

export default {
  name: "Details",
  components: {
    detailsApp,
    HeadApp,
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
