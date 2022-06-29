<script setup>
import { ref } from 'vue'

const amount = ref(0)
const shopList = ref([])
const addBasket = ref([])

function getItems() {
    fetch("/shop.json").then(res=>res.json())
      .then((data) => {
        for (let i = 0; i < data.length; i++) {
          shopList.value.push(data[i])          
        }
    })
}
getItems()


</script>

<template>
    <div class="container mx-auto flex gap-10 py-16">
        <div 
        class="flex flex-col gap-10"
        v-for="(item) in shopList"
        :key="item.id"
        >
            <img class="w-full rounded-lg" :src="item.image" alt="">
            <div class="flex items-center justify-between">
                <p class="text-xl font-semibold"> {{item.name}} </p>
                <p class="text-xl font-semibold"> {{item.price}} $ </p>
                <button class="p-3 bg-green-200 rounded-md" @click="Basket(item)">basket
                
                </button>
            </div>
        </div>
    </div>
</template>

