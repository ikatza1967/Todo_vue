<script setup>
import { ref } from "vue";
import axios from "axios";

const titulo = ref("");
const fecha = ref("");
const contenido = ref("");

const datosJson = ref("");

const editfecha = ref("");
const edittitulo = ref("");
const editcontenido = ref("");

//mostrar tareas

let headersList = {
  Accept: "*/*",
  "User-Agent": "Thunder Client (https://www.thunderclient.com)",
};

let reqOptions = {
  url: "https://jesus-78vd.onrender.com/tareas",
  method: "GET",
  headers: headersList,
};

//mostras tareas

const getDatos = async () => {
  const { data } = await axios.request(reqOptions);
  datosJson.value = data;
  console.log(data);
};
getDatos();

// crear post

const CreatePost = () async=> {
 await axios.post("https://jesus-78vd.onrender.com/tareas", {
    titulo: titulo.value,
    fecha: fecha.value,
    contenido: contenido.value,
  });
};

// crear delete

function deleteTask(id) {
  const url = `https://jesus-78vd.onrender.com/tareas/${id}`;
  const datosDelete = axios.delete(url);
  location.reload();
}

// editar comentario

const idTask = ref("");
let mostrar = ref(false);

function editTask(id) {
  idTask.value = id;
  mostrar.value = true;
  console.log("id", idTask.value);
}

function showEdit() {
  try {
    axios.put(`https://jesus-78vd.onrender.com/tareas/${idTask.value}`, {
      titulo: edittitulo.value,
      fecha: editfecha.value,
      contenido: editcontenido.value,
    });
    location.reload();
  } catch (error) {
    console.log(error);
  }
}
</script>

<template>
  <div class="fondo">

  <h1 class="text-center w-100 border mb-5">TO DO LIST</h1>

  <div class="d-flex container ">
    <div class="create_task border col p-5">
      <h1>CREAR TAREAS</h1>
      <form class="">
        <input
          class="w-75 mb-3"
          placeholder="Nombre de la tarea"
          type="text"
          v-model="titulo"
        />
        <input class="date" type="date" v-model="fecha" />
        <textarea
          cols="30"
          placeholder="Descripción"
          class="w-100"
          rows="10"
          v-model="contenido"
        ></textarea>
        <button type="submit" class="w-100" @click="CreatePost()">
          guardar
        </button>
      </form>
    </div>

    <div class="viewtask border col p-5 ">
      <h1>Tareas</h1>

      <div
        class="cuadroDerecha d-flex border rounded-1 p-1 gap-1 p-2 flex-wrap"
        v-for="datos in datosJson"
        :key="datosJson.id"
      >
        <h6 class="col-9">{{ datos.fecha }}</h6>

        <button @click="deleteTask(datos.id)">
          <i class="fa-solid fa-trash-can"></i>
        </button>

        <button @click="editTask(datos.id)">
          <i class="fa-solid fa-pen-to-square"></i>
        </button>
        <h5 class="border rounded-1 col-12">{{ datos.titulo }}</h5>

        <h6 class="border rounded-1 col-12">{{ datos.contenido }}</h6>
        <img
          class="img"
          src="src/assets/istockphoto-1330040188-170667a.jpg"
          alt="" 
        />
      </div>
      
      <div v-show="mostrar">
        <h4>Para editar</h4>
        <input type="date" v-model="editfecha" />
        <input type="text" v-model="edittitulo" />
        <input type="text" v-model="editcontenido" />
        <button @click="showEdit()"><i class="fa-solid fa-check"></i></button>
      </div>
    </div>
  </div>
</div>
</template>
<style scoped>
.date {
  width: 1.6rem;
}
.create_task {
  background-color: aquamarine;
}
.viewtask {
  background-color: rgb(165, 230, 175);
}
h1 {
  background-color: rgb(135, 150, 222);
}
img {
  width: 50px;
}
.img {
  margin-left: 12rem;
}
.fondo {
  background-image: url(src/assets/piedras.jpeg);
  padding-bottom: 5rem;
  background-repeat: no-repeat;
  background-size: cover;
}
.cuadroDerecha {
  background-color: rgb(0, 174, 255);
}

</style>
