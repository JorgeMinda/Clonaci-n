<template>
        <HeaderView/>
        <aside class="sidebar">
<nav>
    <ul>
        <li>
            <router-link to="/dashboard" class="active">
                <i class="fa-solid fa-chart-simple"></i> Dashboard
            </router-link>
        </li>
        <li>
            <router-link to="/menus">
                <i class="fa-solid fa-envelope"></i> Menu
            </router-link>
        </li>
        <li>
            <router-link to="/visualizar">
            <i class="fa-solid fa-table-cells-large"></i> Visualizar
            </router-link>
        </li>
        <li>
            <router-link to="/empleados">
                <i class="fa-solid fa-person"></i> Empleados
            </router-link>
        </li>
        <li>
            <router-link to="/pagos">
                <i class="fa-regular fa-credit-card"></i> Métodos de pago
            </router-link>
        </li>
        <li>
            <router-link to="/proveedores">
                <i class="fa-solid fa-user-group"></i> Proveedores
            </router-link>
        </li>
        <li>
            <router-link to="/inventario">
                <i class="fa-solid fa-file-invoice-dollar"></i> Inventario
            </router-link>
        </li>
        </ul>
    </nav>
</aside>
<!-- ! Termina el SIDEBAR -->
<div class="content">
    <div class="top">
      <h2>Proveedores</h2>
      <div class="superior">
        <router-link to="/create">
          <button>Agregar Proveedor</button>
        </router-link>
      </div>
    </div>
    <div class="table-container">
      <table>
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Contacto</th>
            <th>Email</th>
            <th>Dirección</th>
            <th>Ciudad</th>
            <th>Producto</th>
            <th>Estado</th>
            <th>Acciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="proveedor in proveedor.lista" :key="proveedor.idProveedores">
            <td>{{ proveedor.nombreProveedor }}</td>
            <td>{{ proveedor.numContacto }}</td>
            <td>{{ proveedor.emailContacto }}</td>
            <td>{{ proveedor.direccion }}</td>
            <td>{{ proveedor.ciudad }}</td>
            <td>{{ proveedor.tipoProducto }}</td>
            <td>{{ proveedor.estado }}</td>
            <td class="actions">
              <router-link :to="{ name: 'UpdateProveedores', params: { idProveedores: proveedor.idProveedores }}">
                <i class="fa-solid fa-pen-to-square"></i>
              </router-link>
              <i class="fas fa-trash-alt" @click="deleteProveedor(proveedor.idProveedores)"></i>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>


<script>
import HeaderView from '@/components/header/HeaderView.vue';
import axios from 'axios';

export default {
  name: 'ProveedoresView',
  components: {
    HeaderView
  },
  data() {
    return {
      proveedor: {
        lista: [],
        form: {
          idProveedores: null,
          nombreProveedor: '',
          numContacto: '',
          emailContacto: '',
          direccion: '',
          ciudad: '',
          tipoProducto: '',
          estado: ''
        }
      }
    };
  },
  created() {
    this.getProveedores();
  },
  methods: {
    async getProveedores() {
      try {
        const response = await axios.get('http://localhost:4200/proveedores');
        console.log(response.data);
        this.proveedor.lista = response.data;
      } catch (error) {
        console.error('Error al obtener proveedores:', error);
      }
    },
    async deleteProveedor(id) {
      try {
        await axios.delete(`http://localhost:4200/proveedores/${id}`);
        this.proveedor.lista = this.proveedor.lista.filter(proveedor => proveedor.idProveedores !== id);
        console.log('Proveedor eliminado correctamente');
      } catch (error) {
        console.error('Error al eliminar proveedor:', error);
      }
    }
  }
};
</script>


<style scoped>
.imgP{
    width: 50px;
}
.content {
    margin-left: 50px;
    padding: 20px;
    width: calc(100% - 270px);
}
.superior {
    display: flex;
    margin-top: 10px;
    width: 90%;
    height: 10%;
    align-items: center;
    justify-content: space-between; /* Distribuye los elementos horizontalmente */
    padding: 0 30px;
    box-sizing: border-box; /* Incluye el padding en el ancho total */
}
.superior h1{
    font-weight: bold;
    font-size: 50px;
}
.superior button{
    background-color: #FF7A00;
    border: none;
    border-radius: 20px;
    height: 30px;
    padding: 0 60px;
    margin-left: 740px;
    color: white;
    white-space: nowrap;
}
.top { 
    background-color: #75787b;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    margin-top: 100px;
    margin-left: 19px;
    width: 1200px;
    height: 80px;
    justify-content: space-between;
}
.header h2 {
    font-size: 20px;
    }
.table-container {
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    overflow: hidden;
    padding: 20px;
}
table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 10px;
}
th, td {
    padding: 15px;
    text-align: left;
}
th {
    background-color: #bebebe;
    color: #333;
    font-weight: bold;
}
tr:nth-child(even) {
    background-color: #f9fafb;
}
tr:hover {
    background-color: #f1f1f1;
}
.actions i {
    margin-right: 10px;
    cursor: pointer;
}
.actions i:hover {
    color: #FF0000;
}
.header-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #d1d8dd;
    padding: 20px;
    border-radius: 10px;
    margin-bottom: 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
.header-container h1 {
    margin: 0;
    font-size: 24px;
}
</style>
