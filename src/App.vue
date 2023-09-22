<script setup>
import {ref, reactive,onMounted,watch} from 'vue'
import {db} from './data/guitarras'
import Guitarra from './components/Guitarra.vue'
import Header from './components/Headerr.vue'
import Footer from './components/Footer.vue'

const guitarras=ref([])
const carrito=ref([])
const guitarra=ref({})


watch(carrito,()=>{

guardarCarrito()

},{
    deep:true
})
const state=reactive({
  guitarras:[]
})


onMounted(()=>{
    guitarras.value=db;
    state.guitarras=db
    guitarra.value=db[3]


    const guardarCarrito2=localStorage.getItem('carrito')
    if(guardarCarrito2){
        carrito.value=JSON.parse(guardarCarrito2)
    }
})


const guardarCarrito=()=>{

    localStorage.setItem('carrito',JSON.stringify(carrito.value))

}


const agregarCarrito=(guitarra)=>{
    const existeCarrito=carrito.value.findIndex(producto=>producto.id===guitarra.id)

    if(existeCarrito>=0){

        carrito.value[existeCarrito].cantidad++

    }else{
        guitarra.cantidad=1;
   carrito.value.push(guitarra)
    }
    guardarCarrito()
   

}

const decrementarCantidad=(id)=>{
    const index=carrito.value.findIndex(producto=>producto.id===id)
    if (carrito.value[index].cantidad<=1) return
    carrito.value[index].cantidad--
console.log(id)

}

const incrementarCantidad=(id)=>{
    const index=carrito.value.findIndex(producto=>producto.id===id)
    if (carrito.value[index].cantidad>=5) return
    carrito.value[index].cantidad++
console.log(id)
    
}

const eliminarProducto= id=>{

    carrito.value=carrito.value.filter(producto=>producto.id !== id)
}

const vaciarCarrito=()=>{

carrito.value=[]

}

const incrementar =()=>{
    numero.value++
}


</script>

<template>
  

    <Header
    :carrito="carrito"
    :guitarra="guitarra"
    @incrementar-cantidad="incrementarCantidad"
    @decrementar-cantidad="decrementarCantidad"
    @agregar-carrito="agregarCarrito"
    @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarrito"
    
 
    />


    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <Guitarra 
            v-for="guitarra in guitarras" 
            v-bind:guitarra="guitarra"
            @agregar-carrito="agregarCarrito"
            />

        </div>
    </main> 


  <Footer
  
  
  
  
  />


</template>

