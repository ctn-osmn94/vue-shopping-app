<script setup>
import { ref } from 'vue'

const amount = ref(0)
const shopList = ref([])
const Basket = ref([])

function getItems() {
    fetch("/shop.json").then(res=>res.json())
      .then((data) => {
        for (let i = 0; i < data.length; i++) {
          shopList.value.push(data[i])          
        }
    })
}
getItems()

function addBasket(item) {
      amount.value++
      Basket.value.push(item)
}

function totalPrice() {
    return Basket.value.reduce((current,next)=>
    current+next.price,0)
}
totalPrice()
addBasket()
function deleteItem(index) {
      Basket.value.splice(index,1)
      amount.value--
      return Basket.value.reduce((current,next)=>
      next-current.price,0)
}
deleteItem()



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
                <button class="p-3 bg-green-200 rounded-md" @click="addBasket(item)">basket
                
                </button>
            </div>
        </div>
    </div>
    <div class="container mx-auto ">
        <h1 class="font-bold text-2xl mb-8">BASKET</h1>
        <div class="flex flex-col gap-5">
            
            <div class="flex gap-3 items-center" v-for="(item,index) in Basket"
            :key="item.id" >
                <p> {{item.name}} </p>
                <p> {{item.price}} $ </p>
                <i @click="deleteItem(index)" class="fa-solid fa-trash-can cursor-pointer"></i>
            </div>
            <p> total amount: {{amount}} </p>
            <p> total price: {{totalPrice()}} $ </p>
        </div>
    </div>
</template>

