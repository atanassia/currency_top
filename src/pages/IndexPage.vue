<template>
  <q-page class="flex flex-center">
    <div class="row text-center">
      <div class="col-12 text-h6 text-weight-thin">Курс валют на {{ currentDate.format("D MMMM YYYY") }}</div>
      <div class="col-12">1 USD = {{ currentValue }} RUR</div>
    </div>
  </q-page>
</template>

<script setup>
import { api } from 'src/boot/axios';
import { ref } from 'vue';
import dayjs from 'dayjs';

import { useQuasar } from 'quasar'

const $q = useQuasar();
const currentValue = ref(0);
const currentDate = ref(dayjs());

window.addEventListener('online', (event) => {
  $q.notify({
    color:"positive",
    message: "В сети",
    multiline: true,
    position: "top"
  });
});

window.addEventListener('offline', (event) => {
  $q.notify({
    color:"warning",
    message: "Нет сети",
    multiline: true,
    position: "top"
  });
});


api.get('https://www.cbr-xml-daily.ru/daily_json.js')
.then((response) => {
  currentValue.value = response.data.Valute.USD.Value;
})
</script>
