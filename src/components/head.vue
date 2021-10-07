<template>
  <div class="head shadow" :class="{ dark: isDark }">
    <div class="container d-flex justify-content-between p-3">
      <h1>Where in the world</h1>
      <div @click="change">
        <i class="fas fa-moon mx-2" v-if="!isDark"></i>
        <i class="far fa-moon mx-2" v-if="isDark"></i>
        <span>{{ mode }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HeadApp",
  data() {
    return {
      mode: `light Mode`,
      isDark: false,
    };
  },
  provide() {
    return { isDark: this.isDark };
  },
  methods: {
    change() {
      this.isDark = !this.isDark;
      this.$emit(`passData`, this.isDark);
      if (!this.isDark) {
        this.mode = `Dark Mode`;
      } else {
        this.mode = `Light Mode`;
      }
    },
  },
};
</script>

<style scoped>
.dark {
  background-color: hsl(209, 23%, 22%);
  color: white !important;
}
.head {
  color: hsl(200, 15%, 8%);
}
</style>
