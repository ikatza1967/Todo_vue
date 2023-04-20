<script setup>
import { ref } from "vue";
import axios from "axios";


const titulo = ref("");
const fecha = ref("");
const contenido = ref("");

const datosJson = ref ("")

//mostrar tareas

let headersList = {
 "Accept": "*/*",
 "User-Agent": "Thunder Client (https://www.thunderclient.com)" 
}

let reqOptions = {
  url: "http://localhost:3000/tareas",
  method: "GET",
  headers: headersList,
}

const getDatos = async () =>{
  const response = await axios.request(reqOptions)
  datosJson.value = response.data
  console.log(response.data);
}
getDatos()

//guardar tareas



            // crear post
            const CreatePost = async () => {
  await axios({
   method: 'post',
   url: 'http://localhost:3000/tareas',
   headers: {
     'Content-Type': 'application/json'
   },
   data: {
     "titulo": titulo.value,
     "fecha": fecha.value,
     "contenido": contenido.value
   }
 })
 reload();
            }
                              // crear delete
  async function deletecomentario(id) {
  const url = `http://localhost:3000/tareas/${id}`;
  const response = await fetch(url, {
    method: 'DELETE',
    headers: { 'Content-Type': 'application/json' }
  })
  //recarga
  
}           

</script>

<template>
  <h1 class="text-center w-100 border mb-5 mt-4">TO DO LIST</h1>

  <div class="d-flex  container">

    <div class="create_task border col p-5">
      <h1>CREAR TAREAS</h1>
      <form >
        <input class="w-75 mb-3" placeholder="Nombre de la tarea" type="text" v-model="titulo" />
        <input class="date"  type="date" v-model="fecha" />
        <textarea cols="30" placeholder="Descripción" class="w-100" rows="10" v-model="contenido"></textarea>
        <button type="submit" class="w-100" @click="CreatePost">guardar</button>
      </form>
    </div>

    <div class="viewtask border col p-5">
      <h1>Tareas</h1>

      <div   class="d-flex border rounded-1 p-1 gap-1 p-2 flex-wrap "
      v-for="datos in datosJson" :key="datosJson.id"
      >

        <h6 class="col-10 ">{{ datos.fecha }}</h6>

        <i class="fa-solid fa-trash-can" @click="deletecomentario"></i>

        <i class="fa-solid fa-pen-to-square"></i>

        <h5 class="border rounded-1 col-12">{{ datos.titulo }}</h5>

        <p class="border rounded-1 col-12">{{ datos.contenido }}</p>

      </div>

    </div>

  </div>
</template>
<style scoped>
.date{
  width: 1.6rem;
}

</style>

