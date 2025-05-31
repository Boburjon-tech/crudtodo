<template class="flex items-center ">
  <div class="flex p-60 pl-10 pr-20 lg:pl-110 lg:pr-20  gap-40 ">
    <div class="flex items-center md:p-10   gap-8 fixed md:top-65 md:left-5 top-0 left-0  ">
      <input class="p-4 rounded-2xl border border-solid" v-model="newWork" placeholder="yangi vazifa">
      <button  class="btn btn-primary" @click="addWork">Qo'shish</button>
      
    </div>

    <div class="w-full ">
      <p class="" v-if="works.length===0">Ayni damda ma'lumot yo'q</p>
      <ul class="flex flex-col gap-3 ">
        <li class="border-2 rounded-2xl flex items-center bg-white  p-3" v-for="(item,index) in works" :key="index">
          
          <p :class={on_true:item.done} class="text-2xl mr-auto text-red-600" @click="makeDone(index)">{{ item.work }}</p>
          <button class="mr-3 btn btn-success" @click="editWork(index)">Edit</button>
          <button class="mr-3 btn btn-secondary" @click="deleteWork(index)">Delete</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
  import {ref,watch} from "vue";
  const newWork = ref('');
  const editwork = ref(false);
  const editindex = ref(null);
  const works = ref(JSON.parse(localStorage.getItem('works'))||[]);

  function addWork (){
  if(newWork.value!==""){
    if(!editwork.value){
      works.value.push({work:newWork.value, done:false});
     
    }
    else {
      works.value[editindex.value].work = newWork.value;
      
      if(works.value[editindex.value].done==true){
        works.value[editindex.value].done=false;
      };
      editwork.value  = false;
      editindex.value = false; 
    }
    newWork.value = ""
  }else {
    alert("Kechirasiz, siz hali topshiriq yozmadingiz!!!")
  }
  }

  function editWork(index){

    newWork.value = works.value[index].work;
    editwork.value = true;
    editindex.value  = index;
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