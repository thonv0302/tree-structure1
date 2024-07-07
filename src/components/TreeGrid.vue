<script setup>
import { computed } from 'vue';
import CardGrid from './CardGrid.vue';
import { myInjectionKey, isCounter } from './key.js';

import { ref, provide, onMounted } from 'vue';

const data = ref([
  {
    id: 1,
    name: 'Mobile',
    childrenCount: 3,
    isShowChildren: false,
    isActive: false,
    children: [
      {
        id: 11,
        name: 'MNO',
        childrenCount: 5,
        isShowChildren: false,
        isActive: false,
        children: [],
      },
      {
        id: 12,
        name: 'MVNO',
        childrenCount: 3,
        isShowChildren: false,
        isActive: false,
        children: [
          {
            id: 121,
            name: 'CDMA',
            childrenCount: 2,
            isShowChildren: false,
            isActive: false,
            // children: [],
            children: [
              {
                id: 1211,
                name: 'Smart Phone',
                childrenCount: 5,
                children: [],
                isShowChildren: false,
                isActive: false,
                children: [
                  {
                    id: 12111,
                    name: 'General',
                    childrenCount: 1,
                    isActive: false,
                    children: [],
                  },
                  {
                    id: 12112,
                    name: 'Kids',
                    childrenCount: 1,
                    isActive: false,
                    children: [],
                  },
                  {
                    id: 12113,
                    name: 'Junior',
                    childrenCount: 7,
                    isActive: false,
                    children: [],
                  },
                  {
                    id: 12114,
                    name: 'Senior',
                    childrenCount: 3,
                    isActive: false,
                    children: [],
                  },
                  {
                    id: 12115,
                    name: 'Company',
                    childrenCount: 3,
                    isActive: false,
                    children: [],
                  },
                ],
              },
              {
                id: 1212,
                name: 'Tablet',
                childrenCount: 3,
                isShowChildren: false,
                isActive: false,
                children: [],
              },
              {
                id: 1213,
                name: 'Wearable',
                childrenCount: 4,
                isShowChildren: false,
                isActive: false,
                children: [],
              },
              {
                id: 1214,
                name: 'Router',
                childrenCount: 3,
                isActive: false,

                children: [],
              },
              {
                id: 1215,
                name: 'M2M',
                childrenCount: 6,
                isShowChildren: false,
                isActive: false,
                children: [
                  {
                    id: 12151,
                    name: 'M2M-1',
                    childrenCount: 3,
                    isShowChildren: false,
                    isActive: false,
                    children: [],
                  },
                  {
                    id: 12152,
                    name: 'M2M-2',
                    childrenCount: 5,
                    isShowChildren: false,
                    isActive: false,
                    children: [],
                  },
                ],
              },
            ],
          },
          {
            id: 122,
            name: 'LTE',
            childrenCount: 5,
            isShowChildren: false,
            isActive: false,
            children: [],
          },
          {
            id: 123,
            name: 'SG',
            childrenCount: 6,
            isShowChildren: false,
            isActive: false,
            children: [],
          },
        ],
      },
    ],
  },
  {
    id: 2,
    name: 'Laptop',
    childrenCount: 3,
    isShowChildren: false,
    isActive: false,
    children: [],
  },
]);

const deactivateActiveItems = (items) => {
  items.forEach((item) => {
    if (item.isActive) {
      item.isActive = false;
    }
    if (item.children && item.children.length > 0) {
      deactivateActiveItems(item.children);
    }
  });
};

const countElement = (items) => {
  let mapObj = {};
  items.forEach((item) => {
    if (item.children && item.children.length > 0) {
      if (item.isShowChildren) {
        countElement(item.children);
      }
    }
  });
};

// Function to calculate total number of children recursively
function calculateChildrenCount(node) {
  let count = 0;
  if (node.isShowChildren && node.children.length > 0) {
    count = node.children.length;
  }
  for (const child of node.children) {
    count = count + calculateChildrenCount(child);
  }
  return count;
}

// Function to update the children count for each node in the tree
function updateChildrenCounts(nodes) {
  for (const node of nodes) {
    if (node.id === nodes[nodes.length - 1].id) {
      console.log('node: ', node);
      console.log('nodes: ', nodes);
      node.childrenCount1 = nodes.length;
    } else {
      node.childrenCount1 = node.childrenCount;
    }
    // else {
    // node.childrenCount1 = calculateChildrenCount(node);
    // }

    if (node.children.length > 0) {
      updateChildrenCounts(node.children);
    }
  }
}

provide(myInjectionKey, {
  /* data to provide */
  refreshActive: () => {
    deactivateActiveItems(data.value);
  },
  calcLineHeight: (parentId) => {
    let height = 0;
    items.forEach((item) => {
      if (item.isShowChildren) {
      }
      // if (item.isShowChildren) {
      //   item.isActive = false;
      // }
      // if (item.children && item.children.length > 0) {
      //   deactivateActiveItems(item.children);
      // }
    });
  },
});

let isFlag = ref(false);
const recieveData = (data) => {
  data.isActive = true;
  isCounter.value = false;
};

// const calcLineHeight = computed(() => (parentId) => {
//   return parentId;
// });
</script>

<template>
  <!-- <pre>{{ data }}</pre> -->
  <div class="flex flex-row gap-6">
    <div class="w-[200px] h-[30px] border">1</div>
    <div class="w-[200px] h-[30px] border">2</div>
    <div class="w-[200px] h-[30px] border">3</div>
    <div class="w-[200px] h-[30px] border">4</div>
    <div class="w-[200px] h-[30px] border">5</div>
  </div>
  <div v-for="item in data" :key="item.id" class="w-100 mt-6">
    <CardGrid :items="data" :item="item" @sendActive="recieveData" />
  </div>
  <pre>{{ updateChildrenCounts(data) }}</pre>
</template>

<style>
.arrow {
  stroke: black;
  fill: none;
  stroke-width: 2;
}
.line {
  position: absolute;
  height: 2px;
  background-color: black;
  transform-origin: top left;
}
</style>
