<template>
  <div class="mt-5">
    <b-row>
        <b-alert show variant="success" v-if="success">Registro creado correctamente</b-alert>
        <b-alert show variant="danger" v-if="alert">{{alerta}}</b-alert>
    </b-row>
    <b-row>
        <h1>Ponchar Entrada</h1>
    </b-row>
    <b-row>
        <b-form>
            <div
            class="px-4"
            :class="{ 'hasError': $v.nombreEmpleado.$error }">
                <label>Empleado</label>
                <select class="custom-select" v-model="nombreEmpleado"  @change="seleccionarIndex($event.target.selectedIndex)" required>
                    <option v-for="(empleado,index) of empleados" :value="empleado.nombreEmpleado">{{empleado.nombreEmpleado}}</option>
                </select>
            </div>
            <div
            class="px-4"
            :class="{ 'hasError': $v.fechaEntrada.$error }">
                <label>Fecha</label>
                <b-form-datepicker v-model="fechaEntrada"  class="mb-2"></b-form-datepicker>
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
  name: 'Ponchar Entrada',
  data(){
      return {
        ponches: [],
        nombreEmpleado: '',
        fechaEntrada:'',
        tipoPonche: 'entrada',
        empleados: JSON.parse(localStorage.getItem('empleados')),
        indexSelected: '',
        alerta:'',
        success: false,
        alert:false,

      }
  },
  validations: {
      nombreEmpleado: { required },
      fechaEntrada: { required }
  },

  methods:{
      ponchar (){
          this.$v.$touch();
          if(this.$v.$error) return
          const today = new Date();
          const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
          let estado = this.empleados[this.indexSelected].estado;
          if(estado != "Entrada"){
              this.ponches.push({
                nombreEmpleado: this.nombreEmpleado,
                fecha: this.fechaEntrada,
                hora: time,
                tipoPonche: this.tipoPonche
              });
              localStorage.setItem('ponches',JSON.stringify(this.ponches));
              this.success = true;
          }
          else{
              this.alerta = "No puede registrar una entrada en este momento";
              this.alert = true;
          }
          
          this.nombreEmpleado = '';
          this.fechaEntrada = '';
          
      },
      seleccionarIndex(selectedIndex){
          this.indexSelected = selectedIndex;
      },
      editarEmpleado : function(){
          this.$v.$touch();
          if(this.$v.$error) return
          this.empleados[this.indexSelected].estado = "Entrada"
          localStorage.setItem('empleados',JSON.stringify(this.empleados));
      }
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
