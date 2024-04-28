<script setup> 
    import {ref, watch, computed} from 'vue';
    import IndexPage from './IndexPage.vue';

    //VARIABLES
    const tarea = ref('');
    const ocultarTareas = ref(JSON.parse(localStorage.getItem('ocultarTareas')) || false);
    const listaTareas = ref(JSON.parse(localStorage.getItem('listaTareas')) || []);

    //ESTILOS
    function esPar(numero){
        return numero % 2 === 0;
    }

    //EVENTOS
    const agregarTarea = () =>{
        listaTareas.value.push({nombre: tarea.value, tareaHecha: false})
        tarea.value = ''
    }
    const eliminarTarea = (value) =>{
        listaTareas.value = listaTareas.value.filter( e => e != value)
    }
    const tareasTerminadas = computed( () =>{
        let cont = 0
        listaTareas.value.forEach( e =>{
            if(e.tareaHecha){
                cont++
            }
        })
        
        return cont;
    })
    const filtrarTareas = computed( () =>{
        if(ocultarTareas.value){
            return listaTareas.value.filter( e => e.tareaHecha == false)
        }else{
            return listaTareas.value;
        }
    })

    watch(listaTareas, (nuevaTarea) =>{
        localStorage.setItem('listaTareas', JSON.stringify(nuevaTarea))
    }, {deep: true})

    watch(ocultarTareas, (nuevoValor) =>{
        localStorage.setItem('ocultarTareas', JSON.stringify(nuevoValor))
    }, {deep: true})
</script>

<template>
    <q-page class="contenedor">
        <h1 class="bg-black text-white q-pa-sm title">Hacer Tareas</h1>
        <q-input style="width: 400px;" v-model="tarea" @keyup.enter="agregarTarea"></q-input>
        <br>
        <q-btn @click="agregarTarea">Agregar</q-btn>

        <ul v-if="listaTareas.length > 0">
           <div class="lista">
            <li
            v-for="(value, index) in filtrarTareas"
            :key="index"
            :class="{'bg-black text-white q-pa-xs': esPar(index)}">
            <IndexPage 
            :nombre="value.nombre"
            :tareaHecha="value.tareaHecha"
            :posicion="index"
            />

            <q-toggle v-model="value.tareaHecha"/>
            <q-btn icon="delete" @click="eliminarTarea(value)"></q-btn>
            </li>
           </div>
        </ul>
        <p v-else class="q-mt-lg">No hay tareas Pendientes!</p>

        <h5>Tareas Terminadas: {{ tareasTerminadas }}</h5>
        <h5>Ocultar Tareas Terminadas <q-toggle v-model="ocultarTareas" /></h5>
    </q-page>
</template>

<style scoped>
 .contenedor{
    display: flex;
    flex-direction: column;
    align-items: center;
 }
 .lista{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 15px;
 }
 li{
    list-style-type: none;
 }
 .title{
    border: 2px solid black;
    border-radius: 10px;
    font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
 }
</style>