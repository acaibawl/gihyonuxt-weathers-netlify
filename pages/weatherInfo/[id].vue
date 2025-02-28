<script setup lang="ts">
import type { City } from '~/interfaces';

// ルートオブジェクトを用意
const route = useRoute();
// 都市情報リストをステートから取得
const cityList = useState<Map<number, City>>("cityList");
// ルートパラメータをもとに該当都市データを取得
const selectedCity = computed(
  (): City => {
    const idNo = Number(route.params.id);
    return cityList.value.get(idNo) as City;
  }
)

const asyncData = useWeatherInfoFetcher(selectedCity.value);
const weatherDescription = asyncData.data;
const status = asyncData.status;
</script>

<template>
  <section>
    <h2>{{ selectedCity.name }}の天気</h2>
    <p v-if="status == 'pending'">データ取得中・・・</p>
    <p v-else>{{ weatherDescription }}</p>
  </section>
  <p>リストに<NuxtLink :to="{name: 'index'}">戻る</NuxtLink></p>
</template>