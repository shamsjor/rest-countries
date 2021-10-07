<template>
  <section class="p-5" :class="{ dark: dark }">
    <div class="container">
      <div class="d-flex justify-content-between position-relative">
        <i class="fas fa-search"></i>
        <input
          type="search"
          name="search"
          class="form-control w-50"
          placeholder="Search for a country..."
          v-model="countryName"
          @input="searchFilter"
          :class="{ dark: dark }"
        />
        <select
          name="filter"
          id="select"
          v-model="filterInput"
          :class="{ dark: dark }"
        >
          <option value="">Filter by Region</option>
          <option value="Africa">Africa</option>
          <option value="Americas">America</option>
          <option value="Asia">Asia</option>
          <option value="Europe">Europe</option>
          <option value="Oceania">Oceania</option>
        </select>
      </div>
      <div class="body" id="repo">
        <div class="d-flex justify-content-between flex-wrap py-5">
          <router-link
            v-for="country in filter"
            :key="country.code"
            :to="{
              name: 'Details',
              params: { id: country.name },
            }"
            ><div class="card py-3" :class="{ dark: dark }">
              <img :src="country.flag" alt="" />
              <div class="card-body">
                <h3 class="card-title fw-bold">{{ country.name }}</h3>
                <div class="card-text">
                  <p>
                    Population: <span>{{ country.population }}</span>
                  </p>
                  <p>
                    Region: <span>{{ country.region }}</span>
                  </p>
                  <p>
                    Capital: <span>{{ country.capital }}</span>
                  </p>
                </div>
              </div>
            </div>
          </router-link>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "BodyApp",
  props: ["dark"],
  data() {
    return {
      countriesObject: [],
      filterInput: ``,
      countryName: ``,
    };
  },
  mounted() {
    let request = new XMLHttpRequest();
    request.open(`GET`, `https://restcountries.com/v3.1/all`, true);
    request.onreadystatechange = () => {
      if (request.readyState === 4 && request.status === 200) {
        let obj = JSON.parse(request.responseText);
        obj.forEach((element) => {
          let country = {
            name: ``,
            flag: ``,
            population: ``,
            region: ``,
            capital: ``,
            code: ``,
          };
          country.name = element.name.common;
          country.flag = element.flags.svg;
          country.population = element.population;
          country.region = element.region;
          if (element.capital !== undefined) {
            country.capital = element.capital.join(``);
          }
          country.code = element.alpha3Code;
          this.countriesObject.push(country);
        });
      }
    };
    request.send();
  },
  computed: {
    filter() {
      if (this.filterInput === ``) {
        return this.countriesObject;
      } else {
        return this.countriesObject.filter((country) => {
          return country.region === this.filterInput;
        });
      }
    },
  },
  methods: {
    searchFilter() {
      let filter = [];
      if (this.countryName) {
        filter = this.countriesObject.filter((country) => {
          return country.name
            .toLowerCase()
            .startsWith(this.countryName.toLowerCase());
        });
        this.countriesObject = filter;
      } else {
        location.reload();
      }
    },
  },
};
</script>
<style scoped>
section {
  background-color: hsl(0, 0%, 98%);
}
section.dark {
  background-color: hsl(207, 26%, 17%);
}
a {
  text-decoration: none !important;
}
.card {
  flex-basis: 20% !important;
  /* background-color: white !important; */
  height: 90%;
  color: hsl(200, 15%, 8%) !important;
  width: 260px;
  margin-bottom: 30px;
}
.card.dark {
  background-color: hsl(209, 23%, 22%) !important;
  color: white !important;
}
img {
  width: 260px;
}
span {
  color: hsl(0, 0%, 52%);
}
input::placeholder {
  padding: 20px;
}
input.dark {
  background-color: hsl(209, 23%, 22%) !important;
  color: white;
}
select.dark {
  background-color: hsl(209, 23%, 22%) !important;
  color: white;
}

i {
  position: absolute;
  top: 12px;
  left: 1%;
  color: hsl(0, 0%, 52%);
}
</style>
