<script setup>

    import {ref, reactive, onMounted, watch} from 'vue';
    import { v4 as uuidv4 } from 'uuid';
    import Header from './components/Header.vue';
    import Formulario from './components/Formulario.vue';
    import Paciente from './components/Paciente.vue';

    const pacientes = ref([]);

    const paciente =  reactive({
        id: null,
        nombre: '',
        propietario: '',
        email: '',
        alta: '',
        sintomas: ''
    });

    onMounted(() => {

        const pacientesStorage = localStorage.getItem('pecientesStorage');

        if(pacientesStorage){
            pacientes.value = JSON.parse(pacientesStorage);
        }

    });

    watch(pacientes, () =>{
        guardarLocalStorage();
        },{
            deep: true, 
    });

    const guardarPaciente = () =>{

        if(paciente.id){

            const { id } = paciente;
            const index = pacientes.value.findIndex((pacienteState) => pacienteState.id === id );
            
            pacientes.value[index] = {...paciente};
            
        }
        else{
            pacientes.value.push({...paciente, id: uuidv4()});
        }

        Object.assign(paciente, {
            id:null,
            nombre: '',
            propietario: '',
            email: '',
            alta: '',
            sintomas: ''
        });
        
    }

    const actualizarPaciente = (id) =>{
        
        const pacienteEditar = pacientes.value.filter( paciente => paciente.id === id)[0];
        Object.assign(paciente,pacienteEditar)
    }

    const eliminarPaciente = (id) =>{
        
        pacientes.value = pacientes.value.filter(paciente => paciente.id != id);

    }

    const guardarLocalStorage = () =>{
        localStorage.setItem('pecientesStorage', JSON.stringify(pacientes.value));
    }

</script>

<template>
    <div class="container mx-auto mt-20"> 
        
        <Header/>

        <div class="mt-12 md:flex">
            
            <Formulario
                :id="paciente.id"
                v-model:nombre="paciente.nombre"
                v-model:propietario="paciente.propietario"
                v-model:email="paciente.email"
                v-model:alta="paciente.alta"
                v-model:sintomas="paciente.sintomas"
                @guardar-paciente="guardarPaciente"
            />

            <div class="md:w-1/2 md:h-screen overflow-y-scroll">

                <h3 class="font-black text-3xl text-center">Administra tus pacientes</h3>
                
                <div v-if="pacientes.length > 0">

                    <p class="text-lg mt-5 text-center mb-10">
                        Información <span class="text-indigo-600 font-bold">Adminístralos</span>
                    </p>

                    <Paciente
                        v-for="paciente in pacientes"
                        :paciente="paciente"
                        @actualizar-paciente="actualizarPaciente"
                        @eliminar-paciente="eliminarPaciente"
                    />

                </div>

                <p v-else class="mt-10 text-2xl text-center">No hay pacientes para mostrar</p>

            </div>

        </div>

    </div>

</template>


