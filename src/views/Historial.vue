<template>
    <div class="mt-5">
        <b-row>
            <h1>Historial de ponches</h1>
        </b-row>
        <b-row>
            <b-table striped hover :items="ponches"></b-table>
        </b-row>
    </div>
</template>

<script>
import { required, email, minLength } from "vuelidate/lib/validators";

export default {
  name: 'Historial',
  data(){
      return {
        ponches: JSON.parse(localStorage.getItem('ponches'))

      }
  }
}
</script>
