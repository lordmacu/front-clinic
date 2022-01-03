<template>
<div style="">

    <div v-on:click="focused_comuna = false; focused_exam = false;" class="dont-close-exam dont-close-comuna" v-if="focused_exam || focused_comuna" style="height:100vh; position:fixed; width:100vw; background:rgba(0,0,0); z-index:99; top:0px; left:0px; opacity:0.4;">

     </div>
    <div class="main-search">

      <div style="position:relative;  z-index:-1;"  @mouseenter="hovering_exam = true" @mouseleave="hovering_exam = false" v-on:click="focusInputExam()" :style="{ 'z-index': focused_exam ? '10002':'1', 'width': focused_exam || focused_prev || focused_comuna || (examenes_agregados.length > 0 && comuna != -1) ? '50%':'60%', '-webkit-box-shadow': focused_exam ? '2px 3px 25px -1px rgba(0,0,0,0.31)': 'none', 'background':focused_exam ? 'white': ''}" class="main-search-exam dont-close-exam">
      <div :class="{'subido': focused_exam || examenes_agregados.length != 0, 'bajado': !(focused_exam || examenes_agregados.length != 0)}" 
      
      style=" position:absolute; transition:0.1s; z-index:1; ">
Examen o prestación
      </div>
      <div v-if="!focused_exam && examenes_agregados.length >0" style="position:absolute; left:60px; top:23px; z-index:4; color:rgb(102, 102, 102);">
        {{examenes_agregados.reduce((x, y)=>{return x+y.cantidad}, 0)}} {{examenes_agregados.reduce((x, y)=>{return x+y.cantidad}, 0) > 1 ? 'exámenes agregados': 'examen agregado'}}.
      </div>
      <b-icon style="position:absolute; color:rgb(7, 136,224); top:24px; height:20px; width:20px;" icon="heart-fill"></b-icon>
      <b-icon style="position:absolute; right:30px; top:25px; color:rgb(7, 136,224); z-index:4; " :style="{'transform': focused_exam ? 'rotate(180deg)': 'none'}" icon="chevron-down"></b-icon>   
        <!--<input class="dont-close-exam data-hj-allow" v-model="examen_buscado" v-on:keyup.enter="onEnterExamen()" :style="{'background':focused_exam ? 'white': ''}" ref="examen"  @focus="focusInputExam(); examen_buscado = ''" type="text">-->
        <input style="width:100%; font-size:16px; background:none; z-index:6; transform:translate(30px, 13px); " class="dont-close-exam data-hj-allow" v-on:keyup.enter="onEnterExamen()" v-on:input="examen_buscado = $event.target.value" v-model="examen_buscado" :style="{'background':focused_exam ? 'none': ''}" ref="examen"  @focus="focusInputExam(); examen_buscado = ''" type="text" 
        :placeholder="focused_exam ? 'Digite examen':''">
        <!--<div v-if="!focused_exam && examen_buscado == ''" style="position:absolute; left:36px; top:30px; cursor:text; color:rgb(0,0,0,0.6); font-size:14px;">¿Qué examen necesitas?</div> -->

        <div :style="{'display':hovering_exam || focused_exam || hovering_comuna || focused_comuna ? 'none':'', 'border-left': focused_exam || focused_prev || focused_comuna || (examenes_agregados.length > 0 && comuna != -1) ? '1px solid rgba(195,195,195,0)':'1px solid rgba(195,195,195,1)'} "  class="vl-exam"></div>
      



      
      </div>




      <div style="position:relative;" @mouseenter="hovering_comuna = true" @mouseleave="hovering_comuna = false" v-on:click="focusInputComuna($event)" :style="{ 'z-index': focused_comuna ? '10002':'1',  'width': focused_exam || focused_prev || focused_comuna  || (examenes_agregados.length > 0 && comuna != -1) ? 'Calc(50% - 5px)':'Calc(40% - 5px)','-webkit-box-shadow': focused_comuna ? '2px 3px 25px -1px rgba(0,0,0,0.31)': 'none', 'background':focused_comuna ? 'white': ''}" class="main-search-comuna">
           <div :class="{'subido2': focused_comuna || comuna != -1, 'bajado2': !focused_comuna && comuna == -1}" 
      
      style=" position:absolute; transition:0.1s; z-index:1; ">
Ciudad o comuna 
      </div>
        <div v-if="!focused_comuna && comuna != -1" style="position:absolute; left:58px; top:23px; z-index:4; color:rgb(102, 102, 102);">
        {{comunas_usadas.filter(x=>{return x.id == comuna})[0].Nombre}}.
      </div>
      <b-icon style="position:absolute; color:rgb(7, 136,224); top:20px; height:25px; width:25px; z-index:4;" icon="geo-alt-fill"></b-icon>
<b-icon style="position:absolute; left:230px; top:25px; color:rgb(7, 136,224); z-index:4; " :style="{'transform': focused_comuna ? 'rotate(180deg)': 'none'}" icon="chevron-down"></b-icon>   

        <input style="font-size:16px; background:none; z-index:6; transform:translate(30px, 12px);" class="dont-close-comuna data-hj-allow" v-on:keyup.enter="onEnterComuna()" v-model="comuna_buscado" :style="{'background':focused_comuna ? 'white': ''}" ref="comuna" @focus="focusInputComuna(); comuna_buscado = ''" type="text"
         :placeholder="focused_comuna ? 'Digite comuna':''">
        <button @mouseleave="alerta = false" @mouseenter="alerta = true"  style="margin-top:4px;" class="search-btn dont-close-btn dont-close-exam dont-close-prev dont-close-comuna" v-on:click="checkout(); $gtag.event('barrabusqueda-mandar', {'event_category': 'busqueda', 'event_label': 'buscar', 'value': 1} );"
        :style="{'width': focused_exam || focused_prev || focused_comuna || (examenes_agregados.length > 0 && comuna != -1) ? '120px':'50px',
    
                 'background': comuna != -1 && examenes_agregados.length > 0 ? 'rgb(254, 177, 53)':'rgb(255,222,166)'}"
        :class="{'glow': comuna != -1 && examenes_agregados.length > 0}"
                 >
                 <!--'#3D9970':'#FF4136' -->
                 <b-icon class="dont-close-btn dont-close-exam dont-close-prev dont-close-comuna" style="position:absolute; left:15px; top:17px;" icon="calendar"/><div class="dont-close-btn dont-close-exam dont-close-prev dont-close-comuna" :style="{'color': focused_exam || focused_prev || focused_comuna || (examenes_agregados.length > 0) ? 'rgba(255,255,255,1)':'rgba(255,255,255,0)'}" style="margin-left:20px; display:inline; transition-delay:0.3s; font-size:14px; font-weight:700;">{{focused_exam || focused_prev || focused_comuna || (examenes_agregados.length > 0 && comuna != -1) ? '  Agendar':''}}</div>
        </button>

      </div>

    <div v-if="focused_exam"  style=" vertical-align:middle;" class="search-exam-popup dont-close-exam">

      
      <div v-if="resultado_fuse_examen.length == 0" style="" class="dont-close-exam">

<!--
     <b-icon icon="search" style="height:40px; width:80px; padding-left:20px; padding-right:20px;" /> <div class="dont-close-exam" style="height:60px; margin-left;40px; display:inline-block; vertical-align:middle;">Escribe el examen que necesites buscar</div>
-->     
      <div style="padding-left:20px; font-weight:700; font-size:14px; padding-top:10px; color:rgb(7, 136,224); display:inline-block;">Populares: 
        
      </div>
      <div v-on:click="agregarExamenAction(examenes.filter(x=>{return x.id == 241})[0])"  style="display:inline-block; cursor:pointer; color:rgb(7, 136,224); font-size:14px; margin:0px 4px; padding: 5px 10px; border-radius:5px; background:rgb(240, 240, 240);">PCR </div>
      <div v-on:click="agregarExamenAction(examenes.filter(x=>{return x.id == 22})[0])"  style="display:inline-block; cursor:pointer; color:rgb(7, 136,224); font-size:14px; margin:0px 4px; padding: 5px 10px; border-radius:5px; background:rgb(240, 240, 240);">Hemograma </div>
      <div v-on:click="agregarExamenAction(examenes.filter(x=>{return x.id == 242})[0])"  style="display:inline-block; cursor:pointer; color:rgb(7, 136,224); font-size:14px; margin:0px 4px; padding: 5px 10px; border-radius:5px; background:rgb(240, 240, 240);">PCR Express </div>
      <div v-on:click="agregarExamenAction(examenes.filter(x=>{return x.id == 243})[0])"  style="display:inline-block; cursor:pointer; color:rgb(7, 136,224); font-size:14px; margin:0px 4px; padding: 5px 10px; border-radius:5px; background:rgb(240, 240, 240);">Perfil Lipídico </div>

       <ul style="padding-top:12px;">
         <li class="dont-close-exam " style="height:52px; position:relative; vertical-align:middle; " v-on:click="onClickExam(l); $gtag.event('barrabusqueda-examen', {'event_category': 'busqueda', 'event_label': l.nombre, 'value': 1} );" v-for="l in examenes.slice(0, 6)" >
          <!--
           <div :style="{'color': tipo_color[tipos.filter(x=>{return x.id == l.item.tipo_id})[0].id-1], 'border': '2px solid '+tipo_color[tipos.filter(x=>{return x.id == l.item.tipo_id})[0].id-1] }" style="text-align:center; border-radius: 10px; padding:2px; display:inline-block; margin-left:10px; width:110px;">
             {{tipos.filter(x=>{return x.id == l.item.tipo_id})[0].Nombre}}
             </div>
           -->
            <b-icon style="position:absolute; top: 20px; left:20px; height:14px; width:14px; color:rgb(20, 134, 231);" :icon="examenes_agregados.some(e => e.id == l.id) ? 'check': 'heart-fill'"/> 
           <div class="dont-close-exam" style="font-size: 14px; padding-top:6px; display:inline-block; overflow: hidden; margin-left:50px;  text-overflow: ellipsis; max-width:430px;  white-space: nowrap;">{{l.nombre}}</div>
           <div class="dont-close-exam" style="position:absolute; background:rgb(210, 236, 254); color:rgb(7, 136,224); padding: 2px 8px 2px 6px; border-radius:10px; right:20px; top:14px;">
          ${{Intl.NumberFormat('de-DE').format(l.precio)}}
           </div>

         </li>
       </ul>


     </div>

     <div class="dont-close-exam" v-else>


      <div style="padding-left:20px; font-weight:700; font-size:14px; padding-top:10px; color:rgb(7, 136,224); display:inline-block;">Populares: 
        
      </div>
      <div v-on:click="agregarExamenAction(examenes.filter(x=>{return x.id == 241})[0])"  style="display:inline-block; cursor:pointer; color:rgb(7, 136,224); font-size:14px; margin:0px 4px; padding: 5px 10px; border-radius:5px; background:rgb(240, 240, 240);">PCR </div>
      <div v-on:click="agregarExamenAction(examenes.filter(x=>{return x.id == 22})[0])"  style="display:inline-block; cursor:pointer; color:rgb(7, 136,224); font-size:14px; margin:0px 4px; padding: 5px 10px; border-radius:5px; background:rgb(240, 240, 240);">Hemograma </div>
      <div v-on:click="agregarExamenAction(examenes.filter(x=>{return x.id == 242})[0])"  style="display:inline-block; cursor:pointer; color:rgb(7, 136,224); font-size:14px; margin:0px 4px; padding: 5px 10px; border-radius:5px; background:rgb(240, 240, 240);">PCR Express</div>
      <div v-on:click="agregarExamenAction(examenes.filter(x=>{return x.id == 243})[0])"  style="display:inline-block; cursor:pointer; color:rgb(7, 136,224); font-size:14px; margin:0px 4px; padding: 5px 10px; border-radius:5px; background:rgb(240, 240, 240);">Perfil Lipídico </div>

       <ul style="padding-top:12px;">
         <li class="dont-close-exam " style="height:52px; position:relative; vertical-align:middle; " v-on:click="onClickExam(l.item); $gtag.event('barrabusqueda-examen', {'event_category': 'busqueda', 'event_label': l.item.nombre, 'value': 1} );" v-for="l in resultado_fuse_examen.slice(0, 6)" >
          <!--
           <div :style="{'color': tipo_color[tipos.filter(x=>{return x.id == l.item.tipo_id})[0].id-1], 'border': '2px solid '+tipo_color[tipos.filter(x=>{return x.id == l.item.tipo_id})[0].id-1] }" style="text-align:center; border-radius: 10px; padding:2px; display:inline-block; margin-left:10px; width:110px;">
             {{tipos.filter(x=>{return x.id == l.item.tipo_id})[0].Nombre}}
             </div>
           -->
            <b-icon style="position:absolute; top: 20px; left:20px; height:14px; width:14px; color:rgb(20, 134, 231);" :icon="examenes_agregados.some(e => e.id == l.item.id) ? 'check': 'heart-fill'"/> 
           <div class="dont-close-exam" style="font-size: 14px; padding-top:6px; display:inline-block; overflow: hidden; margin-left:50px;  text-overflow: ellipsis; max-width:430px;  white-space: nowrap;">{{l.item.nombre}}</div>
           <div class="dont-close-exam" style="position:absolute; background:rgb(210, 236, 254); color:rgb(7, 136,224); padding: 2px 8px 2px 6px; border-radius:10px; right:20px; top:14px;">
          ${{Intl.NumberFormat('de-DE').format(l.item.precio)}}
           </div>

         </li>
       </ul>

     </div>
    </div>








    <div v-if="focused_comuna"  style=" ertical-align:middle;" class="search-comuna-popup">
      <div v-if="resultado_fuse_comuna.length == 0 && comuna_buscado == ''" style="line-height:20px; vertical-align:middle;">
     <b-icon icon="search" style="height:40px; width:80px; padding-left:20px; padding-right:20px;" /> <div class="dont-close-comuna" style="height:60px; margin-left;40px; display:inline-block; vertical-align:middle;">Escribe para buscar tu comuna</div>
      <div v-on:click="getMyComuna()" class="location-button dont-close-comuna">
        <b-icon v-if="gettingComuna != 1" icon="cursor"  style="height:20px; width:40px;" /> 
        <div v-if="gettingComuna == 1" class="spinner-border spinner-border spinner-border-sm" style="margin-left:10px; margin-right:14px; margin-top:2px; margin-bottom:2px; color:grey;" role="status"></div>
           <span class="dont-close-comuna" v-if="gettingComuna == 0">Agregar comuna cercanas </span>
           <span class="dont-close-comuna" v-else-if="gettingComuna == 1"> Buscando comunas...</span>
           <span class="dont-close-comuna" v-else-if="gettingComuna == 2"> ¡Comuna agregada!</span>
           <span class="dont-close-comuna" v-else> ¡No se encontraron comunas!</span>

        </div>
      </div>
      <div v-else-if="resultado_fuse_comuna.length == 0 && comuna_buscado != ''" >
        
          <div style="padding-left:20px;" class="dont-close-comuna" >
            <b-icon icon="x-circle" style="height:40px;width:40px; display:inline-block;" /> <div style="transform:translate(5px,-12px); display:inline-block;">No encontramos comunas.</div>
          </div>
      </div>
      <div v-else>

       <ul style="padding-top:0px;">
    
          <div style="padding-left:20px; font-weight:700; font-size:14px; padding-top:10px; color:rgb(7, 136,224); display:inline-block;">Populares: 
        
      </div>
      <div v-on:click="onClickComuna(comunas_usadas.filter(x=>{return x.id == 51})[0])"  style="display:inline-block; cursor:pointer; color:rgb(7, 136,224); font-size:14px; margin:0px 4px; padding: 5px 10px; border-radius:5px; background:rgb(240, 240, 240);">Vitacura </div>
      <div v-on:click="onClickComuna(comunas_usadas.filter(x=>{return x.id == 52})[0])"  style="display:inline-block; cursor:pointer; color:rgb(7, 136,224); font-size:14px; margin:0px 4px; padding: 5px 10px; border-radius:5px; background:rgb(240, 240, 240);">Lo Barnechea </div>
      <div v-on:click="onClickComuna(comunas_usadas.filter(x=>{return x.id == 39})[0])"  style="display:inline-block; cursor:pointer; color:rgb(7, 136,224); font-size:14px; margin:0px 4px; padding: 5px 10px; border-radius:5px; background:rgb(240, 240, 240);">Las Condes </div>

         <li style="height:52px; position:relative; vertical-align:middle;" v-on:click="onClickComuna(l.item); $gtag.event('barrabusqueda-comuna', {'event_category': 'busqueda', 'event_label': l.item.Nombre, 'value': 1} );" v-for="l in resultado_fuse_comuna.slice(0, 6)" >
           <div class="dont-close-comuna" >
             <!--
          <div class="dont-close-comuna" style="display:inline-block; padding-left:10px; padding-top:10px; height:40px; width:30px; vertical-align:middle;"> <b-icon icon="cursor"  style="height:15px; width:15px;" /></div> 
           <div class="dont-close-comuna"  style="display:inline-block; height:40px; padding-top:10px; vertical-align:middle;">{{l.item.Nombre}} <b-icon v-if="selected_comunas.includes(l.item.id)" style="color:green;" icon="check" /> </div>
           -->
                       <b-icon style="position:absolute; top: 20px; left:20px; height:16px; width:16px; color:rgb(20, 134, 231);" icon="geo-alt-fill"/> 
           <div class="dont-close-exam" style="font-size: 14px; padding-top:6px; line-height:40px; display:inline-block; overflow: hidden; margin-left:50px;  text-overflow: ellipsis; max-width:430px;  white-space: nowrap;">{{l.item.Nombre}}</div>
          <!-- <div class="dont-close-comuna"  style="position:absolute;  padding:4px; border: 1px solid rgb(20, 134, 231); color:rgb(20, 134, 231); border-radius: 10px; right:10px; margin-top:-35px;">{{regiones.filter(x=>{return x.id == l.item.region_id})[0].Nombre}}</div>-->
           </div>
         </li>
       </ul>

     </div>
    </div>
    <div v-if="alerta && !(comuna != -1 && examenes_agregados.length > 0)" class="alertaa">
        ¡Debes agregar al menos una comuna y examen!
    </div>
    <!--
    <div v-if="(comuna != -1 && examenes_agregados.length > 0)" class="alertaa2">
        ¡Ya estás listo para agendar! <img style="height:30px; width:30px; transform:rotate(-55deg);" src="@/assets/icon/tap2.svg" alt="">
    </div>
-->
    <div class="tags-container">
      <span style="margin-right:4px;">Lo más buscado:</span>
      <div v-on:click="agregarExamenAction(examenes.filter(x=>{return x.id == 241})[0])" class="tag">PCR</div>      
      <div v-on:click="agregarExamenAction(examenes.filter(x=>{return x.id == 22})[0])" class="tag">Hemograma</div>
      <div v-on:click="agregarExamenAction(examenes.filter(x=>{return x.id == 242})[0])" class="tag">PCR Express</div>
   
      <div v-on:click="agregarExamenAction(examenes.filter(x=>{return x.id == 243})[0])" class="tag">Perfil Lipídico</div>

    </div>


    </div>
<div v-if="error_comuna" v-on:click="error_comuna = false" style="height:100vh; width:100vw; position:fixed; top: 0; left: 0; background-color:rgba(0,0,0,0.4); z-index:99998"></div>
    <div v-if="error_comuna" v-on:click="error_comuna = false"  class="comuna_error move">
      <img src="@/assets/icon/remove-location.svg" style="height:30px; width:30px; margin-bottom:10px; fill:#FF4136;" alt="">

      <div  v-on:click="error_comuna = false"  style="color:#FF4136;"> Comuna actualmente <b>no disponible</b>. Por favor, selecciona otra opción.</div>
    </div>

</div>
</template>

<script>

import {mapGetters, mapActions} from 'vuex';
import axios from 'axios'
import Fuse from 'fuse.js'
//import "bootstrap-icons/font/bootstrap-icons.css";

export default {
  name: 'VerPrestador',
  components: {
},
  data(){

    return{
        alerta:false,

        tipo_color:['#FF0000','#FFA500','#008000','#FFDC00','#800080','#00FFFF'],

        focused_exam:false,
        hovering_exam:false,
        
        focused_prev:false,
        hovering_prev:false,

        focused_comuna:false,
        hovering_comuna:false,

        examen_buscado:'',
        prevision_buscado:'',
        comuna_buscado:'',

        resultado_fuse_examen:[],
        fexamenes:null,

        resultado_fuse_insti:[],
        fInstituciones:null,

        resultado_fuse_comuna:[],
        fComunas:null,

        selected_comunas:[],
        selected_prevision:0,
        selected_examen:0,
        gettingComuna:0,
        error_comuna:false,

    }
  },
  destroyed() {

  },
  created () {
    

  },
  mounted(){
  
    this.fexamenes = new Fuse(this.examenes, {  keys: ['nombre', 'keywords']})
    this.fInstituciones = new Fuse(this.instituciones, {  keys: ['nombre', 'keywords']})
    this.fComunas = new Fuse(this.comunas, {  keys: ['Nombre']})

    document.addEventListener('click', this.close) 

  },
  beforeDestroy () {
    document.removeEventListener('click',this.close)
  },

  methods: {
    ...mapActions(['setCoords', 'clickearComunaAction', 'clickearExamenAction', 'agregarExamenAction', 'setNAction']),

  onEnterExamen(){
    if(this.resultado_fuse_examen.length > 0){
      this.clickearExamenAction(this.resultado_fuse_examen[0].item)
      if (this.comuna == -1)
        this.$refs.comuna.focus()
      else
        this.checkout()
    }
  },
  onEnterComuna(){
    if(this.resultado_fuse_comuna.length > 0){
      this.clickearComunaAction(this.resultado_fuse_comuna[0].item.id)
      if (this.examenes_agregados.length == 0)
        this.$refs.examen.focus()
      else
        this.checkout()
    }
  },
  checkout(){
    if(this.comuna != -1 && this.examenes_agregados.length>0){
      let n_personas = 0
      for (var z = 0; z < this.examenes_agregados.length; z++){
        if (this.examenes_agregados[z].cantidad > n_personas)
          n_personas = this.examenes_agregados[z].cantidad

      }
      this.setNAction(n_personas)
      this.$router.push({ name: 'FechaYHora' })
      }

  },
  async getMyComuna()
  {
    this.gettingComuna = 1;
    this.$getLocation({})
    .then(coordinates=>{
      this.setCoords(coordinates)
      
      axios.post(window.hostname+'api/getComunasCercanas',
      {lat:coordinates.lat,
      lon:coordinates.lng})
      .then(respuesta => {
        if (typeof(respuesta.data.resultados) == "object")
        {
            if(this.comunas_usadas.some(x=>{return x.id == respuesta.data.resultados[0].id}))
            {
              
              this.clickearComunaAction(respuesta.data.resultados[0].id)
              this.gettingComuna = 2;
              this.focused_comuna = false;
            }
            else
              this.gettingComuna = 3;
          
        }
      })
      .catch(error =>{
        this.gettingComuna = 4;
      }) 
      })
      .catch(error =>{
        this.gettingComuna = 4;
      })
    },

    close(e){
      if (!(e.target.classList.contains('search-exam-popup') || e.target.classList.contains('dont-close-exam') ) && this.focused_exam)
      {
        this.focused_exam = false;
        this.examen_buscado = ''
      }
      if (!(e.target.classList.contains('search-prev-popup') || e.target.classList.contains('dont-close-prev') ) && this.focused_prev)
        this.focused_prev = false;
      if (!(e.target.classList.contains('search-comuna-popup') || e.target.classList.contains('dont-close-comuna') ) && this.focused_comuna)
      {
        this.comuna_buscado = '';
        this.$nextTick(() => this.focused_comuna = false);
      }

    },

    focusInputExam() {
      this.selected_examen = 0;
      if(!this.focused_exam)
        this.focused_exam = true;
      this.focused_comuna = false;
      this.focused_prev = false;
       setTimeout(()=>{this.$refs.examen.focus(), 100});
    },
    focusInputPrev() {
      this.selected_prevision = 0;
      if(!this.focused_prev)
        this.focused_prev = true;
      this.focused_comuna = false;
      this.focused_exam = false;
      setTimeout(()=>{this.$refs.prev.focus(), 100});
    },
    focusInputComuna(e) {
      if (typeof e !== 'undefined'){
        if(e.target.classList.contains('dont-close-btn')){
          return;
          }
      }
        if(!this.focused_comuna)
          this.focused_comuna = true;
        this.focused_exam = false;
        this.focused_prev = false;
        setTimeout(()=>{this.$refs.comuna.focus(), 100});
      
    },
    onClickExam(l){

      /*
      this.examen_buscado = '' //l.Nombre
      this.selected_examen = l.id
      this.focusInputPrev();
      */
     this.clickearExamenAction(l)
     this.focused_exam = false;     
     
    },
    onClickInsti(l){
      this.selected_prevision = l.id
      this.prevision_buscado = ''//l.nombre
      this.focusInputComuna();
    },
    onClickComuna(l){
      /*
      if (this.selected_comunas.includes(l.id)){
        this.selected_comunas.splice(this.selected_comunas.indexOf(l.id), 1);
      }
      else{
        this.selected_comunas.push(l.id)
      }
      */
      if(this.comunas_usadas.some(x=>{return x.id == l.id}))
      {
        this.clickearComunaAction(l.id)
      }
      else{
        this.error_comuna = true;
      }
      this.comuna_buscado = ''
      this.$refs.comuna.blur()
      this.focused_comuna = false;
      //setTimeout(()=>{this.checkout()}, 500);

      //this.comuna_buscado = l.Nombre
    },

    onFocusExam(){},
      
  },
   computed: {
     ...mapGetters(['comunas', 'comuna', 'regiones', 'comunas_usadas', 'myCoords', 'hasCoords', 'examenes', 'examenes_agregados']),


  },
  watch: {
    focused_exam(val){
      if(val == false)
        //setTimeout(() => {this.examen_buscado = ''}, 100);
        this.examen_buscado = ''
    },
    focused_comuna(val){
      if(val == false)
        //setTimeout(() => {this.examen_buscado = ''}, 100);
        this.comuna_buscado = ''
    },
    examen_buscado(val){
      this.resultado_fuse_examen = this.fexamenes.search(val)
      /*
      if(!this.focused_exam)
        this.focused_exam = true;
        */
    },
    prevision_buscado(val){
      this.resultado_fuse_insti = this.fInstituciones.search(val)
      if(!this.focused_prev)
        this.focused_prev = true;
    },
    comuna_buscado(val){
      this.resultado_fuse_comuna = this.fComunas.search(val)
      //if(!this.focused_comuna)
        //this.focused_comuna = true;
    },

    

  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url(https://maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css);
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


.subido{

  font-size:10px;
  top:8px !important;
  left:64px;
  color:rgb(7,136,224);

}
.bajado{
  top:20px;
  left:64px;
  font-weight: 600;
  color:rgb(102, 102, 102);

}
.subido2{

  font-size:10px;
  top:8px !important;
  left:57px;
  color:rgb(7,136,224);

}
.bajado2{
  top:20px;
  left:54px;
  font-weight: 600;
  color:rgb(102, 102, 102);

}
.search-btn{
  position:absolute; 
  right:5px; 
  top: 5px; 
  border-radius:15px; 
  height:50px; 
  width:50px; 
  color:white; 
  outline:none; 
  border:none; 
  background:rgb(0, 191, 222);
  transition: width 0.1s linear .1s, background-color 0.1s linear 0.1s;  
}

.vl-exam{
  
  height: 38px;
  position: absolute;
  top:15px;
  right:0px;
  /*transition: border-left 0.01s linear 0.01s;*/
}

.vl-prev{
  border-left: 1px solid rgba(195,195,195,1);
  height: 30px;
  position: absolute;
  top:15px;
  left:70%;
  transition: left 0.1s linear .1s,
}

.location-button{
  background: rgb(237,237,237);
  width:290px;
  padding:5px;
  border: 1px solid rgb(247,247,247);
  border-radius: 15px;
  margin-left:20px;
  cursor: pointer;
}


.main-search-exam{
  padding-left:30px;
  padding-right:40px;
  height: Calc(100% - 2px);
  margin-top:1px;
  margin-left:3px;
  padding-top:10px;
  width:40%;
  border-radius: 10px;
  display: inline-block;
  cursor: pointer;
  transition: max-width 0.1s linear .1s, height 1s linear .5s;
  transition: width 0.1s linear .1s, height 1s linear .5s;
}

.main-search-exam input[type="text"]::placeholder, .main-search-comuna input[type="text"]::placeholder {

  color: rgb(200, 201, 201);

  opacity: 1;
}

.main-search-exam:hover{
  background: rgb(235,235,235);
}
.main-search-exam:hover input[type=text]{
  background: rgb(235,235,235);
}

.search-prev-popup{
  top:70px;
  background: white;
  width:440px;
  left: 250px;
  padding-top:20px;
  padding-bottom: 20px;
  border-radius: 10px;
  position:absolute;
  z-index: 9999;
  border: 1px solid rgba(200,200,200,1);
}

.search-comuna-popup{
  top:75px;
  right:0px;
  background: white;
  width:420px;
  padding-top:20px;
  padding-bottom: 20px;
  border-radius: 25px;
  position:absolute;
  z-index: 999;
  border: 1px solid rgba(200,200,200,1);
}

.search-exam-popup{
  top:75px;
  background: white;
  width:550px;
  padding-top:20px;
  padding-bottom: 20px;
  border-radius: 15px;
  position:absolute;
  z-index: 9999;
  border: 1px solid rgba(200,200,200,1);
}

.search-exam-popup ul, .search-prev-popup ul, .search-comuna-popup  ul{
  list-style-type: none;
  padding:0;
  margin:0;
}

.search-comuna-popup li{
  list-style-type: none;

  margin:0;
  padding-left:0px;
  font-size: 14px;
  cursor: pointer;
}

.search-prev-popup li{
  list-style-type: none;

  margin:0;
  padding-left:20px;
  font-size: 14px;
  cursor: pointer;
}
.search-exam-popup li {
  list-style-type: none;
  padding:10px 0px;
  margin:0;
  font-size: 14px;
  cursor: pointer;
}
.search-prev-popup li:hover{
  background: rgb(247,247,247);
}
.search-exam-popup li:hover{
  background: rgb(247,247,247);
}

.search-comuna-popup li:hover{
  background: rgb(247,247,247);
}


.main-search-comuna{
  padding-left:25px;
  padding-right:40px;
  height: Calc(100% - 2px);
  margin-top:1px;
  padding-top:10px;
  width:30%;
  border-radius: 15px;
  display: inline-block;
  cursor: pointer;
  transition: width 0.1s linear .1s, height 1s linear .5s;
}
.main-search-comuna:hover{

  background: rgb(235,235,235);

}
.main-search-comuna:hover input[type=text]{

}


.main-search-prev{
  padding-left:25px;
  padding-right:40px;
  height: Calc(100% - 2px);
  margin-top:1px;
  padding-top:10px;
  width:30%;
  border-radius: 25px;
  display: inline-block;
  cursor: pointer;
}
.main-search-prev:hover{
  background: rgb(235,235,235);
}
.main-search-prev:hover input[type=text]{
  background: rgb(235,235,235);
}

.main-search input[type=text]{
  outline: none;
  font-size:14px;
  line-height:14px;
  border: 0px solid white;
  background:none;
  width:100%;

}
.main-search-title{
  font-size: 12px;
  font-weight: 700;
  text-align: left;

}

.main-search-container{
  padding:0px;
}
.main-search{
  position: relative;
  background: white;
  text-align: left;
  width:800px;
  height:70px;
  border-radius: 15px;
  margin: 0 auto;
  border: 0.5px solid rgba(0,0,0,0.15);
  /*
  box-shadow: 2px 3px 20px -1px rgba(0,0,0,0.81);
-webkit-box-shadow: 2px 3px 20px -1px rgba(0,0,0,0.81);
-moz-box-shadow: 2px 3px 20px -1px rgba(0,0,0,0.81);
border: 1px solid rgba(0,0,0,0.21)
*/

}
.main-image{
  /*min-height: ;*/
   background-repeat: no-repeat;
  background-size: cover;
  height: Calc(100vh - 160px);
}

@media only screen and (max-width: 1000px) {


}
.tags-container{
  color:white;
  display: inline-block;
  margin-top:16px;
  margin-left:10px;
}
.tag{
  display: inline-block;
  border: 1px solid white;
  border-radius: 7px;
  padding: 5px 10px;
  font-size:14px;
  margin-left: 10px;
  font-weight: 500;
  cursor: pointer;
  transition: 0.2s;
}
.tag:hover{
  display: inline-block;
  border: 1px solid white;
  background-color: white;
  border-radius: 10px;
  padding: 0px 10px;
  margin-left: 10px;
  color:black;
}

.alertaa{
  position:absolute;
  right:5px;
  top:-79px;
  padding:10px;
  background:rgb(20, 134, 231);
  color:white;
  z-index:10001; 
  height:68px; 
  width:220px; 
  border-radius:10px; 
  border:2px solid rgb(15, 100, 200);
  font-weight: 700;
}
.alertaa:before{
  content:'';
  position:absolute;
  top:59px;
  transform: rotate(-135deg);
  right:14px;
  height:12px;
  width:12px;
  background: rgb(20, 134, 231);
  border-top: 2px solid rgb(15, 100, 200);
  border-left: 2px solid rgb(15, 100, 200);

  
}
.alertaa2{
  position:absolute;
  right:5px;
  top:-79px;
  padding:10px;
  background:rgb(20, 134, 231);
  color:white;
  z-index:10001; 
  height:68px; 
  width:170px; 
  border-radius:10px; 
  border:2px solid rgb(15, 100, 200);
  font-weight: 700;
}
.alertaa2:before{
  content:'';
  position:absolute;
  top:59px;
  transform: rotate(-135deg);
  right:14px;
  height:12px;
  width:12px;
  background: rgb(20, 134, 231);
  border-top: 2px solid rgb(15, 100, 200);
  border-left: 2px solid rgb(15, 100, 200);

  
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
    box-shadow: 0 0 10px #fff, 0 0 20px #fff, 0 0 30px rgb(255, 200, 9), 0 0 40px rgb(255, 200, 9), 0 0 50px rgb(255, 200, 9), 0 0 60px rgb(255, 200, 9), 0 0 70px rgb(255, 200, 9);
  }
  to {
    box-shadow: 0 0 20px #fff, 0 0 30px rgb(255, 200, 9), 0 0 40px rgb(255, 200, 9), 0 0 50px rgb(255, 200, 9), 0 0 60px rgb(255, 200, 9), 0 0 70px rgb(255, 200, 9), 0 0 80px rgb(255, 200, 9) !;
  }
}

</style>
