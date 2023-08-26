<template>
  <div class="container">
    <h1>Formulario de Productos</h1>
    <form id="student-form" @submit.prevent="guardar">
      <div class="form-group">
        <label for="codigo">Código:</label>
        <input type="text" id="codigo" name="codigo" required  v-model="codigo" />
      </div>
      <div class="form-group">
        <label for="nombre">Nombre:</label>
        <input type="text" id="nombre" name="nombre" required v-model="nombre"/>
      </div>
      <div class="form-group">
        <label for="cantidad">Cantidad:</label>
        <input type="text" id="cantidad" name="cantidad" required v-model="cantidad"/>
      </div>
      <div class="form-group">
        <label for="precio">Precio:</label>
        <input type="number" id="precio" name="precio" required v-model="precio" />
      </div>

      <button type="submit" id="guardar" name="guardar">Guardar</button><br />
      <button type="button" id="eliminar" name="eliminar" @click="eliminar"> Eliminar</button ><br />
      <button type="button" id="actualizar" name="actualizar"  @click="actualizar"> Actualizar</button><br />
      <button type="button" id="consultar" name="consultar" @click="consultar"> Consultar</button><br />
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  
  data() {

    return {
      codigo: "",
      nombre: "",
      cantidad: "",
      precio: null,
    };

  },

  methods: {
    guardar() {
      
      axios
        .post("http://localhost:8080/api/productos", {
          codigo: this.codigo,
          nombre: this.nombre,
          cantidad: this.cantidad,
          precio: this.precio,
        })
        .then((response) => {
          console.log("Producto registrado con éxito:", response.data);
          alert("Producto creado con éxito");
          this.codigo = '';
        this.nombre = '';
        this.cantidad = '';
        this.precio = '';
         
        })
        .catch((error) => {
          console.error("Error al registrar producto:", error);
        });
    },
    
    consultar() {
      
      axios
        .get('http://localhost:8080/api/productos/'+this.codigo)
        .then((response) => {
          // Actualizar los campos del formulario con los datos del producto consultado
          this.nombre = response.data.nombre;
          this.cantidad = response.data.cantidad;
          this.precio = response.data.precio;
        })
        .catch((error) => {
          console.error("Error al consultar producto:", error);
        });
    },


    actualizar() {
      
      axios
        .put("http://localhost:8080/api/productos/actualizar/"+this.codigo, {
          codigo:this.codigo,
          nombre: this.nombre,
          cantidad: this.cantidad,
          precio: this.precio,
        })
        .then((response) => {
          console.log("Producto actualizado con éxito:", response.data);
          alert("Producto actualizado con éxito")
        })
        .catch((error) => {
          console.error("Error al actualizar producto:", error);
        });
    },
    eliminar() {
     
      axios
        .delete("http://localhost:8080/api/productos/"+this.codigo)
        .then(() => {
          console.log("Producto eliminado con éxito");
          alert("Producto eliminado con éxito")
          // Limpiar los campos del formulario después de eliminar
          this.codigo = "";
          this.nombre = "";
          this.cantidad = "";
          this.precio = null;
        })
        .catch((error) => {
          console.error("Error al eliminar producto:", error);
        });
    },
  },
};
</script>
