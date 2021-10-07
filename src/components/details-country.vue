<template>
  <div class="details py-5" :class="{ dark: dark }">
    <div class="container">
      <router-link to="/">
        <button class="btn btn-light shadow my-5" :class="{ dark: dark }">
          <i class="fas fa-arrow-left"></i>
          Back
        </button>
      </router-link>
      <div
        class="d-flex justify-content-between flex-wrap flex-column flex-md-row"
      >
        <img :src="countryObject.flag" alt="" class="w-50" />
        <div class="column1 my-2">
          <h1 class="my-4">{{ countryObject.name }}</h1>
          <p>
            Native Name: <span>{{ countryObject.nativeName }}</span>
          </p>
          <p>
            Population: <span>{{ countryObject.population }}</span>
          </p>
          <p>
            Region: <span>{{ countryObject.region }}</span>
          </p>
          <p>
            Subregion: <span>{{ countryObject.subregion }}</span>
          </p>
          <p>
            <!-- Capital: <span>{{ countryObject.capital }}</span> -->
          </p>
        </div>
        <div class="column2">
          <p>
            <!-- Top Level domain: <span>{{ countryObject.domain[0] }}</span> -->
          </p>
          <p>
            Currencies: <span>{{ countryObject.currencies }}</span>
          </p>
          <p>
            Languages: <span>{{ countryObject.languages }}</span>
          </p>
        </div>
      </div>
      <p class="borders">
        Border Countries:
        <span
          class="shadow mx-2"
          v-for="(border, index) in countryObject.border"
          :key="index"
          >{{ border }}
        </span>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Details",
  props: ["dark"],
  data() {
    return {
      countryObject: {},
    };
  },
  mounted() {
    let request = new XMLHttpRequest();
    request.open(
      `GET`,
      `https://restcountries.com/v2/name/${this.$route.params.id}`,
      true
    );
    request.onreadystatechange = () => {
      if (request.readyState === 4 && request.status === 200) {
        let obj = JSON.parse(request.responseText);
        this.countryObject.flag = obj[0].flags.svg;
        this.countryObject.name = obj[0].name;
        this.countryObject.nativeName = obj[0].nativeName;
        this.countryObject.population = obj[0].population;
        this.countryObject.region = obj[0].region;
        this.countryObject.subregion = obj[0].subregion;
        this.countryObject.capital = obj[0].capital;
        this.countryObject.domain = obj[0].topLevelDomain;
        this.countryObject.currencies = obj[0].currencies[0].name;
        this.countryObject.languages = obj[0].languages[0].name;
        this.countryObject.border = obj[0].borders;
      }
    };
    request.send();
  },
};
</script>
<style scoped>
.details.dark {
  background-color: hsl(207, 26%, 17%);
  color: white;
}
button.dark {
  background-color: hsl(209, 23%, 22%);
  color: white;
  border: none;
}
.column1,
.column2 {
  flex-basis: 20%;
}
.column2 {
  align-self: center;
}
@media (max-width: 768px) {
  .column2 {
    align-self: flex-start;
  }
}
span {
  color: hsl(0, 0%, 52%);
}
.borders {
  margin-left: 55%;
}
@media (max-width: 768px) {
  .borders {
    margin-left: 0%;
  }
}
@media (max-width: 768px) {
  img {
    width: 100% !important;
  }
}
</style>
