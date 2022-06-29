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
    <div class="container mx-auto flex gap-10 py-16 items-center justify-center">
        <div 
        class="flex flex-col gap-10 bg-slate-300 p-5 rounded-lg"
        v-for="(item) in shopList"
        :key="item.id"
        >
            <div class="rounded-lg w-full"><img class="w-96 h-60" :src="item.image" alt=""></div>
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
            
            <div class="flex gap-3 items-center bg-slate-300 p-3 w-60 rounded-lg" v-for="(item,index) in Basket"
            :key="item.id" >
                <img class="w-10 h-10" :src="item.image" alt="">
                <p> {{item.name}} </p>
                <p> {{item.price}} $ </p>
                <i @click="deleteItem(index)" class="fa-solid fa-trash-can cursor-pointer"></i>
            </div>
            <p> total amount: {{amount}} </p>
            <p> total price: {{totalPrice()}} $ </p>
        </div>
    </div>
</template>

