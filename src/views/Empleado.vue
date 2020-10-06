<template>
    <div class="mt-5">
        <b-row>
            <b-alert show variant="success" v-if="success">Registro creado correctamente</b-alert>
        </b-row>
        <b-row>
            <h1>Crear Empleado</h1>
        </b-row>
        <b-row>
            <b-form>
                <div
                class="px-4"
                :class="{ 'hasError': $v.nombreEmpleado.$error }">
                    <label>Nombre Empleado</label>
                    <b-form-input v-model="nombreEmpleado"  placeholder="Nombre Empleado"></b-form-input>
                </div>
                <div
                class="px-4">
                    <b-button variant="outline-primary" @click="guardar()">Guardar Empleado</b-button>
                </div>
            </b-form>
        </b-row>
        <b-row class="mt-5">
            <h1>Empleados</h1>
        </b-row>
        <b-row>
            <b-table striped hover :items="empleadosLista"></b-table>
        </b-row>
    </div>
</template>

<script>
import { required, email, minLength } from "vuelidate/lib/validators";
export default {
  name: 'Empleado',
  data(){
      return {
        empleados: [],
        nombreEmpleado: '',
        estado: null,
        empleadosLista: JSON.parse(localStorage.getItem('empleados')),
        success: false
      }
  },
  validations: {
      nombreEmpleado: { required },
  },
  methods:{
      guardar (){
          this.$v.$touch();
          if(this.$v.$error) return
          this.empleados.push({
              nombreEmpleado: this.nombreEmpleado,
              estado: this.estado,
              break: 0,
              almuerzo: 0,
          });
          this.success = true;
          this.nombreEmpleado = '';
          localStorage.setItem('empleados',JSON.stringify(this.empleados));
          this.empleadosLista = this.empleados;
      }
  },
  created : function(){
      let data = JSON.parse(localStorage.getItem('empleados'));
      if(data === null){
          this.empleados = [];
      }
      else{
          this.empleados = data;
      }

      // hora a convertir
var t = "03:19:00";

// creamos una fecha genérica con tu tiempo
var d = new Date("0001-01-01T"+t);

// calculamos los minutos a partir de las horas y minutos de la fecha creada
var minutos = d.getHours() * 60 + d.getMinutes();
console.log(minutos)
  }
}
</script>
