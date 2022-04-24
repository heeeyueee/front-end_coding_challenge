<template>
<Header></Header>
  <div class="home-container page-container">
    <!-- <img class="vue-element-plus-logo" alt="Vue logo" src="../assets/logo.png" />
    <div class="page-title">Vite2.x + Vue3.x + TypeScript + Element Plus</div> -->
    <div class="header">
      <el-input
        v-model="searchInput"
        class="w-50 m-2 search"
        placeholder="Search for a country"
        :prefix-icon="Search"
      />
      <el-select
        v-model="value"
        class="m-2 select"
        placeholder="Filter by Region"
        size="large"
        @change="getCountriesInfo"
      >
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        />
      </el-select>
    </div>
    <div class="main" v-loading="loading">
      <div class="nation-item" v-for="(item, index) in CountriesInfo" :key="index">
        <div class="flag">
          <img :src="item.flags.png" />
        </div>
        <div class="info">
          <div class="name">{{ item.name.common }}</div>
          <div class="item">
            <p>Population:</p>
            <p>{{ item.population }}</p>
          </div>
          <div class="item">
            <p>Region:</p>
            <p>{{ item.region }}</p>
          </div>
          <div class="item">
            <p>Capital:</p>
            <p>{{ item.capital == undefined ? null : item.capital.join(',') }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, Ref, onBeforeMount } from 'vue'
import axios from '../utils/axios'
import Header from '../components/Header.vue'
import { Search } from '@element-plus/icons-vue'
import Header from '../components/Header.vue'
export default defineComponent({
    name: "Home",
    setup() {
        const CountriesInfo: Ref = ref([]);
        const value = ref("");
        const searchInput = ref("");
        const options = [
            {
                value: "africa",
                label: "Africa"
            },
            {
                value: "america",
                label: "America"
            },
            {
                value: "asia",
                label: "Asia"
            },
            {
                value: "europe",
                label: "Europe"
            },
            {
                value: "oceania",
                label: "Oceania"
            }
        ];
        const loading = ref(false);
        onBeforeMount(() => {
            getCountriesInfo("");
        });
        const getCountriesInfo = (value: string) => {
            const url = value === "" ? "/all" : "/region/" + value;
            console.log("url: ", url);
            loading.value = true;
            axios
                .get(url)
                .then((response) => {
                console.log("response: ", response.data);
                CountriesInfo.value = response.data;
                loading.value = false;
            })
                .catch((error) => {
                loading.value = false;
                console.error(error);
            });
        };
        return {
            value,
            options,
            CountriesInfo,
            searchInput,
            Search,
            loading,
            getCountriesInfo
        };
    },
    components: { Header }
})
</script>

<style scoped lang="stylus">
.home-container {
  display: flex;
  flex-direction: column;

  .vue-element-plus-logo {
    width: 50%;
  }

  .header {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    width: 90%;
    margin: 0 auto;
    margin-top: 20px;
  }

  .search {
    width: 30%;
  }
  .select{
    margin-right: 15px
  }

  .main {
    width: 90%;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    justify-content: space-between;
    margin: 0 auto;
    margin-top: 10px;
  }

  .nation-item {
    border: 1px solid rgba(0, 0, 0, 0.125);
    border-radius: 0.25rem;
    width: 260px;
    height: 320px;
    margin: 20px 15px 20px 0px;

    .flag {
      width: 260px;
      height: 180px;
      margin-bottom: 18px;
      border-bottom: 1px solid rgba(0, 0, 0, 0.125);
      border-radius: 0.25rem 0.25rem 0 0;

      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
      }
    }

    .info {
      display: flex;
      margin-left: 20px;
      flex-direction: column;
      align-items: flex-start;
      font: 'Nunito Sans', sans-serif;

      .name {
        font: 600 16px 'Nunito Sans', sans-serif;
        margin-bottom: 10px;
      }

      .item {
        display: flex;
      }

      p:nth-of-type(1) {
        font-size: 14px;
        font-weight: 600;
        margin: 3px 0px;
      }

      p:nth-of-type(2) {
        font-size: 14px;
        margin: 3px 2px;
      }
    }
  }
}
</style>
