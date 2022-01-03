<template>
<div style="margin-top:0px; position:relative;">
  <div v-if="estado == 1">


<TopNavD /> 
<div :style="{'height': window.width >= 1200 ? '100px': '56px'}" style="height:100px; width:100%;"></div>
<div class="barra-lateral" :style="{'height': window.width >= 1200 ? 'Calc(100vh - 100px)': 'Calc(100vh - 60px)', 'top': window.width >= 1200 ? '100px': '56px'}" style="position:absolute; overflow-y: scroll; padding:30px 0px;  width:450px; background-color:white; z-index:9999; top:100px; left: 00px;">
  <div v-if="profesional.hasOwnProperty('lat') && this.socket.connected">
    
  <div style="padding: 0px 20px;">
  <h4 style="text-align:center; font-weight:700; color:rgb(20, 134, 231); margin-bottom:30px; margin-top:30px;">¡Tu orden ya está en camino!</h4>

  <img style="max-width:250px;" src="@/assets/gif/moto.gif" alt="">
  <!--
  {{(datos_orden.lat-profesional.lat)}} {{datos_orden.lon-profesional.lon}} {{window.height-100}}
  {{Math.log((180*(window.height))/(256*Math.abs(datos_orden.lat-profesional.lat)))/Math.log(2)}}
  {{Math.log((360*window.width)/(256*Math.abs(datos_orden.lon-profesional.lon)))/Math.log(2)}}
  {{zoom_computed}}
  
  {{datos_orden}}
  <input v-on:input="updatelon($event.target.value)" v-model="datos_orden.lon" type="number">
  <input v-on:input="updatelat($event.target.value)" v-model="datos_orden.lat" type="number">
  <button v-on:click="forceupdate()"></button>
{{center_computed}}
-->


  <!--
  <hr />
  -->
  </div>
  <div style="">
  <div style="text-align:left; width:100%;" class="profesionales" @mouseenter="listMouseOver(p)" @mouseleave="listMouseLeave(p)">

    <div style="margin-bottom:20px; padding: 0px 20px;">
    <!--<div style="display:inline;"> {{p.id}} </div>
    -->
    <div style="text-align:center;">En unos momentos más <span v-if="containsKey(profesional, 'nombre') && containsKey(profesional, 'apellido')" style="color:rgb(7, 136,224); font-weight:600;">{{profesional.nombre}} {{profesional.apellido}}</span>  estará en <span style="color:rgb(7, 136,224); font-weight:600;">{{datos_orden.direccion}}</span> para tomar tu muestra. La ubicación que se muestra en el mapa es aproximada.</div>

    <!--
          <button v-on:click="gotoMiddle()">asd </button>
    {{center_real}}
    <div style="display:inline;" v-if="containsKey(profesional, 'email')"> {{profesional.email}}</div>
    
    <div style="display:inline-block; font-size:14px; font-style: italic;" v-if="containsKey(profesional, 'ultima_vez')"> Última actualización hace {{getTimeDifference(profesional.ultima_vez) }}.</div>
    -->

    </div>
  </div>
</div>


  </div>
  <div style=" padding-top:70px;" v-else>
    
<div style="margin: auto auto;" class="loader"></div>
<div style="color:rgb(140, 140, 140); margin-top:10px; font-style: italic;">Cargando...

</div>

  </div>
<img style="position:absolute;bottom:0px; left:0px; z-index:-1;" src="@/assets/svg/deco1.svg" alt="">
</div>


<!--
{{profesionales}}
-->
<div  class="map" :style=" {'height': window.width >= 1200 ? 'Calc(100vh - 100px)' : 'Calc(100vh - 50px)'}" >
<!--no-blocking-animations-->
   <l-map 
    :options="{attributionControl:false, zoomControl: false, dragging:false, scrollWheelZoom: false, touchZoom:false, doubleClickZoom:false, zoomSnap: 0.125}"
    @update:zoom="zoomUpdate"
    :ref="'map'"
    @update:center="centerUpdate"
    :zoom="zoom_computed" 
    :center="center_real">
    <l-tile-layer :url="url"></l-tile-layer> 
    <l-marker v-if="datos_orden.lat != null && datos_orden.lon != null"
    :lat-lng="[datos_orden_c.lat, datos_orden_c.lon]"
    :icon="icon2"
          :key="datos_orden.hasta"
      :ref="'orden_'"
    
    >

    </l-marker>



            <l-moving-rotated-marker v-if="profesional.hasOwnProperty('lat')"
     
      :key="profesional.id"
      :ref="'marker_'+profesional.id"


      :lat-lng="[profesional.lat, profesional.lon]"
      :visible="true"
      
      :duration="2000"
      :rotationAngle="angle"
      :icon="icon"
  >

        </l-moving-rotated-marker>
    <!--<l-control-zoom position="topright"  ></l-control-zoom>-->
   </l-map>
</div>
</div>
<div v-else-if="estado == 0">
      <div style="padding-top:Calc(50vh - 100px);">
        <img src="@/assets/logo3.png" style="max-height:60px;" alt="Imagen"> 
        <div class="loader"></div>
      </div>
</div>
<div style="position:relative;" v-else-if="estado == 2">
  
  <div style="   margin: 0;
  position: absolute;
  top: 120px;
  left: 50%;
  -ms-transform: translate(-50%, -0%);
  transform: translate(-50%, -0%);">
      <img style="max-height:250px;" src="@/assets/svg/warning.svg" alt="">
      <div><h4 style="text-align:center; font-weight:700; color:rgb(20, 134, 231); margin-bottom:30px; margin-top:30px;">¡Error!</h4></div>       
      <div style="margin-top:20px; margin: 0 auto; max-width:600px; margin-bottom:10px;">Esta orden no existe, o dejó de estar en curso.</div>
      <router-link style="background-color: rgb(254, 177, 53); border: 1px solid rgb(254, 177, 53); margin-top:20px; padding:12px 10px;" class="btn btn-success btn-bien" :to="{name:'Home'}"> Ir al inicio</router-link>
    </div>

</div>
<div style="position:relative; height:100vh; width:100vw;" v-else-if="estado == 3">
  <div style="   margin: 0;
  position: absolute;
  top: 120px;
  left: 50%;
  -ms-transform: translate(-50%, -0%);
  transform: translate(-50%, -0%);">
      <img style="max-height:250px;" src="@/assets/svg/moto.svg" alt="">
      <div><h4 style="text-align:center; font-weight:700; color:rgb(20, 134, 231); margin-bottom:30px; margin-top:30px;">¡Muestra tomada!</h4></div>       
      <div style="margin-top:20px; margin: 0 auto; max-width:600px; margin-bottom:10px;">Hemos terminado con tu toma de muestra. Ahora solo falta esperar los resultados.</div>
      <router-link style="background-color: rgb(254, 177, 53); border: 1px solid rgb(254, 177, 53); margin-top:20px; padding:12px 10px;" class="btn btn-success btn-bien" :to="{name:'Home'}"> Ir al inicio</router-link>
    </div>
</div>
</div>
</template>

<script>
import axios from 'axios';
import * as L from 'leaflet'
import { latLng } from "leaflet";
import { LMap, LTileLayer, LMarker, LGeoJson, LCircle, LCircleMarker, LPopup, LIcon, LControlAttribution, LRectangle, LPolygon,LPolyline, LControlZoom } from "vue2-leaflet";
import "leaflet/dist/leaflet.css";
import io from 'socket.io-client';
import {mapGetters, mapActions} from 'vuex';
import LMovingRotatedMarker from 'vue2-leaflet-moving-rotated-marker'
import TopNavD from '@/components/TopNavDFijo.vue'


window.socket = 'https://socket.clinicgo.cl';

export default {
  name: 'HelloWorld',
  props: ['token'],
    components: {
      TopNavD,
      LMap, 
      LTileLayer, 
      LMarker, 
      LCircle, 
      LIcon,
      LControlAttribution,
      LControlZoom,
      LMovingRotatedMarker,
      LRectangle,
      LPolygon,
      LPolyline,
      LPopup,
      LCircleMarker,
    },
  data() {
    return {
        icon: L.icon({
          iconUrl: require('@/assets/auto_i.png'),
          iconSize: [30,30],
          iconAnchor: [15,15],
          popupAnchor: [0,-7]
      }),
        icon2: L.icon({
          iconUrl: require('@/assets/persona2.png'),
          iconSize: [30,30],
          iconAnchor: [15,15],
          popupAnchor: [0,-7]
      }),
      window: {
          width: 0,
          height: 0
      },
      circulo:{'mostrando':false, 'lat':0, 'lon':0},
      profesional:{},
      key:1,
      angle:0,
      socket:undefined,
      zoom:12,
      estado:0,
      center: L.latLng(-33.438926, -70.651746),
      currentCenter: L.latLng(-33.438926, -70.651746),
      url:'https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}{r}.png',
      datos_orden:{lat:null, lon:null, direccion:null, desde:null, hasta:null},
    }
  },
  methods:{
    updatelat(v){
      this.$set(this.datos_orden, 'lat', v)
      console.log(this.datos_orden.lat, v)
      this.$refs.orden_.mapObject.setLatLng([this.datos_orden.lat, this.datos_orden.lon]);
    },
    updatelon(v){
      this.$set(this.datos_orden, 'lon', v)
      this.$refs.orden_.mapObject.setLatLng([this.datos_orden.lat, this.datos_orden.lon]);
    },
    forceupdate(){
      this.goto((Number(this.datos_orden.lat) + this.profesional.lat)/2, (Number(this.datos_orden.lon) + this.profesional.lon)/2)
    },
    
        handleResize() {
          this.window.width = window.innerWidth;
          this.window.height = window.innerHeight;
      },
      init_socket(){
        console.log("iniciando socket...")
        this.socket = io(window.socket,   {extraHeaders: {
        "token": this.$route.params.token,
        "tipo": "cliente",
      }})
      this.socket.on("connect", ()=>{
        console.log("Se conecto")
      })
      this.socket.on("escuchar_info", (data) => {
      console.log("escuchando...", data)
      var datax = JSON.parse(data)
      var nAngle = Math.atan2(datax.lon - this.profesional.lon, datax.lat - this.profesional.lat) * 180 / Math.PI;
      if (nAngle != 0)
        this.angle = nAngle
      this.profesional = datax
      setTimeout(() => {this.gotoMiddle()}, 500 )
    });
    this.socket.on("terminar_orden", () => {
        console.log("termino orden...")
        this.estado = 3
        this.datos_orden = {lat:null, lon:null, direccion:null, desde:null, hasta:null}
      });

      },
      initialize(){
        axios.post(window.hostname+'api/seguir_orden', {token:this.$route.params.token})
        .then(r => 
        {
          this.init_socket();
          this.datos_orden = r.data
          this.estado = 1;
        })
        .catch(e=>{          
          this.estado = 2;

        })
        
      },
    
      openPopup: function (event) {
        console.log("LOL")
          event.target.openPopup();
      },
      closePopup: function (event) {
          event.target.closePopup();
      },

    getTimeDifference(t){
      let diff = Date.now() - new Date(t).getTime()
      let minutos = Math.floor(((diff % 86400000) % 3600000) / 60000)
      let segundos = Math.floor((((diff % 86400000) % 3600000) % 60000)/1000)
      if (minutos > 0)
        return minutos+' minutos y '+segundos+ (segundos > 1 && segundos != 0 ? ' segundos' : ' segundo')
      return segundos+ (segundos > 1 && segundos != 0 ? ' segundos' : ' segundo')

    },
    containsKey(obj, key ) {
        return Object.keys(obj).includes(key);
    },
    listMouseOver: function(x){
//

    },
    listMouseLeave: function(x){
      this.circulo.mostrando = false;

    },
    gotoMiddle(){
      let lat = 0;
      let lon = 0;
      if(this.datos_orden.lat != null && this.datos_orden.lon != null && this.profesional.lat != null && this.profesional.lon != null)
      {
       lat = (Number(this.datos_orden.lat) + this.profesional.lat)/2
       lon = (Number(this.datos_orden.lon) + this.profesional.lon)/2
      }else if ( (this.datos_orden.lat == null || this.datos_orden == null) && this.profesional.lat != null && this.profesional.lon != null)
      {
        lat = this.profesional.lat
        lon = this.profesional.lon
      }
      else if ( (this.profesional.lat == null || this.profesional == null) && this.datos_orden.lat != null && this.datos_orden.lon != null)
      {
          lat = this.datos_orden.lat 
          lon = this.datos_orden.lon
      }
      else{
          lat = -33.438926
          lon = -70.651746
      }
      this.center_real =  L.latLng(lat+(Math.random()*0.000001), lon+(Math.random()*0.000001));
      this.$forceUpdate()
    },
    goto: function(lat, lon){
      console.log("goto ejecutada")
      if (lat != null && lon != null)
        this.center =  L.latLng(lat+(Math.random()*0.000001), lon+(Math.random()*0.000001));
    },
    zoomUpdate: function(zoom){
      this.currentZoom = zoom;
    },
      centerUpdate: function(center){
      this.currentCenter = center;
    },


  },
  watch:{
    center_computed(){
      this.center_real = this.center_computed;

    },
    zoom_computed(){
      console.log("cambio el centro...")
    },
    zoom_computed(){

      setTimeout(() => this.gotoMiddle(), 200 )
    }
   /* profesionales: function(val){
      this.key += 1;
      this.profesionales_filtrados = []
      let ret = []
      for(var z of this.profesionales)
      {
        if(z.lat != null && z.lon != null)
        this.profesionales_filtrados.push(z)

      }
      //this.profesionales_filtrados = ret
      this.$forceUpdate();
    }
    */
  },
  computed:{
    ...mapGetters(['comunas', 'regiones', 'prestaciones', 'tipos', 'admin_datos', 'comunas_usadas', 'token']),
    zoom_computed(){
    if(this.datos_orden.lat != null && this.datos_orden.lon != null && this.profesional.lat != null && this.profesional.lon != null)
    {
  return (Math.round(4*
    (
  Math.min(Math.log((180*(this.window.height - (this.window.width >= 1200 ? 50 : 0)))/(256*Math.abs(this.datos_orden.lat-this.profesional.lat)))/Math.log(2),
  Math.log((360*(this.window.width-(this.window.width >= 1200 ? 590 : 100)))/(256*Math.abs(this.datos_orden.lon-this.profesional.lon)))/Math.log(2))
    )
    ))/4
    }
    else{
      return 15;
    }
    },
    datos_orden_c(){
      let ret = this.datos_orden
      ret.f = "A"
      return ret
    },
    center_computed(){
      console.log("center copmuted: ", this.datos_orden.lat, this.datos_orden.lon, this.profesional.lat, this.profesional.lon)
      if(this.datos_orden.lat != null && this.datos_orden.lon != null && this.profesional.lat != null && this.profesional.lon != null)
      {
        return L.latLng((Number(this.datos_orden.lat) + this.profesional.lat)/2, (Number(this.datos_orden.lon) + this.profesional.lon)/2)
      }else if ( (this.datos_orden.lat == null || this.datos_orden == null) && this.profesional.lat != null && this.profesional.lon != null)
      {
        return L.latLng(this.profesional.lat, this.profesional.lon)
      }
      else if ( (this.profesional.lat == null || this.profesional == null) && this.datos_orden.lat != null && this.datos_orden.lon != null)
      {
          return L.latLng(this.datos_orden.lat, this.datos_orden.lon)
      }
      else{
          return L.latLng(-33.438926, -70.651746)
      }

    },
  },

  created() {
      window.addEventListener('resize', this.handleResize);
      this.handleResize();
  },

  mounted(){
    this.initialize()
  }

}
</script>


<style scoped>

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

.btn-ir{
  background-color: white;
  color:rgba(20, 134, 231, 1);
  font-weight:700;
  border: 3px solid rgba(20, 134, 231, 1);
  padding: 0px 13px;
  border-radius: 10px;
  cursor: pointer;
  transition:0.1s;


}

.loader {

}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
.btn-ir:hover{
  color:rgba(20, 134, 231, 1);

}

  .map{
    
      /*-moz-animation: 
      height: 320px;
      width: 320px;
      */
      /*transform: translate(0px, -120px);*/
      /*height: Calc(100vh - 100px);*/
      /*min-width: 400px;*/

      margin-left:450px;
      margin-right:0 !important;
  }

td{
  padding: 5px 10px 0px 5px;
}
@media only screen and (max-width: 1200px) {
.map{
  margin-left:0px;
}
.barra-lateral{
  display: none;
}

}


</style>

