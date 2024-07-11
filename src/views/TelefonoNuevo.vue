<template>
    <ion-page>

        <ion-header>
            <ion-toolbar>
                <ion-title>Inventario</ion-title>
            </ion-toolbar>
        </ion-header>

        <ion-content class="ion-padding">
            <ion-grid>
                <ion-row>
                    <ion-col size="12">
                        <ion-input label-placement="stacked" fill="outline" shape="round" color="success" label="ID" :disabled="true" v-model="id"></ion-input>
                    </ion-col>

                    <ion-col size="12">
                        <ion-input label-placement="stacked" fill="outline" shape="round" color="success" label="Nombre de telefono" v-model="phone.name"></ion-input>
                    </ion-col>

                    <ion-col size="12">
                        <ion-input label-placement="stacked" fill="outline" shape="round" color="success" label="Año" type="number" v-model="phone.data.year"></ion-input>
                    </ion-col>

                    
                    <ion-col size="12">
                        <ion-input label-placement="stacked" fill="outline" shape="round" color="success" label="Precio" type="number" v-model="phone.data.price"></ion-input>
                    </ion-col>

                    <ion-col size="12">
                        <ion-input label-placement="stacked" fill="outline" shape="round" color="success" label="CPU model" v-model="phone.data['CPU model']"></ion-input>
                    </ion-col>

                    <ion-col size="12">
                        <ion-input label-placement="stacked" fill="outline" shape="round" color="success" label="Capacidad Memoria" v-model="phone.data['Hard disk size']"></ion-input>
                    </ion-col>

                    <ion-col size="4">
                        <ion-button shape="round" color="primary" expand="full" @click="agregarTelefono" >
                            Agregar
                        </ion-button>
                    </ion-col>

                    <ion-col size="4">
                        <ion-button shape="round" color="success" expand="full" @click="editarTelefono"  >
                            Editar
                        </ion-button>
                    </ion-col>

                    <ion-col size="4">
                        <ion-button shape="round" color="danger" expand="full" @click="eliminarTelefono">
                            Eliminar
                        </ion-button>
                    </ion-col>

                    
                </ion-row>
            </ion-grid>

            <!-- toast para mensajes -->
            <ion-toast color="dark" :icon="informationOutline" :message="toastMessage" :duration="2000" :isOpen="errorState" @didDismiss="showToast(false)"></ion-toast>
        
        </ion-content>

    </ion-page>
</template>

<script>

import {
    IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonGrid, IonRow, IonCol, IonInput, IonButton, IonToast
} from '@ionic/vue';

import { informationOutline } from 'ionicons/icons';

// importar axios
import axios from 'axios';

export default {
    name: 'NuevoTelefono',
    components:{
        IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonGrid, IonRow, IonCol, IonInput, IonButton, IonToast
    },
    data(){
    return{
        // permite editar y eliminar un telefono
        id:0,
        //este objeto guardara los datos del formulario
        phone:{
            "name":"Iphone 15 pro max",
            "data":{
                "year": 2023,
                "price": 1500,
                "CPU model": "A15 pro",
                "Hard disk size": "1 TB"
            }
        },
        errorState: false,
        //Mensaje del toast
        toastMessage: '',
        // retornar el icono
        informationOutline

    }
    },
    methods:{
        agregarTelefono(){
            // consumir el endpoint para insertar telefono
            axios.post('https://api.restful-api.dev/objects', this.phone)
            .then(response => {
                console.log(response.data);
                this.id = response.data.id;
                this.showToast(true, 'Telefono Agregado');


            })
            .catch(error =>console.log(error))
        },
        editarTelefono(){

            if(this.id !== 0){
            // consumir el endpoint para insertar telefono
            axios.put(`https://api.restful-api.dev/objects/${this.id}`, this.phone)
            .then(response => {
                console.log(response.data);
                this.showToast(true, 'Telefono actualizado');


            })
            .catch(error =>console.log(error))
        }else{
            this.showToast(true, 'El id del telefono no existe');
        }

        },
        eliminarTelefono(){

            
            if(this.id !== 0){
            // consumir el endpoint para insertar telefono
            axios.delete(`https://api.restful-api.dev/objects/${this.id}`)
            .then(response => {
                console.log(response.data);
                this.showToast(true, 'Telefono eliminado');
                this.id=0;


            })
            .catch(error =>console.log(error))
        }else{
            this.showToast(true, 'El id del telefono no existe');
        }

        },
        showToast(state, message){
            this.errorState = state;
            this.toastMessage = message;
        }

    }

}
</script>

<style>
</style>