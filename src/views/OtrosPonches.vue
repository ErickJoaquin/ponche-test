<template>
    <div class="mt-5">
    <b-row>
        <b-alert show variant="success" v-if="success">Registro creado correctamente</b-alert>
        <b-alert show variant="danger" v-if="alert">{{alerta}}</b-alert>
    </b-row>
    <b-row>
        <h1>Otros ponches</h1>
    </b-row>
    <b-row>
        <b-form>
            <div
            class="px-4"
            :class="{ 'hasError': $v.nombreEmpleado.$error }">
                <label>Nombre Empleado</label>
                <select class="custom-select" v-model="nombreEmpleado" @change="seleccionarIndex($event.target.selectedIndex)" >
                    <option v-for="(empleado,index) of empleados" :value="empleado.nombreEmpleado">{{empleado.nombreEmpleado}}</option>
                </select>
            </div>
            <div
            class="px-4"
            :class="{ 'hasError': $v.fechaEntrada.$error }">
                <label>Fecha Entrada</label>
                <b-form-datepicker v-model="fechaEntrada"  class="mb-2"></b-form-datepicker>
            </div>
            <div
            class="px-4"
            :class="{ 'hasError': $v.tipoPonche.$error }">
                <label>Tipo Ponche</label>
                <b-form-select
                    v-model="tipoPonche"
                    :options="tipoPonches"
                    class="mb-3"
                    value-field="tipo"
                    text-field="tipo"
                ></b-form-select>
            </div>
            <div
            class="px-4">
                <b-button variant="outline-primary" @click="ponchar();editarEmpleado()">Ponchar</b-button>
            </div>
        </b-form>
    </b-row>
  </div>
</template>

<script>

import { required, email, minLength } from "vuelidate/lib/validators";


export default {
  name: 'Otros Ponches',
  data(){
      return {
        ponches: [],
        nombreEmpleado: '',
        fechaEntrada:'',
        tipoPonche:'',
        break: '',
        almuerzo: '',
        indexSelected: '',
        tipoPonches: [
            {
                "tipo": "break"
            },
            {
                "tipo": "almuerzo"
            },
            {
                "tipo": "salida"
            }
        ],
        alert:'',
        success:'',
        alerta:'',
        empleados: JSON.parse(localStorage.getItem('empleados'))
    }
  },
  validations: {
      nombreEmpleado: { required },
      fechaEntrada: { required },
      tipoPonche: {required}
  },
    methods:{
        ponchar (){
            this.$v.$touch();
            if(this.$v.$error) return
            const today = new Date();
            const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
            let estado = this.empleados[this.indexSelected].estado;
            if(estado == "Entrada"){
                if(this.tipoPonche == "break"){
                    let breakCount = this.empleados[this.indexSelected].break;
                    if(breakCount == 2){
                        this.alerta = "Ha agotado sus breaks";
                        this.alert = true;
                    }
                    else{
                        this.ponches.push({
                            nombreEmpleado: this.nombreEmpleado,
                            fecha: this.fechaEntrada,
                            hora: time,
                            tipoPonche: this.tipoPonche,
                        });
                        this.success = true;
                        this.empleados[this.indexSelected].break = this.empleados[this.indexSelected].break + 1;
                        this.empleados[this.indexSelected].estado = this.tipoPonche;

                    }
                } 
                else if (this.tipoPonche == "almuerzo"){
                    let almuerzoCount = this.empleados[this.indexSelected].almuerzo;
                    if(almuerzoCount == 1){
                        this.alerta = "Ha agotado su tiempo de almuerzo";
                        this.alert = true;
                    }
                    else{
                        this.ponches.push({
                            nombreEmpleado: this.nombreEmpleado,
                            fecha: this.fechaEntrada,
                            hora: time,
                            tipoPonche: this.tipoPonche,
                        });
                        this.success = true;
                        this.empleados[this.indexSelected].almuerzo = this.empleados[this.indexSelected].almuerzo + 1;
                        this.empleados[this.indexSelected].estado = this.tipoPonche;
                    }
                }
                else if (this.tipoPonche == "salida"){
                    this.ponches.push({
                        nombreEmpleado: this.nombreEmpleado,
                        fecha: this.fechaEntrada,
                        hora: time,
                        tipoPonche: this.tipoPonche,
                    });
                    this.success = true;
                    this.empleados[this.indexSelected].break = 0;
                    this.empleados[this.indexSelected].almuerzo = 0;
                    this.empleados[this.indexSelected].estado = this.tipoPonche;
                }
            }
            else{
                this.alerta = "No puedes hacer este registro antes de ponchar una entrada";
                this.alert = true;
            }
            localStorage.setItem('empleados',JSON.stringify(this.empleados));
            this.nombreEmpleado = '';
            this.fechaEntrada = '';
            this.tipoPonche = '';

            localStorage.setItem('ponches',JSON.stringify(this.ponches));
        },
        seleccionarIndex(selectedIndex){
            this.indexSelected = selectedIndex;
        },
    },
    created : function(){
        let data = JSON.parse(localStorage.getItem('ponches'));
        if(data === null){
            this.ponches = [];
        }
        else{
            this.ponches = data;
            
        }
    }
}
</script>
