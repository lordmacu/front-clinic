<template>
  <div>
    <div class="footer-dark">
      <footer>
        <div class="container">
          <div class="row">
            <div class="col-md-12 item text">
              <p class="mission-text pr-3 pl-3">
                Nuestra misión en
                <b>Clinic Go</b> es acercar la salud a las personas, mediante la
                toma de muestras médicas a domicilio y el envío de resultados de
                manera <b>100% digital</b>, siempre con precios trasparentes y
                justos.
              </p>
            </div>
            <div class="col-12 col-lg-4">
              <h2>Contáctanos</h2>
              <div class="col item social mt-3">
                <a href="#"><b-icon icon="facebook" /></a>
                <a href="https://www.instagram.com/clinicgocl/"
                  ><b-icon icon="instagram"
                /></a>
                <a href="https://www.instagram.com/clinicgocl/"
                  ><b-icon icon="instagram"
                /></a>
              </div>
            </div>

            <div class="col-12 col-lg-4">
              <h2>Acerca de</h2>
              <div class="col mt-3">
                <ul class="simple-li">
                  <li><a href="#comofunciona">¿Cómo funciona?</a></li>
                  <li><a href="#comunas">Comunas que cubrimos</a></li>
                </ul>
              </div>
            </div>

            <div class="col-12 col-lg-4">
              <h2>Legal</h2>
              <div class="col mt-3">
                <ul class="simple-li">
                  <li
                    style="cursor: pointer"
                    class="tyc"
                    v-on:click="viendoTerminos = true"
                  >
                    <a class="tyc">Terminos y Condiciones</a>
                  </li>
                  <li
                    style="cursor: pointer"
                    class="pdp"
                    v-on:click="viendoPoliticas = true"
                  >
                    <a class="pdp">Politicas de Privacidad</a>
                  </li>
                </ul>
              </div>
            </div>
          </div>
          <p class="copyright">ClinicGo © 2021</p>
        </div>
      </footer>
    </div>

    <div v-if="viendoTerminos">
      <div
        style="
          position: fixed;
          background-color: black;
          height: 100vh;
          width: 100vw;
          top: 0px;
          z-index: 10000;
          opacity: 0.5;
        "
      ></div>
      <div
        style="
          position: fixed;
          height: Calc(100vh - 120px);
          overflow-y: scroll;
          padding: 10px;
          background: white;
          top: 20px;
          width: 800px;
          max-width: 90%;
          margin: 0px auto;
          left: 0;
          right: 0;
          opacity: 1;
          z-index: 10001;
        "
      >
        <div class="tyc">
          <TyC class="tyc" />
          <button
            class="btn btn-dark"
            style="margin: 20px"
            v-on:click="viendoTerminos = false"
          >
            Entendido
          </button>
        </div>
      </div>
    </div>

    <div v-if="viendoPoliticas">
      <div
        style="
          position: fixed;
          background-color: black;
          height: 100vh;
          width: 100vw;
          top: 0px;
          z-index: 10000;
          opacity: 0.5;
        "
      ></div>
      <div
        style="
          position: fixed;
          height: Calc(100vh - 120px);
          overflow-y: scroll;
          padding: 10px;
          background: white;
          top: 20px;
          width: 800px;
          max-width: 90%;
          margin: 0px auto;
          left: 0;
          right: 0;
          opacity: 1;
          z-index: 10001;
        "
      >
        <div class="pdp">
          <PdP class="pdp" />
          <button
            class="btn btn-dark"
            style="margin: 20px"
            v-on:click="viendoPoliticas = false"
          >
            Entendido
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TyC from "@/components/TerminosYCondiciones.vue";
import PdP from "@/components/PoliticasDePrivacidad.vue";

export default {
  name: "Footer",
  components: {
    TyC,
    PdP,
  },
  data() {
    return {
      viendoTerminos: false,
      viendoPoliticas: false,
    };
  },
  created() {
    document.addEventListener("click", this.close);
  },
  destroyed() {},
  beforeDestroy() {
    document.removeEventListener("click", this.close);
  },
  methods: {
    close(e) {
      if (!e.target.classList.contains("tyc") && this.viendoTerminos)
        this.viendoTerminos = false;
      if (!e.target.classList.contains("pdp") && this.viendoPoliticas)
        this.viendoPoliticas = false;
    },
  },
  computed: {},
};
</script>
<style scoped>
.mission-text {
  color: white;
  opacity: 1 !important;
  padding-left: 150px;
  padding-right: 150px;
  font-size: 17px;
  padding-bottom: 60px !important;
  border-bottom: 1px solid white;
}

/* Smartphones (portrait) ----------- */
@media only screen and (max-width: 320px) {
  .mission-text {
    padding-left: 30px;
    padding-right: 30px;
  }
}

.footer-dark {
  padding: 70px 0;
  color: #f0f9ff;
  background-color: #003e6a;
}

.simple-li li {
  text-align: left;
  margin-bottom: 10px;
}
.simple-li li a {
  font-size: 17px;
  color: white;
  opacity: 1 !important;
}

.simple-li {
  width: 180px;
  margin: auto;
}
.footer-dark h3 {
  margin-top: 0;
  margin-bottom: 12px;
  font-weight: bold;
  font-size: 16px;
}

.footer-dark ul {
  padding: 0;
  list-style: none;
  line-height: 1.6;
  font-size: 14px;
  margin-bottom: 0;
}

.footer-dark ul a {
  color: inherit;
  text-decoration: none;
  opacity: 0.6;
}

.footer-dark ul a:hover {
  opacity: 0.8;
}

@media (max-width: 767px) {
  .footer-dark .item:not(.social) {
    text-align: center;
    padding-bottom: 20px;
  }
}

.footer-dark .item.text {
  margin-bottom: 36px;
}

@media (max-width: 767px) {
  .footer-dark .item.text {
    margin-bottom: 0;
  }
}

.footer-dark .item.text p {
  opacity: 0.6;
  margin-bottom: 0;
}

.footer-dark .item.social {
  text-align: center;
}

@media (max-width: 991px) {
  .footer-dark .item.social {
    text-align: center;
    margin-top: 20px;
  }
}

.footer-dark .item.social > a {
  font-size: 20px;
  width: 60px;
  height: 60px;
  line-height: 36px;
  display: inline-block;
  text-align: center;
  border-radius: 20px;
  box-shadow: 0 0 0 1px rgb(255 255 255 / 40%);
  margin: 0 8px;
  color: #fff;

  background: #e4f4ff;
}

.footer-dark svg {
  width: 35px;
  height: 35px;
  margin-top: 12px;
  color: #0085e4;
}

.footer-dark .item.social > a:hover {
  opacity: 0.9;
}

.footer-dark .copyright {
  text-align: center;
  padding-top: 24px;
  opacity: 0.3;
  font-size: 17px;
  margin-bottom: 0;
  opacity: 1;
  font-weight: bold;
  margin-top: 30px;
}
</style>
