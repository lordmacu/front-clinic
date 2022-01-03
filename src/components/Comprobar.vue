<template>
  <div class="home" style="background-color:rgba(250,250,250,1); min-height:100vh;">
    <TopNavD  />
<div class="top-spacer"></div>
    <div class="container" style="background-color:rgba(250,250,250,1); text-align:left; position:relative;">
      <h4 style="text-align:center; font-weight:700; color:rgb(20, 134, 231); margin-bottom:30px;">¡Estás a un paso de agendar!</h4>
      <p style="text-align:center;">Gracias por confiar en nosotros <b><span v-if="orden_final.length == 1">{{orden_final[0].nombre}}</span> <span v-else-if="orden_final.length == 2">{{orden_final[0].nombre+' y '+orden_final[1].nombre}}</span> <span v-else v-for="o, n in orden_final">{{ n == orden_final.length-1 ? ' y '+o.nombre: o.nombre + ', '}}</span>. </b> 
      Por favor, confirma que la información es correcta y nos veremos en <b>{{direccion}} {{departamento ? 'Departamento '+n_depto:''}}, {{comunas.filter(x=>{return x.id == comuna})[0].Nombre}}</b>, el día 
     <b> {{new Date(año, mes, dia).toLocaleDateString("es-ES", { year: 'numeric', month: 'long', day: 'numeric' })}} entre {{timeConvert(inicio)}} y {{timeConvert(termino)}} hrs</b></p>

      <div class="alert alert-info" style="background-color:rgb(20, 134, 231, 0.2) !important; border-left:6px solid rgb(102, 190, 254);"  role="alert">
        <div>
          <img src="@/assets/icon/no-junk-food.png" style="max-height:20px; transform:translateY(-3px);" alt="Imagen">
          Recuerda que <strong>no podrás  consumir alimentos</strong> en las <strong> 8 horas anteriores</strong> a la toma de muestra, con la excepción de exámenes micro biológicos y PCR.
        </div>
        </div>

        <div class="alert alert-info" style="background-color:rgb(20, 134, 231, 0.2) !important; border-left:6px solid rgb(102, 190, 254);"  role="alert">
        <div style="margin-top:10px;">
          <img src="@/assets/icon/runer-silhouette-running-fast.png" style="max-height:20px;  transform:translateY(-3px);" alt="Imagen">
          Una vez tomada la muestra, recibirás el  <strong>resultado</strong> de manera digital en <strong> menos de 24 horas.</strong>
        </div>
        </div>
        <div   class="alert alert-info" style="margin-bottom:40px; background-color:rgb(20, 134, 231, 0.2) !important; border-left:6px solid rgb(102, 190, 254);"  role="alert">
        <div style="margin-top:10px;">
          <img src="@/assets/icon/shield.png" style="max-height:20px;  transform:translateY(-3px);" alt="Imagen">
          Recuerda que tu <strong>muestra</strong> será siempre analizada por <strong> laboratorios certificados </strong>con los más altos estándares aplicables.
        </div>
      </div>


      <div style=" padding: 20px; box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px; border-radius:5px;">
        <h4 style="font-weight:700; font-size:18px; margin-top:0px; text-align:center;">Resumen de Orden</h4>
        <table style="width:100%;">
          <tr>
            <td class="desktop" style="width:30px;"></td>
            <td>Examen</td>
            <td style="width:10%; text-align:center;">Cantidad</td>

            <td style="width:10%; text-align:center;">Precio</td>
          </tr>
          <tr v-for="e in examenes_agregados">
            <td class="desktop" style="padding:4px; color:rgb(108, 193, 252);">
              <!--<b-icon v-on:click="clickearExamenAction(e)"  style="cursor:pointer;" icon="x-circle"/>-->
              </td>
            <td>{{e.nombre}}</td>
            <td style="text-align:center;">
              <!--<b-icon style="cursor:pointer;" v-on:click="cambiarCantidad(e, 'restar')" icon="dash-circle"/>-->
              {{e.cantidad}} 
              <!--<b-icon style="cursor:pointer;" icon="plus-circle" v-on:click="cambiarCantidad(e, 'sumar')"/>-->
             </td>
            
            <td style="padding:15px 0px;">
              <div style="display:block; text-align:center; background:rgb(210, 236, 254); color:rgb(7, 136,224); padding: 2px 8px 2px 4px; border-radius:5px;">
              ${{Intl.NumberFormat('de-DE').format(e.precio*e.cantidad)}}
              </div>
              </td>
          </tr>

          <tr>
            <td class="desktop" > </td>
            <td style="padding-top:20px;"></td>
            <td></td>
            <td></td>
          </tr>

          <tr>
            <td class="desktop" style="width:30px; border-top: 1px solid rgb(140,140,140);"></td>
            <td style="border-top: 1px solid rgb(140,140,140); margin-top:40px;"></td>
            <td style="border-top: 1px solid rgb(140,140,140); ">Total exámenes:</td>
            <td style="border-top: 1px solid rgb(140,140,140);">${{Intl.NumberFormat('de-DE').format(examenes_agregados.reduce((x, y)=>{return x+y.precio*y.cantidad}, 0))}}</td>
          </tr>
          <!--
          <tr>
            <td  class="desktop" ></td>
            <td></td>
            <td>Número de personas:</td>
            <td style="text-align:left;"><b-icon style="cursor:pointer;" icon="plus-circle" v-on:click="cambiarCantidadPersonas('sumar')"/> {{n_personas}} <b-icon style="cursor:pointer;" v-on:click="cambiarCantidadPersonas('restar')" icon="dash-circle"/></td>
          </tr>-->
          <tr>
            <td class="desktop"></td>
            <td></td>
            <td style=" vertical-align:top;">Toma de muestra (c/u):</td>
            <td style=""> <div style="text-decoration:line-through;"> <span v-if="n_personas>1">${{Intl.NumberFormat('de-DE').format((costo_servicio+costo_servicio_pendiente))}}</span></div>  <span style="color:rgb(108, 193, 252);">${{  Intl.NumberFormat('de-DE').format(Math.round((costo_servicio+(n_personas*costo_servicio_pendiente))/n_personas)) }}</span> </td>
          </tr>
          <tr>
            <td class="desktop"></td>
            <td></td>
            <td><b>Total a pagar:</b></td>
            <td style="font-weight:700;">${{Intl.NumberFormat('de-DE').format(examenes_agregados.reduce((x, y)=>{return x+y.precio*y.cantidad}, 0) + costo_servicio+(n_personas*costo_servicio_pendiente))}}</td>
          </tr>
        </table>
        <div style="width:100%;">
        
        </div>
      </div>
      <div class="tyc-container" >
        <input type="checkbox" id="checkbox" v-model="aceptoTerminos">
         Acepto los 
         <span class="tyc" style="cursor:pointer; color:rgb(20, 134, 231); text-decoration: underline;" v-on:click="viendoTerminos = true;"> Términos y Condiciones</span>
          y las 
         <span class="pdp" style="cursor:pointer; color:rgb(20, 134, 231); text-decoration: underline;" v-on:click="viendoPoliticas = true;"> Políticas de Privacidad</span>
         
         </div>
      <router-link :to="{name:'DatosPersonales'}" tag="button" style="margin-top:10px; float:left;" class="btn btn-volver"><b-icon  icon="arrow-left-circle"/> Volver</router-link>
      
      <!--<div style="float:right;"><button v-on:click="checkout()" style="margin-top:10px; float:right;" :class="{'btn-success': puedeContinuar, 'btn-danger':!puedeContinuar}" class="btn">Métodos de pago <b-icon style="transform:translateY(2px);" icon="arrow-right-circle"/></button></div>
      -->
      <div style="float:right;"><button v-on:click="transferencia()" style="margin-top:10px; float:right;" :class="{'btn-bien': aceptoTerminos && examenes_agregados.length > 0, 'btn-mal':!(aceptoTerminos && examenes_agregados.length > 0)}" class="btn">
        Agendar 
        <b-icon v-if="!loading_transferencia" icon="calendar"/>
        <div v-if="loading_transferencia" class="spinner-border text-light spinner-border spinner-border-sm" style="margin: 0 auto; color:white;" role="status"></div>
        </button></div>
    </div>
    <div class="bottom-spacer"></div>

    <div v-if="viendoTerminos">
      <div style="position:fixed; background-color:black; height:100vh; width:100vw; top:0px; z-index:10000; opacity:0.5;">


      </div>
      <div style="position:fixed; height:Calc(100vh - 120px); overflow-y: scroll; padding:10px; background:white; top:20px; width:800px; max-width:90%; margin: 0px auto; left: 0; right: 0; opacity:1; z-index:10001;">
          <div class="tyc">
          <TyC class="tyc" />
          <button class="btn btn-dark" style="margin: 20px;" v-on:click="viendoTerminos = false;">Entendido</button>
          </div>
      </div>
    </div>


    <div v-if="viendoPoliticas">
      <div style="position:fixed; background-color:black; height:100vh; width:100vw; top:0px; z-index:10000; opacity:0.5;">


      </div>
      <div style="position:fixed; height:Calc(100vh - 120px); overflow-y: scroll; padding:10px; background:white; top:20px; width:800px; max-width:90%; margin: 0px auto; left: 0; right: 0; opacity:1; z-index:10001;">
          <div class="pdp">
          <PdP class="pdp" />
          <button class="btn btn-dark" style="margin: 20px;" v-on:click="viendoPoliticas = false;">Entendido</button>
          </div>
      </div>
    </div>


  </div>

</template>

<script>
import axios from 'axios'
import TopNavD from '@/components/TopNavDFijo.vue'
import TyC from '@/components/TerminosYCondiciones.vue'
import PdP from '@/components/PoliticasDePrivacidad.vue'
import {mapGetters, mapActions} from 'vuex';

export default {
  name: 'Home',
  components: {
    TyC,
    TopNavD,
    PdP,
  },
  data() {
    return {
      viendoTerminos:false,
      viendoPoliticas:false,
      aceptoTerminos:false,
      loading_transferencia:false,
      loading_transbank:false,
    
    }
  },
  created() {

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
          ['f', 'm'].includes(this.orden_final[i].sexo) &&
          this.orden_final[i].email.length > 2 && 
          this.orden_final[i].apellido1.length > 1 &&
          this.orden_final[i].nacionalidad.length > 1 &&
          this.orden_final[i].telefono.length > 6 &&
          this.verificar_correo(this.orden_final[i].email) &&
          (this.verificar_rut(this.orden_final[i].rut) || this.orden_final[i].tipo_identificador != 'rut')
          
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

  },
  methods: {
    ...mapActions(['clickearExamenAction', 'cambiarCantidadAction', 'cambiarCantidadPersonasAction']),
    timeConvert(n) {
      var num = n;
      var hours = (num / 60);
      var rhours = Math.floor(hours);
      var minutes = (hours - rhours) * 60;
      var rminutes = Math.round(minutes);
      return (rhours < 10 ? '0':'')+rhours + ":"+ (rminutes < 10 ? '0':'') + rminutes + "";
    },
    verificar_correo(c){
      const re = /^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      if(c != '')
        return re.test(c) ? true: false;
      return true;
    },

    verificar_rut(r)
    {
      function dv(T){
        var M=0,S=1;
        for(;T;T=Math.floor(T/10))
          S=(S+T%10*(9-M++%6))%11;
        return S?S-1:'k';
      }
    function validaRut (rutCompleto) {
        if (!/^[0-9]+[-|‐]{1}[0-9kK]{1}$/.test( rutCompleto ))
          return false;
        var tmp 	= rutCompleto.split('-');
        var digv	= tmp[1]; 
        var rut 	= tmp[0];
        if ( digv == 'K' ) digv = 'k' ;
          return (dv(rut) == digv ) ? true : false;
      }
      return validaRut(r)
    },

    cambiarCantidadPersonas(tipo){
        if (tipo == "sumar")
          this.cambiarCantidadPersonasAction({cantidad:this.n_personas+1})
        else
          this.cambiarCantidadPersonasAction({cantidad:this.n_personas-1})
        this.$forceUpdate();
    },
  cambiarCantidad(e, tipo){
    if (tipo == "sumar")
      this.cambiarCantidadAction({eid:e.id, cantidad:e.cantidad+1})
    else
      this.cambiarCantidadAction({eid:e.id, cantidad:e.cantidad-1})
    if(this.examenes_agregados.length == 0)
      this.$router.push({ name: 'Home' })
    this.$forceUpdate();
  },
  checkout(){
    if(this.puedeContinuar)      
      this.$router.push({ name: 'MetodosDePago' })

  },
      close(e){
      if (!e.target.classList.contains('tyc') && this.viendoTerminos)
        this.viendoTerminos = false;
      if (!e.target.classList.contains('pdp') && this.viendoPoliticas)
        this.viendoPoliticas = false;
      },
    redirectPost(url, data) 
    {
      var form = document.createElement('form');
      document.body.appendChild(form);
      form.method = 'post';
      form.action = url;

      for (var name in data) 
      {
          var input = document.createElement('input');
          input.type = 'hidden';
          input.name = name;
          input.value = data[name];
          form.appendChild(input);
      }
      form.submit();
    },

    transferencia(){
       if(this.puedeContinuar && this.aceptoTerminos)
      {
        const json_ob = JSON.stringify({
        examenes:this.examenes_agregados,

        dia:this.dia,
        mes:this.mes,
        año:this.año,

        comuna_id:this.comuna,
        desde:this.inicio,
        hasta:this.termino,
        direccion:this.direccion,
        departamento:this.departamento,
        n_depto:this.n_depto,
        orden:this.orden_final,

        })
        const blob = new Blob([json_ob], {type:'application/json'});
        let data = new FormData();
        data.append("document", blob)
        for(var f = 0; f < this.orden_final.length; f++){
          if(this.orden_final[f].orden_medica != null)
          {
            data.append(f, this.orden_final[f].orden_medica)
          }

        }
        this.loading_transferencia = true;
        axios({method:'post', url:window.hostname+'api/prueba', data:data})
        .then(r=>{
          this.$router.push({ path: '/PagoRealizado/'+r.data.orden_id })
          this.loading_transferencia = false;
        })
        .catch(e=>{
          this.loading_transferencia = false;
        })
        this.$gtag.event('comprobar-agendar', {'event_category': 'agendar', 'event_label': 'correcto', 'value': 1} );
      }
      else{
        this.$gtag.event('comprobar-agendar', {'event_category': 'agendar', 'event_label': 'incorrecto', 'value': 1} );
      }
    },
/*
    transferencia(){
      if(this.puedeContinuar && this.aceptoTerminos)
      {
        this.loading_transferencia = true;
        axios.post(window.hostname+'api/reservar_sin_identificar_transferencia',
        {
        examenes:this.examenes_agregados,

        dia:this.dia,
        mes:this.mes,
        año:this.año,

        comuna_id:this.comuna,
        desde:this.inicio,
        hasta:this.termino,
        direccion:this.direccion,
        departamento:this.departamento,
        n_depto:this.n_depto,
        orden:this.orden_final,

        })
        .then(r=>{
          this.$router.push({ path: '/PagoRealizado/'+r.data.orden_id })
          this.loading_transferencia = false;
        })
        .catch(e=>{
          this.loading_transferencia = false;
        })
        this.$gtag.event('comprobar-agendar', {'event_category': 'agendar', 'event_label': 'correcto', 'value': 1} );
      }
      else{
        this.$gtag.event('comprobar-agendar', {'event_category': 'agendar', 'event_label': 'incorrecto', 'value': 1} );
      }

    }
    */

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
    loading(){
      return this.loading_transferencia || this.loading_transbank
    },
    puedeContinuar(){
      return this.examenes_agregados.length > 0 && !this.loading && this.direccion.length > 2 && this.puedeContinuarError == ''
    },

    puedeContinuarError(){
      let mails = []
      for (var i = 0; i<this.orden_final.length; i++){
        if (this.orden_final[i].examenes.length > 0)
        {

          if(!(

          this.orden_final[i].nombre.length > 1 &&
          this.orden_final[i].rut.length>7 &&
          ['f', 'm'].includes(this.orden_final[i].sexo) &&
          this.orden_final[i].email.length > 2 && 
          this.orden_final[i].apellido1.length > 1 &&
          this.orden_final[i].nacionalidad.length > 1 &&
          this.orden_final[i].telefono.length > 6 &&
          this.verificar_correo(this.orden_final[i].email) &&
          (this.verificar_rut(this.orden_final[i].rut) || this.orden_final[i].tipo_identificador != 'rut')
          
          ))
          {
            console.log("nope")
            return 'Hay pacientes con información incompleta.'
          }
          if(this.orden_final[i].email.length != '')
            mails.push(this.orden_final[i].email)
        }
      }
      if (this.direccion.length < 3)
        return 'Falta agregar dirección.'
      if (this.examenes_agregados.length == 0)
        return 'Debes agregar al menos un examen.'
      if (!(mails.length === new Set(mails).size))
        return 'No puede haber pacientes con e-mails repetidos.'

      return ''
    },
  
    error_crear_correo_electronico(){
      const re = /^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      if(this.email != '')
        return re.test(this.email) ? '': 'Correo en formato incorrecto.';
      return '';
    },

    error_crear_rut(){
      function dv(T){
        var M=0,S=1;
        for(;T;T=Math.floor(T/10))
          S=(S+T%10*(9-M++%6))%11;
        return S?S-1:'k';
    }
    function validaRut (rutCompleto) {
        if (!/^[0-9]+[-|‐]{1}[0-9kK]{1}$/.test( rutCompleto ))
          return 'Formato invalido.';
        var tmp 	= rutCompleto.split('-');
        var digv	= tmp[1]; 
        var rut 	= tmp[0];
        if ( digv == 'K' ) digv = 'k' ;
          return (dv(rut) == digv ) ? '' : 'Digito verificador incorrecto.';
      }
    if (this.rut != '')
      return validaRut(this.rut)
    return ''
    },

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
  height:0px;
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