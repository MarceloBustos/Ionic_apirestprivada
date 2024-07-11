<template>
    <ion-page>
        <ion-header>
            <ion-toolbar>
                <ion-title>
                    Estudiante
                </ion-title>
            </ion-toolbar>
        </ion-header>

        <ion-content class="ion-padding">
            <ion-grid>
                <ion-row>
                    <ion-col>
                        <ion-card>
                            <ion-card-header>
                                <ion-card-title>
                                    Estudiantes
                                </ion-card-title>

                                <ion-card-subtitle>
                                    Listado de Estudiantes
                                </ion-card-subtitle>
                            </ion-card-header>

                            <ion-card-content>
                                <ion-list>
                                    <template v-for="(estudiante, i) in respuesta" :key="i">
                                        <ion-item-sliding>
                                            <ion-item :button="true">
                                                <ion-icon slot="start" :icon="personCircle"></ion-icon>
                                                <ion-label>{{ estudiante.nombre }}</ion-label>

                                                <ion-buttons slot="end">
                                                    <ion-button color="tertiary" @click="showStudent(estudiante.id, 1)">
                                                        <ion-icon slot="icon-only" :icon="eye"></ion-icon>
                                                    </ion-button>
                                                    <ion-button color="success" @click="showStudent(estudiante.id, 2)">
                                                        <ion-icon slot="icon-only" :icon="create"></ion-icon>
                                                    </ion-button>
                                                    <ion-button color="danger"
                                                        @click="validardeleteStudent(estudiante.id)">
                                                        <ion-icon slot="icon-only" :icon="trash"></ion-icon>
                                                    </ion-button>

                                                </ion-buttons>
                                            </ion-item>
                                        </ion-item-sliding>
                                    </template>
                                    <!-- Mostrar un mensaje en caso de no tener estudiantes -->
                                    <ion-item v-if="respuesta.length === 0">
                                        <ion-icon slot="start" :icon="closeCircle"></ion-icon>
                                        <ion-label>No hay estudiantes</ion-label>
                                    </ion-item>
                                </ion-list>
                            </ion-card-content>
                        </ion-card>
                    </ion-col>
                </ion-row>
            </ion-grid>


            <ion-alert :is-open="alertState" @didDismiss="alertState = false" header="Eliminar"
                message="¿Estás seguro que deseas eliminar este estudiante?" :buttons="alertButtons"></ion-alert>

            <!-- Modal para datos de estudiante -->
            <ion-modal :is-open="modalState" @didDismiss="modalState = false">
                <ion-header>
                    <ion-toolbar>
                        <ion-title>Estudiante</ion-title>
                        <ion-buttons slot="end">
                            <ion-button color="danger" @click="modalState = false"><ion-icon slot="icon-only"
                                    :icon="closeCircle"></ion-icon>
                            </ion-button>
                        </ion-buttons>
                    </ion-toolbar>
                </ion-header>

                <ion-content class="ion-padding">
                    <ion-list>
                        <ion-item>
                            <ion-icon slot="start" :icon="personCircle"></ion-icon>
                            <ion-label>{{ estudiante.nombre }}</ion-label>
                        </ion-item>

                        <ion-item>
                            <ion-icon slot="start" :icon="checkmark"></ion-icon>
                            <ion-label>{{ estudiante.edad }}</ion-label>
                        </ion-item>

                        <ion-item>
                            <ion-icon slot="start" :icon="mail"></ion-icon>
                            <ion-label>{{ estudiante.correo }}</ion-label>
                        </ion-item>

                        <ion-item>
                            <ion-icon slot="start" :icon="call"></ion-icon>
                            <ion-label>{{ estudiante.telefono }}</ion-label>
                        </ion-item>
                    </ion-list>
                </ion-content>
            </ion-modal>

            <!-- Modal para editar -->
            <ion-modal :is-open="modalState2" @didDismiss="modalState2 = false">
                <ion-header>
                    <ion-toolbar>
                        <ion-title>Estudiante</ion-title>
                        <ion-buttons slot="end">
                            <ion-button color="danger" @click="modalState2 = false"><ion-icon slot="icon-only"
                                    :icon="closeCircle"></ion-icon>
                            </ion-button>
                        </ion-buttons>
                    </ion-toolbar>
                </ion-header>

                <ion-content class="ion-padding">
                    <ion-input color="warning" label-placement="stacket" maxlength="30" counter fill="outline"
                        shape="round" clear-input="true" placeholder="Nombres y Apellidos"
                        v-model="estudiante.nombre"></ion-input><br>

                    <ion-input color="warning" label-placement="stacket" type="number" fill="outline" shape="round"
                        clear-input="true" placeholder="Edad" v-model="estudiante.edad"></ion-input><br>

                    <ion-input color="warning" label-placement="stacket" maxlength="30" counter fill="outline"
                        shape="round" clear-input="true" placeholder="Correo"
                        v-model="estudiante.correo"></ion-input><br>

                    <ion-input color="warning" label-placement="stacket" maxlength="9" counter fill="outline"
                        shape="round" clear-input="true" placeholder="Número de Teléfono"
                        v-model="estudiante.telefono"></ion-input><br>

                    <ion-button shape="round" color="warning" expand="block" @click="editStudent(estudiante.id)">
                        <ion-icon slot="start" :icon="checkmarkOutline"></ion-icon>
                        Guardar cambios
                    </ion-button>
                </ion-content>
            </ion-modal>
            <ion-toast :duration="2500" :message="toastMessage" :is-open="toastState" @didDismiss="toastState = false"
                :icon="informationCircleOutline">

            </ion-toast>
        </ion-content>
    </ion-page>
</template>

<script>
import axios from 'axios'
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonGrid, IonRow, IonCol, IonCard, IonCardHeader, IonCardTitle, IonCardSubtitle, IonCardContent, IonList, IonItemSliding, IonItem, IonIcon, IonLabel, IonItemOptions, IonItemOption, IonModal, IonButtons, IonButton, IonInput, IonToast, IonAlert } from '@ionic/vue'



import { personCircle, eye, create, trash, closeCircle, call, mail, checkmark, checkmarkOutline, informationCircleOutline } from 'ionicons/icons'
export default {
    name: 'AddStudent',
    components: {
        IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonGrid, IonRow, IonCol, IonCard, IonCardHeader, IonCardTitle, IonCardSubtitle, IonCardContent, IonList, IonItemSliding, IonItem, IonIcon, IonLabel, IonItemOptions, IonItemOption, IonModal, IonButtons, IonButton, IonInput, IonToast, IonAlert
    },
    data() {
        return {
            personCircle, eye, create, trash, closeCircle, call, mail, checkmark, checkmarkOutline, informationCircleOutline,

            //Variable para guardar los estuduantes registrados
            respuesta: [],

            //Variable para manejar el modal
            modalState: false,
            modalState2: false,
            toastState: false,
            toastMessage: null,

            alertState: false,

            alertButtons: [
                {
                    text: 'No, Déjalo así!',
                    role: 'cancel',
                    handler: () => {
                        this.alertState = false
                        console.log("Canceló el pastel")
                    }
                },
                {
                    text: 'Si, Eliminar!',
                    role: 'confirm',
                    handler: () => {
                        this.alertState = false
                        this.deleteStudent(this.estudiante)

                        console.log("Eliminó el pastel")

                    }
                },
            ],

            //Variable para guardar los datos del estudiante
            estudiante: {}
        }
    },
    methods: {
        loadData() {
            this.respuesta = []
            axios.get('http://127.0.0.1:8000/api/estudiante/select')
                .then(response => {
                    let res = response.data

                    if (res.code == 200) {
                        this.respuesta = res.data
                    }
                    else {
                        this.respuesta = ''
                    }
                })
                .catch(error => console.log('Ha ocurrido un error ' + error))
        },
        showStudent(id, action) {
            axios.get(`http://127.0.0.1:8000/api/estudiante/find/${id}`)
                .then(response => {
                    let res = response.data
                    if (res.code == 200) {
                        this.estudiante = res.data
                    }
                })
                .catch(error => console.log('Ha ocurrido un error ' + error))

            //Evaluar para mostrar el modal
            if (action == 1) {
                this.modalState = true
            }
            else {
                this.modalState2 = true
            }
        },
        editStudent(id) {
            axios.put(`http://127.0.0.1:8000/api/estudiante/update/${id}`, this.estudiante)
                .then(response => {
                    let res = response.data

                    if (res.code == 200) {
                        this.toastState = true
                        this.toastMessage = res.data
                        this.modalState2 = false
                        this.loadData()
                    }
                })
                .catch(error => console.log('Ha ocurrido un error ' + error))
        },
        validardeleteStudent(id) {

            console.log(id)
            this.alertState = true
            this.estudiante = id

        },
        deleteStudent(id) {
            axios.delete(`http://127.0.0.1:8000/api/estudiante/delete/${id}`)
                .then(reponse => {
                    let res = reponse.data

                    if (res.code == 200) {
                        this.toastState = true
                        this.toastMessage = res.data
                        this.loadData()
                    }
                })
                .catch(error => console.log('Ha ocurrido un error ' + error))
        }
    },
    //Se ejecuta cuando el componente esta a punto de mostrarse
    ionViewWillEnter() {
        this.loadData()
    }
}



</script>

<style>
ion-alert.custom-alert {
    --backdrop-opacity: 0.7;
}

.custom-alert .alert-button-group {
    padding: 8px;
}

button.alert-button.alert-button-confirm {
    background-color: var(--ion-color-success);
    color: var(--ion-color-success-contrast);
}

.md button.alert-button.alert-button-confirm {
    border-radius: 4px;
}

.ios .custom-alert button.alert-button {
    border: 0.55px solid rgba(var(--ion-text-color-rgb, 0, 0, 0), 0.2);
}

.ios button.alert-button.alert-button-cancel {
    border-right: 0;
    border-bottom-left-radius: 13px;
    border-top-left-radius: 13px;
}

.ios button.alert-button.alert-button-confirm {
    border-bottom-right-radius: 13px;
    border-top-right-radius: 13px;
}
</style>