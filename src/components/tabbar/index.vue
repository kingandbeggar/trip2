<template>
  <div class="tabbar">
    <template v-for="item,index in tababrData" :key="item.path">
      <div 
          class="item" 
          :class="{active: index === currentIndex}" 
          @click="clickTabbarItem(item, index)"
          >
        <img v-if="currentIndex !== index" :src="getAssetsUrl(item.url)" alt="">
        <img v-else :src="getAssetsUrl(item.activeUrl)" alt="">
        <span class="name">{{ item.name }}</span>
      </div>
    </template>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";

import tababrData from "../../assets/data/tabbar"
import { getAssetsUrl } from "@/utils/load_assets";

import { useRouter,useRoute } from "vue-router";
const router = useRouter()
const route = useRoute()

const currentIndex = ref(0)
const clickTabbarItem = (item, index) => {
  currentIndex.value = index
  router.push(item.path)
}

watch(route, (newvalue, oldvalue) => {
  const index = tababrData.findIndex(item => item.path === newvalue.path)
  if(index !== -1) currentIndex.value = index
})

</script>

<style lang="less" scoped>
.tabbar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  height: 55px;
  border-top: 1px solid #000;
  text-align: center;
  .item {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    flex: 1;
    img {
      width: 36px;
      height: 28px;
    }
    &.active {
      .name {
        color: var(--primary-color);
      }
    }
  }
}
</style>
