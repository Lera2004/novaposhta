<template>
  <div class="example">
    <div class="block">
      <p class="text">Область:</p>
      <select v-model="area" @change="selectCity" class="select">
        <option value="" disabled selected>Область</option>
        <option v-for="a in areas" :key="a.Ref" :value="a.Ref">{{ a.Description }}</option>
      </select>
    </div>
    <div class="block">
      <p class="text">Населений пункт:</p>
      <select v-model="city" @change="selectWarehouses" class="select">
        <option value="" disabled selected>Населений пункт</option>
        <option v-for="c in cities" :key="c.CityID">{{ c.Description }}</option>
      </select>
    </div>
    <div class="block">
      <p class="text">Поштове відділення:</p>
      <select v-model="warehouse" class="select">
        <option disabled selected value="">Відділення</option>
        <option v-for="w in warehouses" :key="w.Ref">{{ w.Description }}</option>
      </select>
    </div>
    <div class="block">
      <p class="text">Ви обрали: <br>
        Населений пункт: <span class="selected">{{ city }}</span> <br>
        Поштове відділення: <span class="selected">{{ warehouse }}</span>
      </p>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';

export default {
  setup() {
    const cities = ref([]);
    const city = ref('');
    const warehouses = ref([]);
    const warehouse = ref('');
    const area = ref('');
    const areas = ref([]);

    const selectWarehouses = () => {
      axios.post("https://api.novaposhta.ua/v2.0/json/", {
        "apiKey": "6db6fbcd0e280f815aa6859b6ecdc4ec",
        "modelName": "Address",
        "calledMethod": "getWarehouses",
        "methodProperties": {
          "CityName": city.value
        }
      }).then(res => {
        warehouses.value = res.data.data;
      });
    };

    const selectCity = () => {
      axios.post("https://api.novaposhta.ua/v2.0/json/", {
        "apiKey": "6db6fbcd0e280f815aa6859b6ecdc4ec",
        "modelName": "Address",
        "calledMethod": "getCities",
        "methodProperties": {
          "AreaRef": area.value
        }
      }).then(res => {
        cities.value = res.data.data;
      });
    };

    onMounted(() => {
      axios.post("https://api.novaposhta.ua/v2.0/json/", {
        "apiKey": "6db6fbcd0e280f815aa6859b6ecdc4ec",
        "modelName": "Address",
        "calledMethod": "getAreas",
        "methodProperties": {}
      }).then(res => {
        areas.value = res.data.data;
      });
    });

    return {
      cities,
      city,
      warehouses,
      warehouse,
      area,
      areas,
      selectWarehouses,
      selectCity
    };
  }
};
</script>
