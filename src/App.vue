

<script setup>
import {onMounted, ref, watch} from 'vue'

// watch
const tempWatch = ref(false)
const qustion = ref('');
const resultLInk = ref('')
const img = ref('')
const validateNewVal = ref(false)
const answer = ref('give me a question')
const random = ref()
const idName = ref('')
function randomRl(){
 const ex = Math.floor(Math.random()*1885942)+1;
 console.log(ex)
 random.value = ex
}

watch(qustion, async (newVal) => {
  if(newVal.length > 0 && validateNewVal.value == true){
    tempWatch.value = true;
    answer.value = 'thinking...';

    
    try{
      const res = await fetch('https://pixabay.com/api/?key=45259758-7fdb32b94eb6483aa0a4b8024&q=' + newVal + '&image_type=photo&id=');
      const temp = await res.json()  
      console.log(temp)
      img.value = temp.hits.map(item =>({
        url: item.previewURL,
        name: item.tags
      }))
    }catch(err){
      answer.value = 'error'
    }
  }
})

onMounted(()=>{
  randomRl()
})

</script>

<template>
 <main class="watch">
  <input type="text" v-model="qustion" class="border" :disabled="tempWatch">
  <button @click="validateNewVal = true">click</button>
  <p>{{ answer }}</p>
  <div class="wrapper flex flex-wrap gap-5">
  <div v-for="(imgs, index) in img" :key="index">
  <img :src="imgs.url" alt="" class=" w-500 h-500 rounded" id="img">
  <label for="img">{{imgs.name}}</label>
  </div>
  </div>
 </main>
</template>