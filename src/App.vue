<template>
  <div class="caja">
    <div class="barra">
      <h1>Tienda virtual de ciclismo </h1>
      <a href="#" class="bx bxs-cart-alt" @click="mostrarModal = true">
        <span class="carrito-cantidad">{{ carrito.length }}</span>
      </a>
    </div>
    <div class="titulo">
      <input type="text" placeholder="Busca lo que necesites" class=" buscar">
      <img src="https://www.santandercycling.com/cdn/shop/files/la_mejor_ruta_de_santander_1.gif?v=1683680293&width=3840" alt="">
    </div>
    <div class="port"></div>
    <div class="productos">
      <div class="box-contaier">
        <div v-for="(producto, index) in listaProductos" :key="index" class="box">
          <div class="imagen">
            <img :src="producto.imagenSrc" alt="" />
          </div>
          <div class="content">
            <div class="strellas">
              <i class="bx bxs-star" v-for="star in producto.estrellas" :key="star"></i>
              <i class="bx bxs-star-half"></i>
            </div>
            <h3>{{ producto.nombre }}</h3>
            <p>{{ producto.descripcion }}</p>
            <span>{{ formatoPesoColombiano(producto.precio) }}</span>
          </div>
          <button class="btn-pe" @click="agregarAlCarrito(producto)">
            Agregar
          </button>
        </div>
      </div>
    </div>
    <div v-if="mostrarModal" class="modal">
      <div class="modal-content">
        <h2 class="carrito-titulo">Carrito de Compras</h2>
        <div class="carrito-lista" >
          <div v-for="(item, index) in carrito" :key="index" class="carrito-item">
            <span><img :src="item.producto.imagenSrc" /><span class="nombre">{{
              item.producto.nombre
            }}</span>
              <span class="cantidad">{{ item.cantidad }}</span>
              <span class="precio">${{ item.producto.precio }}</span></span>
            <button class="eliminar" @click="eliminarDelCarrito(index)">❌</button>
          </div>
        </div>
        <p class="carrito-total">Total compra: {{ formatoPesoColombiano(calcularTotal()) }}</p>
        <div class="bot">
          <button class="cerrar-modal" @click="mostrarModal = false">Cerrar</button>
          <button class="vaciar" @click="vaciarCarrito">
            Vaciar carrito
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import "boxicons/css/boxicons.min.css";

let carrito = ref([]);
let mostrarModal = ref(false);

let listaProductos = ref([
  {
    imagenSrc: "https://spirobicycles.com/cdn/shop/products/FredoomLtwoo11Velocidades_4_39c777fb-68a8-4c21-9819-c1818878319e.jpg",
    estrellas: [1, 2, 3, 4],
    nombre: "Bicicleta Spiro negra",
    precio: 5000000,
  },
  {
    imagenSrc: "https://www.ciclismoafondo.es/uploads/s1/12/20/44/1/5c7d4b950de6941e51349429-casco-specialized-s-works-evade-w-angi-prueba.jpeg",
    estrellas: [1, 2, 3],
    nombre: "Casco Sworks evade",
    precio: 700000,
  },
  {
    imagenSrc: "https://exitocol.vtexassets.com/arquivos/ids/724406-800-auto?v=637042878212430000&width=800&height=auto&aspect=true",
    estrellas: [1, 2, 3, 4],
    nombre: "Medias Safari colombia",
    precio: 20000,
  },
  {
    imagenSrc: "https://http2.mlstatic.com/D_NQ_NP_775657-MCO69546633165_052023-O.webp",
    estrellas: [1, 2, 3, 4],
    nombre: "Sillin Pro stealth",
    precio: 250000,
  },
  {
    imagenSrc: "https://deportivosnomadas.com/3017-large_default/jersey-ciclismo-colombia-olimpico-azul.jpg",
    estrellas: [1, 2, 3, 4],
    nombre: "Jersey Colombia ",
    precio: 100000,
  },
  {
    imagenSrc: "https://noticiclismo.com/wp-content/uploads/2020/06/Zapatillas-S-Work-Vent-3.jpg",
    estrellas: [1, 2, 3, 4],
    nombre: "Zapatillas Sworks",
    precio: 1000000,
  },
  {
    imagenSrc: "https://megabikes.co/1393-large_megabikes/pedales-shimano-ultegra-pd-r8000.jpg",
    estrellas: [1, 2, 3, 4],
    nombre: "Pedales Shimano ",
    precio: 350000,
  },
  {
    imagenSrc: "https://i.ebayimg.com/thumbs/images/g/jkkAAOSwIWFkhpW0/s-l640.jpg",
    estrellas: [1, 2, 3, 4],
    nombre: "Bielas Sram Force",
    precio: 800000,
  },
  {
    imagenSrc: "https://ciclospecial.com/wp-content/uploads/2022/02/CARAMA%C3%91OLA-FLY.jpg",
    estrellas: [1, 2, 3, 4],
    nombre: "Caramañola Elite",
    precio: 20000,
  },
  {
    imagenSrc: "https://www.tradeinn.com/f/13775/137754966/campagnolo-par-ruedas-carretera-bora-wto-60-2-way-fit-carbon-disc-tubeless.jpg",
    estrellas: [1, 2, 3, 4],
    nombre: "Ruedas Campagnolo",
    precio: 3500000,
  },
  {
    imagenSrc: "https://img.redbull.com/images/c_limit,w_1500,h_1000,f_auto,q_auto/redbullcom/2021/5/27/vyj8s9gzsshtgrlptqtj/cross-country-racing-mountain-bike",
    estrellas: [1, 2, 3, 4],
    nombre: "Bicicleta Cannyon ",
    precio: 34000000,
  },
  {
    imagenSrc: "https://savabike.co/images/virtuemart/product/coraza-continental-ultrasport-ii-23c-25c-28c-ruta.jpg",
    estrellas: [1, 2, 3, 4],
    nombre: "Corazas Continental",
    precio: 150000,
  },
])

const formatoPesoColombiano = (valor) => {
  return valor.toLocaleString('es-CO', {
    style: 'currency',
    currency: 'COP'
  });
}

const agregarAlCarrito = (producto) => {
  let itemEnCarrito = carrito.value.find(
    (item) => item.producto === producto
  );
  if (itemEnCarrito) {
    itemEnCarrito.cantidad++;
  } else {
    carrito.value.push({ producto: producto, cantidad: 1 });
  }
}

const eliminarDelCarrito = (index) => {
  carrito.value.splice(index, 1);
}

const vaciarCarrito = () => {
  carrito.value = [];
}

const calcularTotal = () => {
  return carrito.value.reduce(
    (total, item) => total + item.producto.precio * item.cantidad,
    0
  );
}

</script> 

<style>
* {
  margin: 0%;
  padding: 0%;
  box-sizing: border-box;
  font-family: 'Permanent Marker', cursive;
  outline: none;
  border: none;
  text-decoration: none;
  transition: all 0.2s linear;
}

body {
  background-image: url("");
  background: linear-gradient(90deg,rgb(44, 43, 43),rgb(155, 56, 56)) ;
  object-fit: cover;
  position: relative;
  text-align: center;
  background-size: cover;
  height: 400px;
}


.cantidad {
  margin-left: 44px;
}

.buscar{
  position: absolute;
  left: 25%;
  background: #fffdfd8a;
  bottom: 50%;
  justify-content: center;
  width: 50%;
  padding: 20px;
  border: 7px;
  color: rgb(255, 0, 0);
}

a {
  text-decoration: none;
}

.precio {
  margin-left: 50px;
}



.nombre {
  margin-left: 30px;
}

.barra {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: rgba(255, 255, 255, 0.767);
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.titulo {
  position: relative;
  margin-top: 15px;
}

.titulo img {
  height: 600px;
  background-size: contain;
  width: 99%;
}

.barra h1 {
  font-size: 40px;
  font-weight: bold;
  color: #020100;
  margin-left: 42.5%;
}

.barra a {
  display: flex;
  align-items: center;
  color: #ff0000;
  font-size: 24px;
  background-color: #ffffff;
}


.productos {
  margin: 5rem;
}

.box-contaier {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(20rem, 1fr));
  gap: 1.4rem;
}

.box-contaier .box {
  padding: 2.5rem;
  background: #c7c4c4c0;
  border-radius: 0.5rem;
  border: 0.1rem solid rgba(255, 0, 0, 0.233);
  box-shadow: var(--box-shadow);
  position: relative;
  overflow: hidden;
  text-align: center;
}

@import url('https://fonts.googleapis.com/css2?family=Phudu:wght@300&display=swap');

.box-contaier .box img {
  display: flex;
  width: 100%;
  justify-content: center;
  height: 17rem;
}

.box-contaier .box h3 {
  color: #000000;
  font-weight: bold;
  font-size: 2rem;
  margin: 0.5rem;
}

.box-contaier .box .strellas {
  padding: 0.8rem;
}

.box-contaier .box .strellas i {
  font-size: 1.2rem;
  color: #fbff00a4;
}

.btn-pe {
  margin-top: 1rem;
  display: inline-block;
  font-size: 1.7rem;
  color: #000000;
  background: #858586a6;
  border-radius: 0.5rem;
  cursor: pointer;
  padding: 0.8rem 2rem;
}

.box-contaier .box span {
  color: #000000;
  font-weight: bolder;
  font-size: 2.1rem;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(129, 128, 128, 0.363);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
}

.modal-content {
  background-color: rgba(211, 208, 208, 0.836);
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(66, 65, 65, 0.521);
  width: 70%;
  max-width: 600px;
  overflow-y: auto;
}

.detalle-producto {
  display: flex;
  align-items: center;
  border-bottom: 1px solid #ccc;
}

.detalle-producto img {
  max-width: 60px;
  margin-right: 10px;
}

.detalle-info h3 {
  margin: 0;
}

.detalle-info p {
  margin: 0;
}

.carrito-titulo {
  padding-bottom: 10px;
  margin-bottom: 10px;
  font-style: italic;
  color: #000000;
  text-align: center;
  font-size: 2rem;
  font-weight: bold;
}

.modal-content .carrito-item span img {
  width: 100px;
  height: 60px;
  object-fit: cover;
  border-radius: 7px;
}

.carrito-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid #000000;
  padding: 10px;
}

.carrito-producto {
  display: flex;
  align-items: center;
}

.vaciar {
  margin-top: 1rem;
  display: inline-block;
  font-size: 1.3rem;
  color: #131212;
  background-color: #ff0000;
  border-radius: 0.5rem;
  cursor: pointer;
  padding: 0.8rem 1rem;
  background-color: #ff0000;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  margin-left: 200px;


}

.carrito-cantidad {
  flex: 1;
  text-align: center;
}

.carrito-eliminar {
  margin-left: 10px;
}

.carrito-total {
  font-weight: bold;
  margin-top: 10px;
}

.eliminar {
  margin-top: 1rem;
  display: inline-block;
  font-size: 1rem;
  color: #1a1918;
  background: #0000003f;
  border-radius: 0.5rem;
  cursor: pointer;
  padding: 0.5rem .98rem;
}

.cerrar-modal {
  margin-top: 1rem;
  display: inline-block;
  font-size: 1.3rem;
  color: #131212;
  background: #3b3b3a3f;
  border-radius: 0.5rem;
  cursor: pointer;
  padding: 0.8rem 2rem;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  max-width: 600px;
  width: 100%;
}

.carrito-lista {
  display: flex;
  flex-direction: column;
}

.carrito-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
}

.producto-imagen {
  width: 50px;
  height: 50px;
  object-fit: cover;
  margin-right: 303px;
}

.producto-detalle {
  flex: 1;
  margin-left: -14px;
}

.carrito-total {
  text-align: right;
  font-weight: bold;
  margin-top: 10px;
}

.carrito-titulo {
  font-size: 24px;
  margin-bottom: 10px;
}

.cerrar-modal {
  background-color: #ff0000;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

.eliminar {
  background-color: #ff0d0057;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 50%;
  cursor: pointer;
}


.carrito-lista {
  max-height: 300px;
  overflow-y: auto;
}
</style>