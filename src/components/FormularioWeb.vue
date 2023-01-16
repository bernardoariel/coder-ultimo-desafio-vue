<template>
    <div class="alert alert-danger mt-3" v-if="errores.length >= 1">
        Revise por favor los siguientes inputs
        <ul>
            <li v-for="(item,index) of errores" :key="index">{{ item }}</li>
        </ul>
        
    </div>
   <form ref="form" @submit.prevent="enviarForm">
        <div class="abs-center">
        <div class="card center border-danger mt-5" style="height: 28em">
            <div class="card-body">
            <h4 class="card-title">Ingrese sus datos de Pago:</h4>
            <hr />

            <!-- linea 1 -->
            <div class="row">
                <div class="col-md-2">
                    <label class="float-end">Su nro de tarjeta es:</label>
                </div>    
                <div class="col-md-1">
                   
                    <input type="number" 
                        v-model="nroCredito1"
                        class="form-control"
                        @input="limitDigit($event,4)">
                </div>
                <div class="col-md-1">

                    <input type="number" 
                        v-model="nroCredito2"
                        class="form-control"
                        @input="limitDigit($event,4)">
                </div>
                <div class="col-md-1">
                    
                    <input type="number" 
                        v-model="nroCredito3"
                        class="form-control"
                        @input="limitDigit($event,4)">
                </div>
                <div class="col-md-1">

                    <input type="number" 
                        v-model="nroCredito4"
                        class="form-control"
                        @input="limitDigit($event,4)">
                </div>
                <div class="col-md-4">
                    <img
                    src="../assets/tarjeta1.png"
                    alt="tarjeta de credito"
                    width="50"
                    class="rounded"
                /></div>
            </div>


            <!-- linea 2 -->
            <div class="row mt-2">
                <div class="col-md-3 col-sm-3 col-xs-3">
                <span class="help-block text-muted small-font"
                    ><small>Exp.Mes</small></span
                >
                <input
                    v-model="expMes"
                    type="number"
                    class="form-control"
                    min="1"
                    max="12"
                />
                </div>
                <div class="col-md-3 col-sm-3 col-xs-3">
                <span class="help-block text-muted small-font"
                    ><small>Exp.Año</small></span
                >
                <input
                    v-model="expAnio"
                    type="number"
                    min="2022"
                    class="form-control"
                    placeholder="YY"
                />
                </div>
                <div class="col-md-3 col-sm-3 col-xs-3">
                <span class="help-block text-muted small-font"
                    ><small>CCV</small></span
                >
                <input
                    v-model="ccv"
                    type="text"
                    class="form-control"
                    placeholder="CCV"
                    
                />
                </div>
                <div class="col-md-1">
                <span class="help-block text-muted small-font"
                    ><small>-</small></span
                >
               
                </div>
            </div>
            <!-- linea 3 -->
            <div class="row mt-2">
                <div class="col-md-12">
                <span class="help-block text-muted small-font"
                    ><small>Nombre como figura en la Tarjeta</small></span
                >
                <input
                    v-model="nombreTarjeta"
                    type="text"
                    class="form-control"
                    placeholder="Nombre como en la tarjeta"
                />
                </div>
            </div>
            <!-- linea  -->
            <div class="row mt-2">
                <div class="col-md-12">
                <span class="help-block text-muted small-font"
                    ><small>Ingrese su Email para enviar comprobante</small></span
                >
                <input
                    v-model="emailCliente"
                    type="email"
                    class="form-control"
                    placeholder="Ingrese un email"
                />
                </div>
            </div>

            <!-- linea 4 -->
            <div class="row mt-2">
                <div class="col">
                    <input
                        type="button"
                        class="btn btn-secondary btn-block float-start"
                        value="Limpiar"
                        @click="limpiarForm()"
                    />
                    <input
                        type="submit"
                        class="btn btn-warning btn-block float-end"
                        value="Pagar Ahora"
                        :disabled="desabilitar"
                    />
                </div>
            </div>
            </div>
        </div>
        </div>
    </form>
    <TablaComponent :pagos="pago"/>
</template>
<script lang="ts">
import{ ref } from 'vue'
import TablaComponent from '@/components/TablaComponent.vue'

export default {
    components: { TablaComponent},
  setup() {
    const nroCredito1 = ref("");
    const nroCredito2 = ref("");
    const nroCredito3 = ref("");
    const nroCredito4 = ref("");
    const nroCredito = ref("");
    const expMes = ref(1);
    const expAnio = ref("2022");
    const ccv = ref("");
    const nombreTarjeta = ref("");
    const emailCliente = ref("");
    const miPago = ref({});
    const desabilitar = ref(false);
    const errores = ref<string[]>([]);
    const pago = ref<any[]>([]);

    let limpiarForm = () =>{
      nroCredito.value = "";
      nroCredito1.value=""
    nroCredito2.value=""
    nroCredito3.value=""
    nroCredito4.value=""
    nroCredito.value=""
      expMes.value = 1;
      expAnio.value = "2022";
      ccv.value = "";
      nombreTarjeta.value = "";
      emailCliente.value = "";
      miPago.value = {};
      errores.value = [];
    }

    let validarForm = () => {
        if (!nroCredito1.value) {
            errores.value = [...errores.value,"Ingrese un número de tarjeta de crédito válido (1)"];
        }
        if (!nroCredito2.value) {
            errores.value = [...errores.value,"Ingrese un número de tarjeta de crédito válido (2)"];
        }
        if (!nroCredito3.value) {
            errores.value = [...errores.value,"Ingrese un número de tarjeta de crédito válido (3)"];
        }
        if (!nroCredito4.value) {
            errores.value = [...errores.value,"Ingrese un número de tarjeta de crédito válido (4)"];
        }
        if (!expMes.value || expMes.value < 1 || expMes.value > 12) {
            errores.value = [...errores.value,"Ingrese un mes de expiración válido"];
        }
        if (!expAnio.value) {
            errores.value = [...errores.value,"Ingrese un año de expiración válido"];
        }
        if (!ccv.value) {
            errores.value = [...errores.value,"Ingrese un código de seguridad válido"];
        }
        if (!nombreTarjeta.value) {
            errores.value = [...errores.value,"Ingrese un nombre como figura en la tarjeta"];
        }
        if (!emailCliente.value) {
            errores.value = [...errores.value,"Ingrese un email válido para enviar el comprobante"];
        }
        
}

    function enviarForm() {
      errores.value =[]
      validarForm();
      if (errores.value.length === 0) {
        // Aquí se podría incluir la lógica para enviar el formulario
        // y realizar el pago
        let importe = Math.round(Math.random() * (50000 - 1500) + 1500);
        nroCredito.value = `${nroCredito1.value} ${nroCredito2.value} ${nroCredito2.value} ${nroCredito4.value}`
        console.log(nroCredito)

        miPago.value = {
          nroCredito: nroCredito.value,
          expMes: expMes.value,
          expAnio: expAnio.value,
          ccv: ccv.value,
          nombreTarjeta: nombreTarjeta.value,
          importe,
        };
        pago.value.push(miPago.value)
        limpiarForm()
      }
    }

    function limitDigit(event,maxDigit){
    if(event.target.value.toString().length>maxDigit)
        event.target.value = event.target.value.toString().substring(0, maxDigit)
    }

   

    return {
      nroCredito,
      expMes,
      expAnio,
      ccv,
      nombreTarjeta,
      emailCliente,
      miPago,
      desabilitar,
      errores,
      limpiarForm,
      enviarForm,
      pago,
      nroCredito1,
      nroCredito2,
      nroCredito3,
      nroCredito4,
     limitDigit
    };
  }
};
   
</script>


<style scoped>

</style>