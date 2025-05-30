<template class="flex items-center ">
  <div class="flex p-60 pl-10 pr-20 lg:pl-110 lg:pr-20  gap-40 ">
    <div class="flex items-center md:p-10 bg-emerald-900  gap-8 fixed md:top-50 md:left-5 top-0 left-0  ">
      <input class="p-4 border border-solid" v-model="newWork" placeholder="yangi vazifa">
      <button class="btn btn-primary" @click="addWork">Qo'shish</button>
    </div>

    <div class="w-full ">
      <p class="" v-if="works.length===0">Ayni damda ma'lumot yo'q</p>
      <ul class="flex flex-col gap-3 ">
        <li class="border-2 flex items-center  p-3" v-for="(item,index) in works" :key="index">
          
          <p :class={on_true:item.done} class="text-2xl mr-auto text-red-500" @click="makeDone(index)">{{ item.work }}</p>
          <button class="mr-3 btn btn-secondary" @click="deleteWork(index)">Delete</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
  import {ref,watch} from "vue";

  const newWork = ref('');
  const works = ref(JSON.parse(localStorage.getItem('works'))||[]);

  function addWork (){
  if(newWork.value!==""){
    works.value.push({work:newWork.value, done:false});
    newWork.value = ""
  }else {
    alert("Kechirasiz, siz hali topshiriq yozmadingiz!!!")
  }
  }

  function makeDone(index){
      works.value[index].done = true;
  }

  function deleteWork(index){

    works.value.splice(index,1);
  }

  watch(works,(newVal)=>{
    localStorage.setItem('works',JSON.stringify(newVal))
  },{deep:true});
</script>