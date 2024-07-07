<!-- <script setup>
import AppCard from './components/AppCard.vue';
import AppButton from './components/AppButton.vue';
import AppInput from './components/AppInput.vue';
import TheMainNav from './components/TheMainNav.vue';
import FlexBox from './components/FlexBox.vue';
import Grid from './components/Grid.vue';

import { useDark } from '@vueuse/core';
const isDark = useDark();
</script>

<template>
  <div class="m-10">
    <TheMainNav />
    <AppCard />

    <AppButton class="mt-3">Submit</AppButton>
    <AppInput label="name" />

    <p class="first-letter:text-5xl selection:bg-blue-500">
      Lorem, ipsum dolor sit amet consectetur adipisicing elit. Corrupti sequi
      aspernatur deleniti, sed minus vero iusto repudiandae quaerat molestias
      asperiores delectus, quis debitis explicabo voluptatibus. Adipisci quasi
      vero error voluptates?
    </p>

    <ul class="list-disc marker:text-green-500 marker:text-2xl">
      <li>Vue</li>
      <li>React</li>
      <li></li>
      <li></li>
    </ul>
  </div>

  <input type="checkbox" v-model="isDark" />

  <FlexBox />

  <Grid />
</template> -->
<script setup>
import TreeGrid from './components/TreeGrid.vue';
</script>
<template>
  <div>
    <TreeGrid />
  </div>
</template>
