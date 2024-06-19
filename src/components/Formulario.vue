<script setup>

    import { reactive, computed } from 'vue';
    import Alerta from './Alerta.vue';

    const props = defineProps({
        id:{
            type: [String, null],
            required: true
        },
        nombre: {
            type: String,
            required: true
        },
        propietario: {
            type: String,
            required: true
        },
        email: {
            type: String,
            required: true
        },
        alta:{
            type: String,
            required: true
        },
        sintomas: {
            type: String,
            required: true
        }
    });

    const alerta = reactive({
        tipo: '',
        mensaje: ''
    });

    const emit = defineEmits(['update:nombre', 'update:propietario','update:email', 'update:alta', 'update:sintomas', 'guardar-paciente']);

    const validar = () =>{

        //console.log(Object.values(paciente));
        if(Object.values(props).includes('')){
            alerta.mensaje = 'Todos los campos son obligatorios';
            alerta.tipo = 'error'; 
            
            return;
        }

        emit('guardar-paciente');

        alerta.mensaje = 'Paciente almacenado correctamente';
        alerta.tipo = 'exito';

        setTimeout(() =>{
            Object.assign(alerta,{
                tipo: '',
                mensaje:''
            })
        },3000);

    }

    const editando  = computed(() => {
        return props.id;
    });

</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimineto Pacientes</h2>
        <p class="text-lg mt-5 text-center mb-10">
            Añade pacientes y <span class="text-indigo-600 font-bold">Adminístralos</span>
        </p>

        <Alerta v-if="alerta.mensaje" :alerta="alerta" />

        <form class="bg-white shadow-md rounded-lg py-10 px-5 mb-10" @submit.prevent="validar">
            
            <div class="mb-5">
                <label for="mascota" class="block text-gray-700 uppercase font-bold">Nombre mascota</label>
                <input type="text" id="mascota" placeholder="Nombre" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" @input="$emit('update:nombre', $event.target.value)" :value="nombre">
            </div>

            <div class="mb-5">
                <label for="propietario" class="block text-gray-700 uppercase font-bold">Nombre Propietario</label>
                <input type="text" id="propietario" placeholder="Propietario" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" @input="$emit('update:propietario', $event.target.value)" :value="propietario">
            </div>

            <div class="mb-5">
                <label for="email" class="block text-gray-700 uppercase font-bold">Email</label>
                <input type="email" id="email" placeholder="Email" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" @input="$emit('update:email', $event.target.value)" :value="email">
            </div>

            <div class="mb-5">
                <label for="alta" class="block text-gray-700 uppercase font-bold">Fecha alta</label>
                <input type="date" id="alta" placeholder="Fecha alra" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" @input="$emit('update:alta', $event.target.value)" :value="alta">
            </div>

            <div class="mb-5">
                <label for="sintomas" class="block text-gray-700 uppercase font-bold">Sintomas</label>
                <textarea id="sintomas" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40" placeholder="Describe los sintomas" @input="$emit('update:sintomas', $event.target.value)" :value="sintomas"></textarea>
            </div>

            <input type="submit" class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors" :value="[editando ? 'Guardar Cambios' : 'Registrar']">

        </form>

    </div>  
</template>