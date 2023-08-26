<template>
    <div class="container">
        <h1 class="minimarket">Minimarket La 5ta</h1>
        <h1>Tabla de Productos</h1>
        <table>
            <thead>
                <tr>
                    <th>Código</th>
                    <th>Nombre</th>
                    <th>Cantidad</th>
                    <th>Precio</th>
                    
                </tr>
            </thead>
            <tbody>
              
                    <tr v-for="producto in productos" :key="producto.codigo">
                    <td>{{ producto.codigo }}</td>
                    <td>{{ producto.nombre }}</td>
                    <td>{{ producto.cantidad }}</td>
                    <td>{{ producto.precio }}</td>
                </tr>
                   
                <router-view />
            
              
            </tbody>
        </table>
    </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      productos: [],
    };
  },
  methods: {
    obtenerProductos() {
      // Método para obtener la lista de todos los productos
      axios.get("http://localhost:8080/api/productos/listar")
      .then((response) => {
        this.productos = response.data;
        console.log("esta es mi data", response.data)
      })
      .catch((error) => {
        console.error("Error al obtener productos:", error);
      });
    },
  },
  mounted() {
    // Llamar al método para obtener la lista de productos al cargar el componente
    this.obtenerProductos();
  },
};
</script>
