<template>
  <div class="home" >
    <TopNavD  />

<div style="margin-top:80px;">

  <h1 style="margin-bottom:10px; padding-top:60px;">Órdenes Médicas</h1>
  
  <div class="outer-container">


    <div v-for="p, i in orden_final" style="box-shadow: rgba(0, 0, 0, 0.05) 0px 6px 24px 0px, rgba(0, 0, 0, 0.08) 0px 0px 0px 1px; border-radius:15px; margin-top:15px; ">
      <div class="inner-container">
        <div style="padding: 5px 0px; margin-top:0px; position:relative;" >
                  <b-icon class="icon-excl" v-if="p.examenes.some(x=>{ return x.requiere_orden == 1}) && p.orden_medica == null" icon="exclamation-triangle"></b-icon> 
                  <b-icon class="icon-check" v-else icon="check"></b-icon>
                  <div class="barrita-separadora"></div>
     <div class="name-container">
        {{p.nombre}} {{p.apellido1}}
        </div>
      </div>
      </div>
      <div style="display:inline-block; position:relative; width:160px; height:60px; vertical-align:middle;">
        <div style="" v-if="p.examenes.some(x=>{ return x.requiere_orden == 1})">
          <!--
            <input v-on:change="change_file(i)" :id="'file'+i" :ref="'file'+i"  type="file" accept="image/*,application/pdf" style="visibility:hidden; width:0px;" />
            -->
            <input v-on:change="change_file(i)" :id="'file'+i" :ref="'file'+i"  type="file" accept="image/*, application/pdf" style="visibility:hidden; width:0px;" />
            <label class="btn-receta"  :for="'file'+i">Subir receta <b-icon icon="camera"></b-icon></label>
        </div>
        <div style="cursor: not-allowed; position:absolute; border-radius:5px; color:rgba(0,0,0, 0.4); top:13px; left:0px; padding:5px 40px; background-color:rgb(241,241,241);" v-else>
            No aplica
        </div>
      </div>

    </div>
  
    </div>
    <!--

  <label class="btn-subir-foto" style="margin-bottom:40px;" for="files">Subir orden médica <b-icon icon="camera"></b-icon></label>
  <div v-if="receta_medica != null" style="position:absolute; top:48px; left:18px;">{{receta_medica.name}} <b-icon icon="check"></b-icon></div>
<input v-on:change="change_file()" id="files" ref="file"  type="file" accept="image/*" style="visibility:hidden; width:0px;" />
-->
<div  style=" margin-top:30px;" v-on:click="saltar()" class="btn-subir-foto">Saltar por ahora <b-icon icon="arrow-right"></b-icon></div>

</div>


      <div v-if="window.height >= 400" style="position:fixed; left:0px; height:80px; bottom:0px; width:100vw; background:white; border-top:1px solid rgb(214,214,214); ">
          <div style="">
          <div class="container" style="position:relative">
            <div style="display:inline-block; margin-top:30px; width:100%; padding-left:68px; padding-right:90px; text-align:center;">
                Total orden: <span style="color: rgb(20, 134, 231); font-weight:700; "> ${{Intl.NumberFormat('de-DE').format(examenes_agregados.reduce((x, y)=>{return x+y.precio*y.cantidad}, 0) + costo_servicio+(n_personas*costo_servicio_pendiente))}} </span>
            </div>
          <button @mouseleave="alerta = false" @mouseenter="alerta = true" v-on:click="continuar()" style="font-size:14px;  position:absolute; right:10px; top:25px; padding:7px 4px;" :class="{'btn-mal': !puedeContinuar, 'btn-bien':puedeContinuar, 'glow':puedeContinuar}" class="btn">Resumen <b-icon icon="arrow-right-circle"/></button>
          <router-link :to="{name:'DatosPersonales'}" tag="button" style="font-size:14px; position:absolute; left:0px; top:25px; width:70px; padding:7px 0px;" class="btn btn-volver"><b-icon  icon="arrow-left-circle"/> Volver</router-link>
        </div>
        </div>
      </div>

      <div v-else style=" left:0px; height:80px; bottom:0px; background:white; border-top:1px solid rgb(214,214,214); ">
          <div style="">
          <div  style="position:relative">
            <div style="display:inline-block; margin-top:30px; width:100%; padding-left:68px; padding-right:90px; text-align:center;">
                Total orden: <span style="color: rgb(20, 134, 231); font-weight:700; "> ${{Intl.NumberFormat('de-DE').format(examenes_agregados.reduce((x, y)=>{return x+y.precio*y.cantidad}, 0) + costo_servicio+(n_personas*costo_servicio_pendiente))}} </span>
            </div>
          <button @mouseleave="alerta = false" @mouseenter="alerta = true" v-on:click="continuar()" style="font-size:14px;  position:absolute; right:10px; top:25px; padding:7px 4px;" :class="{'btn-mal': !puedeContinuar, 'btn-bien':puedeContinuar, 'glow':puedeContinuar}" class="btn">Resumen <b-icon  icon="arrow-right-circle"/></button>
          <router-link :to="{name:'DatosPersonales'}" tag="button" style="font-size:14px; position:absolute; left:0px; top:25px; width:70px; padding:7px 0px;" class="btn btn-volver"><b-icon  icon="arrow-left-circle"/> Volver</router-link>
        </div>
        </div>
      </div>

<div v-if="error_archivo" v-on:click="error_archivo = false" style="height:100vh; width:100vw; position:fixed; top: 0; left: 0; background-color:rgba(0,0,0,0.4); z-index:99998"></div>
    <div v-if="error_archivo" v-on:click="error_archivo = false"  class="comuna_error move">
      <img src="@/assets/icon/file-error.svg" style="height:30px; width:30px; margin-bottom:10px; fill:#FF4136;" alt="">

      <div  v-on:click="error_archivo = false"  style="color:#FF4136;"> Archivo en <b>formato equivocado</b>. Por favor, suba un archivo en png, jpg, jpeg o pdf.</div>
    </div>



  </div>

</template>

<script>
import axios from 'axios'
import TopNavD from '@/components/TopNavDFijo.vue'
import {mapGetters, mapActions} from 'vuex';

export default {
  name: 'Home',
  components: {
    TopNavD,

  },
  data() {
    return {
      viendoTerminos:false,
      viendoPoliticas:false,
      aceptoTerminos:false,
      error_archivo:false,
      loading_transferencia:false,
      loading_transbank:false,
      window: {
          width: 0,
          height: 0
      },
    
    }
    
  },
  created() {
    window.addEventListener('resize', this.handleResize);
    this.handleResize();

    if(this.examenes_agregados.length == 0 || this.comuna == -1)
      this.$router.push({ name: 'Home' })
    if(this.inicio == -1 || this.termino == -1 || this.dia == -1 || this.mes == -1 || this.año == -1)
      this.$router.push({ name: 'FechaYHora' })
    //if(this.nombre == '' || this.apellido1 == '' || this.rut == '' || this.email == '' || this.telefono == '')

    for (var i = 0; i<this.orden_final.length; i++){
        if (this.orden_final[i].examenes.length > 0)
        {

          if(!(

          this.orden_final[i].nombre.length > 1 &&
          this.orden_final[i].rut.length>7 && 
          this.orden_final[i].email.length > 2 && 
          this.orden_final[i].apellido1.length > 1 &&
          this.orden_final[i].telefono.length > 6 &&
          this.verificar_correo(this.orden_final[i].email) &&
          this.verificar_rut(this.orden_final[i].rut)
          
          ))
          {
            this.$router.push({ name: 'DatosPersonales' })
            break
          }
          if(this.orden_final[i].email.length != '')
            mails.push(this.orden_final[i].email)
        }
      }
      if (this.direccion.length < 3 || this.examenes_agregados.length == 0 || !(mails.length === new Set(mails).size))
        this.$router.push({ name: 'DatosPersonales' })




  },
  beforeDestroy () {
        window.removeEventListener('resize', this.handleResize);
  },
  methods: {
    ...mapActions(['clickearExamenAction', 'cambiarCantidadAction', 'cambiarCantidadPersonasAction', 'setRecetaMedicaAction']),
    handleResize() {
        this.window.width = window.innerWidth;
        this.window.height = window.innerHeight;
    },
  change_file(i){
    //console.log(this.$refs['file'+i][0].files[0].name, "ASDASD")
    //console.log(this.$refs)
    if (['jpg', 'png', 'jpeg', 'pdf'].includes(this.$refs['file'+i][0].files[0].name.split('.').pop()))
      this.setRecetaMedicaAction({archivo:this.$refs['file'+i][0].files[0], indice:i})
    else
      this.error_archivo = true;
  },
  continuar(){
    if (this.puedeContinuar)
      this.$router.push({ name: 'Comprobar' })
  },
  saltar(){
    this.$router.push({ name: 'Comprobar' })
  }

  },
  watch:{

  },
  computed: {
    ...mapGetters([
    'receta_medica',
    'isLoggedIn', 
    'comuna', 
    'comunas', 
    'examenes_agregados', 
    'inicio', 
    'termino', 
    'dia', 
    'mes', 
    'año',
    'costo_servicio', 
    'costo_servicio_pendiente',
    'n_personas',
    'orden_final',

    'direccion',
    'apellido1',
    'nombre',
    'departamento',
    'rut',
    'email',
    'telefono',
    'n_depto', 
    ]),

  puedeContinuar(){
    for(var i = 0; i < this.orden_final.length; i++){
      if(this.orden_final[i].examenes.some(x=>{return x.requiere_orden == 1}))
       {
         if(this.orden_final[i].orden_medica == null)
         {
           return false
         }
       }
    }
  return true;
  }
  },
}
</script>
<style scoped>
.comuna_error{
  position:fixed; 
  z-index:99999; 
  top:50%; 
  transform: translate(-50%, -50%); 
  left:50%; 
  max-width:320px; 
  padding:20px; 
  background-color:white; 
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px; 
  border-radius:15px;
      -webkit-animation: move 5s linear infinite;
    -moz-animation: move 5s linear infinite;
    animation: move 0.18s linear infinite;
    animation-iteration-count:1;
}
@-webkit-keyframes move {
    0% {
        margin-left: 0px;
        margin-right: 0px;
    }
    50% {
        margin-left: 13px;
        margin-right: 0px;
    }
    100% {
        margin-left: 0px;
        margin-right: 13px;
    }

}
@media only screen and (max-width: 382px) {



.outer-container{
  padding: 0px !important; display:inline-block; margin:0 auto; border-radius:10px; position:relative;
}

.inner-container{
  display:inline-block; padding: 0px !important;  vertical-align:middle;
}
.barrita-separadora{
 left:35px !important;
}

.name-container{
  padding-left:45px !important; width:160px !important;
}
}
@media only screen and (min-width: 420px) {

.name-container{
   width:200px !important;
}
}
@media only screen and (min-width: 520px) {

.name-container{
   width:300px !important;
}
}



.name-container{
  text-align:left; padding-left:60px;  white-space: nowrap;  text-overflow: ellipsis; width:160px;
}
.icon-check{
  float:left; margin-left:5px; margin-top:0px; height:25px; width:25px; color:green;
}
.icon-excl{
  color:black; float:left; margin-left:5px; margin-top:0px; height:25px; width:25px;
}
.inner-container{
  display:inline-block; padding: 10px;  vertical-align:middle;
}
.outer-container{
  padding: 20px; display:inline-block; margin:0 auto; border-radius:10px; position:relative;
}

.barrita-separadora{
  border-right:1px solid rgb(231,231,231); height:45px; left:45px; top:-5px; position:absolute;
}



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
.btn-subir-foto{
  background-color: white;
  color:rgba(20, 134, 231, 1);
  font-weight:700;
  border: 3px solid rgba(20, 134, 231, 1);
  padding: 10px 20px;
  border-radius: 10px;
  cursor: pointer;
  transition:0.1s;
  width:220px;
  margin: 5px auto;
}
.btn-subir-foto:hover{


}
.btn-receta{
  margin-top:13px; 
  border-radius:5px; 
  padding:5px 0px; 
  width:150px; 
  position:absolute; 
  top:0px; 
  right:0px;
  color:white; 
  background-color:rgba(20, 134, 231, 1);
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px; 
  margin-right:10px; 
  cursor:pointer;
}
.btn-receta:hover{

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
  width:240px !important;
}
.btn-bien{
  width:240px !important;
}
.btn-mal{
  width:240px !important;
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