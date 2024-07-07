<script setup>
import { computed, ref, inject, onMounted } from 'vue';
import CardGrid from './CardGrid.vue';
import { myInjectionKey, isCounter } from './key.js';
import { onUpdated } from 'vue';
import { watch } from 'vue';
const props = defineProps({
  item: {
    type: Object,
    default: () => ({}),
  },
  level: {
    type: Number,
    default: 1,
  },
  // calcHeight: {
  //   type: Object,
  // },
});

const emits = defineEmits(['sendActive']);
const injected = inject(myInjectionKey);

const hasChildren = computed(() => !!props.item.children.length);
const isShowChildren = computed(() => !!props.item.isShowChildren);

const isDisplayChildren = computed(
  () => hasChildren.value && isShowChildren.value
);

const displayChildren = () => {
  props.item.isShowChildren = !props.item.isShowChildren;
};

const bubleToParent = () => {
  // console.log('item: ', props.item);
};

const count = ref(1);

const recieveData = (data) => {
  console.log('data: ', data);
  data.isActive = true;
};

const sendData = () => {
  if (!isCounter.value) {
    injected.refreshActive();
  }
  isCounter.value = true;
  emits('sendActive', props.item);
};

const countItem = computed(() => {
  // return props.item.children;
  let height = 0;
  props.item.children.forEach((itemChild) => {
    if (itemChild.children && itemChild.children.length > 0) {
      height += 1;
    }
  });

  return props.item.children.length;
});

const computedHeight = computed(() => {
  console.log('props.item.children: ', props.item.children);
  return (
    props.item.childrenCount1 * 30 + (props.item.childrenCount - 1) * 8 + 'px'
  );
});

onMounted(() => {
  if (props.item.children.length > 1) {
    const firstItem = document.getElementById(props.item.children[0].id);
    const lastItem = document.getElementById(
      props.item.children[props.item.children.length - 1].id
    );

    if (firstItem && lastItem) {
      const firstItemY = firstItem.getBoundingClientRect().y;
      const lastItemY = lastItem.getBoundingClientRect().y;
      testHeight.value = lastItemY - firstItemY;
    }
  }
});

let testHeight = ref(0);

watch(
  () => [props.item],
  (newVal) => {
    if (props.item.children.length > 1) {
      const firstItem = document.getElementById(props.item.children[0].id);
      const lastItem = document.getElementById(
        props.item.children[props.item.children.length - 1].id
      );

      if (firstItem && lastItem) {
        const firstItemY = firstItem.getBoundingClientRect().y;
        const lastItemY = lastItem.getBoundingClientRect().y;
        testHeight.value = lastItemY - firstItemY;
      }
    }
  },
  {
    // immediate: true,
    deep: true,
    flush: 'post',
  }
);
</script>
<template>
  <div class="flex flex-row gap-6 relative" @click="sendData">
    <div
      :class="[
        'w-[200px] h-[30px] border  flex justify-between',
        props.item.isActive ? 'border-red-500' : 'border-gray-800',
      ]"
    >
      <!-- <div>{{ props.calcHeight(1) }}</div> -->
      <div>{{ props.item.name }}</div>
      <div>{{ props.item.childrenCount }}</div>
      <button @click.stop="displayChildren" v-if="hasChildren">Show</button>
    </div>

    <div
      v-if="isDisplayChildren"
      :class="[hasChildren ? 'flex flex-col relative' : '']"
    >
      <div
        class="border-l border-gray-500 absolute -left-3 top-[15px]"
        :style="{ height: testHeight + 'px' }"
      ></div>
      <div class="w-[24px] h-[1px] bg-black absolute -left-6 top-4"></div>
      <div
        v-for="(childItem, index) in props.item.children"
        class="mb-2 relative"
        :id="childItem.id"
      >
        <div
          :class="[
            'w-[12px] h-[1px] absolute -left-3 top-4',
            false ? 'bg-red-600' : 'bg-black',
          ]"
        ></div>
        <CardGrid
          @sendActive="recieveData"
          :item="childItem"
          :level="props.level + 1"
        />
      </div>
    </div>
    <!-- {{ countItem }} -->
  </div>
</template>
