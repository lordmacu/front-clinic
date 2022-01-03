<template>
  <div>
  <div v-if="loaded" class="home" style="background-color:rgba(250,250,250,1); min-height:100vh;">
    <TopNavD  />

    <div v-if="estado == 4" style="padding-top:140px;">
      <div><h2>¡Pago realizado!</h2></div> 
      <b-icon style="height:200px; width:200px; color:green;" icon="check-circle"/>
      <div>¡El pago se realizo con éxito! Pronto estaremos en contacto contigo.</div>
      <router-link class="btn btn-success" :to="{name:'Home'}"> Volver</router-link>
    
    </div>
    <div v-if="estado == 1" style="padding-top:140px;">

      <img style="max-height:250px;" src="@/assets/svg/moto.svg" alt="">
      <div><h4 style="text-align:center; font-weight:700; color:rgb(20, 134, 231); margin-bottom:30px; margin-top:30px;">¡Orden realizada!</h4></div>       
      <div style="margin-top:20px; margin: 0 auto; max-width:600px; margin-bottom:10px;">Te enviamos un email con la información de la orden. Recuerda que deberás hacer el pago antes de recibir los resultados.</div>
      <router-link style="background-color: rgb(254, 177, 53); border: 1px solid rgb(254, 177, 53); margin-top:20px; padding:12px 10px;" class="btn btn-success btn-bien" :to="{name:'Home'}"> Ir al inicio</router-link>
    
    </div>
    <div v-if="estado == 10" style="padding-top:140px;">
      <div><h2>¡Pago no realizado!</h2></div> 
      <b-icon style="height:200px; width:200px; color:red;" icon="x-circle"/>
      <div>¡Ocurrió un error en la transacción! Por favor, intente nuevamente.</div>
      <router-link :to="{name:'Home'}"> Volver</router-link>
    
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios'
import TopNavD from '@/components/TopNavDFijo.vue'
import {mapGetters, mapActions} from 'vuex';

export default {
  name: 'Home',
  props:['id'],
  components: {
    TopNavD,
  },
  data() {
    return {
      estado:-1,
      loaded:false,

    }
  },
  created() {
    
    
        axios.post(window.hostname+'api/orden_get_estado',
        {
         orden_id:this.$route.params.id 
        })
        .then(r=>{
          this.loaded = true
          this.estado = r.data.estado
          if (this.estado == 1 || this.estado == 4)
            this.vaciarCarroAction()
        })
        .catch(e=>{
          this.loaded = true
          console.log(e)
            
        })

  },
  destroyed() {

  },
  methods: {
    ...mapActions(['clickearExamenAction', 'vaciarCarroAction']),

  },
  computed: {
    ...mapGetters(['isLoggedIn','usuario_datos', 'comuna', 'comunas', 'examenes_agregados'])

  },
}
</script>
<style scoped>


.btn-volver{
  border: 2px solid rgb(254, 177, 53);
  color: rgb(254, 177, 53);
}
.btn-volver:hover{
  border: 2px solid rgb(254, 177, 53);
  color: rgb(254, 177, 53);
}
.btn-bien{
background-color: rgb(254, 177, 53);
color:white;

}
.btn-bien:hover{
color:white;

}
.btn-mal{
  background-color: rgb(255,222,166);
  color:white;
}
.btn-mal:hover{
  color:white;
  
}
.glow {
  color: #fff;
  -webkit-animation: glow 1s ease-in-out infinite alternate;
  -moz-animation: glow 1s ease-in-out infinite alternate;
  animation: glow 1s ease-in-out infinite alternate;
  animation-iteration-count: 5;
}

@keyframes glow {
  from {
    box-shadow: 0 0 20px rgb(254, 177, 53), 0 0 20px rgb(254, 177, 53), 0 0 30px rgb(254, 177, 53), 0 0 40px rgb(254, 177, 53), 0 0 50px rgb(254, 177, 53), 0 0 60px rgb(254, 177, 53), 0 0 70px rgb(254, 177, 53);
  }
  to {
    box-shadow: 0 0 10px rgb(254, 177, 53), 0 0 30px rgb(254, 177, 53), 0 0 40px rgb(254, 177, 53) 0 0 50px rgb(254, 177, 53), 0 0 60px rgb(254, 177, 53), 0 0 70px rgb(254, 177, 53), 0 0 80px rgb(254, 177, 53);
  }
}

.top-spacer{
  height:130px;
}
.bottom-spacer{
  height:140px;
}
@media only screen and (max-width: 1200px) {
  .top-spacer{
  height:85px;
}
.bottom-spacer{
  height:60px;
}
}
/* width */
::-webkit-scrollbar {
  width: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  background: #f1f1f1; 
}
 
/* Handle */
::-webkit-scrollbar-thumb {
  background: #888; 
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #555; 
}
.tyc-container{
  margin-top:15px; position:absolute; right:270px; display:block; text-align:right;
}
.tyc-container{
 right:270px;
}
@media only screen and (max-width: 992px) {
.tyc-container{
  margin-top:15px; 
  display:block; 
  text-align:right;
  position:static;
}
.desktop{
  display:none;
}
}

@media (min-width: 576px) { 
.btn-volver{
  width:180px !important;
}
.btn-bien{
  width:180px !important;
}
.btn-mal{
  width:180px !important;
}
}


@media (min-width: 768px) { 
.btn-volver{
  width:240px !important;
}
.btn-bien{
  width:240px !important;
}
.btn-mal{
  width:240px !important;
}
 }


@media (min-width: 992px) { 
.btn-volver{
  width:220px !important;
}
.btn-bien{
  width:220px !important;
}
.btn-mal{
  width:220px !important;
}
.tyc-container{
 right:250px;
}
 }


@media (min-width: 1200px) {
.btn-volver{
  width:240px !important;
}
.btn-bien{
  width:240px !important;
}
.btn-mal{
  width:240px !important;
}
.tyc-container{
 right:260px;
}
}


@media (min-width: 1400px) { 
.btn-volver{
  width:240px !important;
}
.btn-bien{
  width:240px !important;
}
.btn-mal{
  width:240px !important;
}
}
</style>