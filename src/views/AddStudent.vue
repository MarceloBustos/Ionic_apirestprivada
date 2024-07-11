<template>
    <ion-page>
        <ion-header>
            <ion-toolbar color="primary">
                <ion-title>Estudiantes</ion-title>
            </ion-toolbar>
        </ion-header>
        <ion-content class="ion-padding">
            <ion-grid>
                <ion-row>
                    <ion-col>
                        <ion-input label="Nombre" color="warning" label-placement="stacked" maxlenght="30" counter
                            fill="outline" shape="round" clear-input="true" placeholder="Nombres y Apellidos"
                            v-model="estudiante.nombre" required>
                        </ion-input>
                        <br>
                        <ion-input label="Edad" color="warning" label-placement="stacked" fill="outline" shape="round"
                            clear-input="true" placeholder="Edad" type="number" v-model="estudiante.edad">
                        </ion-input>
                        <br>
                        <ion-input label="Correo electrónico" color="warning" label-placement="stacked" fill="outline"
                            shape="round" clear-input="true" placeholder="Correo Electrónico"
                            v-model="estudiante.correo">
                        </ion-input>
                        <br>
                        <ion-input label="Teléfono" color="warning" label-placement="stacked" maxlenght="9" counter
                            fill="outline" shape="round" clear-input="true" placeholder="Número de Teléfono"
                            v-model="estudiante.telefono">
                        </ion-input>
                        <br><br>
                        <ion-button shape="round" color="warning" expand="block" @click="addStudent"><ion-icon
                                slot="start" :icon="checkmarkOutline"></ion-icon>Guardar</ion-button>
                    </ion-col>
                </ion-row>
            </ion-grid>
            <ion-toast :duration="2500" :message="toastMessage" :is-open="toastState" @didDismiss="toastState = false"
                :icon="informationCircleOutline">

            </ion-toast>
        </ion-content>
    </ion-page>
</template>

<script>
import axios from 'axios'
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonGrid, IonRow, IonCol, IonInput, IonButton, IonIcon, IonToast } from '@ionic/vue';
import { checkmarkOutline, informationCircleOutline } from 'ionicons/icons';
export default {
    name: 'AddStudent',
    components: {
        IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonGrid, IonRow, IonCol, IonInput, IonButton, IonIcon, IonToast
    },
    data() {
        return {
            checkmarkOutline,
            informationCircleOutline,

            //Variable para guardar el contenido del formulario
            estudiante: {},

            //Variable para controlar la visibilidad del toast
            toastState: false,

            //Variable para guardar el mensaje
            toastMessage: null
        }
    },
    methods: {
        //Función para guardar el estudiante
        addStudent() {
            //Peticion par ainsertar datos
            axios.post('http://127.0.0.1:8000/api/estudiante/store', this.estudiante)
                .then(response => {
                    let res = response.data
                    this.estudiante = {}

                    if (res.code == 200) {
                        this.toastState = true
                        this.toastMessage = res.data
                    }

                })
                .catch(error => console.log('Ha ocurrido un error' + error))
        }
    }
}
</script>

<style></style>