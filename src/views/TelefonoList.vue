<template>

    <ion-page>

        <ion-header>
                <ion-toolbar>
                    <ion-title>Telefonos</ion-title>
                </ion-toolbar>
        </ion-header>

        <ion-content class="ion-padding">
            <ion-list v-for="(phone, i) in phones" :key="i" lines="full">
                <ion-item>
                    <ion-icon :icon="checkmarkCircle" color="success" slot="start" ></ion-icon>
                    <ion-label>
                        {{ phone.name }}

                        <!-- verifica con el metodo hasOwnProperty de javascript si existe en el objeto la propiedad color -->
                        <spam v-if="phone.data && phone.data.hasOwnProperty('color')">
                        ({{ phone.data.color }})
                        </spam>
                    </ion-label>
                </ion-item>
            </ion-list>
        </ion-content>

    </ion-page>
    
</template>

<script>
import {
    IonPage, IonHeader, IonContent, IonToolbar, IonTitle, IonList, IonItem, IonIcon, IonLabel
} from '@ionic/vue';

import { checkmarkCircle } from 'ionicons/icons';

// importar axios
import axios from 'axios';

export default {
    name: 'NuevoTelefono',
    components:{
        IonPage, IonHeader, IonContent, IonToolbar, IonTitle, IonList, IonItem, IonIcon, IonLabel
    },
    data(){
    return{
        checkmarkCircle,
        // arreglo almacenara los arreglos
        phones: []
    }
},
    methods:{
        // este metodo consumera el enpoint y extraera los telefonos registrados
        getTelefonos(){
            axios.get('https://api.restful-api.dev/objects')
            .then(response => {

                //vamos a agregar los datos a mi arreglo phone
                this.phones = response.data;

                console.log(response.data);

            })
            .catch(error => {
                console.log(error);
            })
        }
    },
    mounted(){
        // se ejecute al inicio
        this.getTelefonos()
    }
}

</script>

<style>
</style>