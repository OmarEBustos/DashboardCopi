<template>
    <div>
        <Sidebar />
        <div class="main-content">

            <TopNav />

            <div class="container-fluid">
                <div class="row justify-content-center">
                <div class="col-12 col-lg-10 col-xl-8">

                    <!-- Header -->
                    <div class="header mt-md-5">
                    <div class="header-body">
                        <div class="row align-items-center">
                        <div class="col">

                            <!-- Pretitle -->
                            <h6 class="header-pretitle">
                            Colaboradores
                            </h6>

                            <!-- Title -->
                            <h1 class="header-title">
                            Nuevo colaborador
                            </h1>

                        </div>
                        </div> <!-- / .row -->
                        <div class="row align-items-center">
                        <div class="col">

                            <!-- Nav -->
                            <ul class="nav nav-tabs nav-overflow header-tabs">
                            <li class="nav-item">
                               <router-link to="/colaborador" class="nav-link">Todos los colaboradores</router-link> 
                              
                            </li>
                            <li class="nav-item">
                                <a class="nav-link active">
                                    Nuevo colaborador
                                </a>
                                
                            </li>
                            
                            </ul>

                        </div>
                        </div>
                    </div>
                    </div>

                    <!-- Form -->


                    <div class="row">
                        <div class="col-12 col-md-6">

                        <!-- First name -->
                        <div class="form-group">

                            <!-- Label -->
                            <label class="form-label">
                            Nombre
                            </label>

                            <!-- Input -->
                            <input type="text" class="form-control" v-model="colaborador.nombres" placeholder="Nombre">

                        </div>

                        </div>
                        <div class="col-12 col-md-6">

                        <!-- Last name -->
                        <div class="form-group">

                            <!-- Label -->
                            <label class="form-label">
                            Apellido
                            </label>

                            <!-- Input -->
                            <input type="text" class="form-control" v-model="colaborador.apellidos" placeholder="Apellido">

                        </div>

                        </div>
                        <div class="col-12">

                        <!-- Email address -->
                        <div class="form-group">

                            <!-- Label -->
                            <label class="mb-1">
                            Correo electrónico
                            </label>

                            <!-- Form text -->
                            <small class="form-text text-muted">
                            This contact will be shown to others publicly, so choose it carefully.
                            </small>

                            <!-- Input -->
                            <input type="email" class="form-control" v-model="colaborador.email" placeholder="Correo electrónico">

                        </div>

                        </div>
                        
                        <div class="col-12 col-md-6">

                        <!-- Birthday -->
                        <div class="form-group">

                            <!-- Label -->
                            <label class="form-label">
                            Cargo
                            </label>

                            <!-- Input -->
                            <select name="" class="form-select" v-model="colaborador.rol">
                                <option value="" disabled selected>Seleccionar</option>
                                <option value="Administrador">Administrador</option>
                                <option value="Vendedor">Vendedor</option>
                                <option value="Inventariado">Inventariado</option>
                            </select>
                        </div>

                        </div>

                    </div> <!-- / .row -->

                    <!-- Divider -->
                    

        

                    <hr class="my-5">

                    <!-- Button -->
                    <button type="button" class="btn btn-primary" v-on:click="validar()">
                        Crear colaborador
                    </button>



                    <br><br>

                </div>
                </div> <!-- / .row -->
            </div>

        </div>
    </div>
</template>

<script>
// @ is an alias to /src
import Sidebar from '@/components/Sidebar.vue';
import TopNav from '@/components/TopNav.vue';
import axios from 'axios';

export default {
    name: 'CreateColaboradorApp',

    data() {
        return {
            colaborador : {
                rol: ''
            },
        }
    },

    methods: {
        validar(){
            if(!this.colaborador.nombres){
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Ingrese el nombre',
                    type: 'error'
                });
            }else if(!this.colaborador.apellidos){
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Ingrese el apellido',
                    type: 'error'
                });
            }else if(!this.colaborador.email){
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Ingrese el email',
                    type: 'error'
                });
            }else if(!this.colaborador.rol){
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Ingrese el cargo',
                    type: 'error'
                });
            }else{
                this.crear_colaborador();
            }
        },

        crear_colaborador(){
            axios.post(this.$url+'/registro_usuario_admin',this.colaborador,{
                headers: {
                     'Content-Type': 'application/json',
                     'Authorization' : this.$store.state.token
                }
            }).then((result)=>{
                console.log(result);
                if (result.data.data == undefined){
                    this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: result.data.message,
                    type: 'error'
                });
                }else{
                    this.$notify({
                    group: 'foo',
                    title: 'SUCCESS',
                    text: 'Se registro el nuevo colaborador',
                    type: 'success'
                });
                   this.$router.push({name: 'colaborador-index'});
                }
            }).catch((error)=>{
                console.log(error);
            });
        }
    },
    
    mounted() {
        
    },

    components: {
        Sidebar,
        TopNav
    },


}
</script>
