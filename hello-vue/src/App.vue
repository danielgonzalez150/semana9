<template>
  <h1>Gestor de tareas</h1>
  <div class="container">
    <input v-model="nuevaTarea" placeholder="Escribe la nueva tarea a realizar" @keyup.enter="agregarTarea" />
    <button @click="agregarTarea"> Agregar </button>
  </div>
  <hr>
  <div class="tareas" v-if="tareasPorHacer.length > 0 || tareasEnProceso.length > 0 || tareasRealizadas.length > 0">
   <!--El if hace que no se muestren estudiantes si hay al menos 1-->
    <h2>Tabla de tareas</h2>
    <div class="tablero">
    <section class="tareas-column todo">
      <h3>Tareas por hacer</h3>
      <ul>
        <li v-for="(tarea, index) in tareasPorHacer" :key="index">
          {{ tarea }}
          <button @click="porHacerAProceso(index)">-></button>
        </li>
      </ul>
    </section>
    <section class="tareas-column inprogress">
      <h3>Tareas en proceso</h3>
      <ul>
        <li v-for="(tarea, index) in tareasEnProceso" :key="index">
          {{ tarea }}
          <button @click="procesoARealizada(index)">-></button>
        </li>
      </ul>
    </section>
    <section class="tareas-column done">
      <h3>Tareas realizadas</h3>
      <ul>
        <li v-for="(tarea, index) in tareasRealizadas" :key="index">
          {{ tarea }}
        </li>
      </ul>
    </section>
    </div>
  </div>
  <div v-else>
    <p>No hay tareas agregadas</p>
  </div>
</template>
<script>
import { ref } from "vue";  //Ref significa que es reactivo a los cambios realizados
const nuevaTarea = ref(''); //Variable reactiva para el nuevo estudiante
const tareasPorHacer = ref([]);
const tareasEnProceso = ref([]);
const tareasRealizadas = ref([]); 
const agregarTarea = () => {
  const nombreTarea = nuevaTarea.value.trim(); //El trim elimina espacios en blanco al inicio y al final
  if (nombreTarea!=='') { //Si el nombre no está vacío
    tareasPorHacer.value.push(nombreTarea); //Añade el nuevo estudiante al array
    nuevaTarea.value = ''; //Limpia el campo de entrada
  }
}; //Función para añadir un estudiante
const porHacerAProceso = (index) => {
  const tarea = tareasPorHacer.value.splice(index, 1)[0]; //Elimina la tarea del array de tareas por hacer y la guarda en una variable
  tareasEnProceso.value.push(tarea); //Añade la tarea al array de tareas en proceso
};
const procesoARealizada = (index) => {
  const tarea = tareasEnProceso.value.splice(index, 1)[0]; //Elimina la tarea del array de tareas en proceso y la guarda en una variable
  tareasRealizadas.value.push(tarea); //Añade la tarea al array de tareas realizadas
};
export default {
  name: 'App',
  setup() {
    return {
      nuevaTarea,
      tareasPorHacer,
      tareasEnProceso,
      tareasRealizadas,   
      agregarTarea,
      porHacerAProceso,
      procesoARealizada,
    };
  }
};
</script>
<style>
.container {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 20px;
}
input {
  padding: 8px;
  width: 70%;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.todo button {
  padding: 8px 12px;
  margin-left: 8px;
  border: none;
  background-color: #28a745;
  color: white;
  border-radius: 4px;
  cursor: pointer;
}
.inprogress button {
  padding: 8px 12px;
  margin-left: 8px;
  border: none;
  background-color: #007bff;
  color: white;
  border-radius: 4px;
  cursor: pointer;
}
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
li {
  margin-bottom: 10px;
  background: white;
  padding: 6px 10px;
  border-radius: 4px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-color: darkslateblue;
  border-style: solid;
}
.tablero {
  display: flex; /* Habilita el modo Flexbox */
  gap: 20px; /* Espacio entre las columnas */
  padding: 10px;
  justify-content: center; 
  align-items: flex-start; 
}

.tareas-column {
  flex: 1; /* Hace que las 3 columnas tengan el mismo ancho */
  min-width: 0;
  padding: 15px;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
  background: #fdfdfd;
  max-height: 400px; 
  overflow-y: auto; 
}
</style>