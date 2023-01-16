<template>
    <div>
        <table class="table mt-5" >
            <thead class="bg-dark text-white">
                <tr>
                    <td>Nro.Tarjeta</td>
                    <td>Expira</td>
                    <td>Nombre</td>
                    <td>Codigo de Seguridad</td>
                    <td>Importe</td>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item, index) of pagos" :key="index">
                    <td>{{ item.nroCredito }}</td>
                    <td>{{ item.expMes }}/{{ item.expAnio }}</td>
                    <td>{{ item.nombreTarjeta }}</td>
                    <td>{{ item.ccv }}</td>
                    <td>$ {{ item.importe }} .-</td>
                </tr>
            </tbody>
        </table>
    </div>

</template>

<script lang="ts">

interface Pago {
    nroCredito: string,
    expMes: string,
    expAnio: string,
    ccv: string,
    nombreTarjeta: string,
    importe: number,
}


export default {
    props: {
        pagos: {
            type: Array as () => Pago[],
            required: true
        }
    },
    
}
</script>

<style lang="scss" scoped>

</style>