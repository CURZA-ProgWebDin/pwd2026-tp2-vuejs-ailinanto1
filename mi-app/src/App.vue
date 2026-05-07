<script setup>
  import { ref, computed } from "vue";
  import ProductForm from './components/ProductForm.vue';
  import ProductList from './components/ProductList.vue';

  const listaProductos = ref([]);
  const categoriaFiltro = ref('');

  const agregarLista = (nuevoProducto) => {
    const nuevoId = listaProductos.value.length + 1;
    const productoConId = {
      id: nuevoId,
      ...nuevoProducto
    };
    listaProductos.value.push(productoConId);
  };

  const eliminarProducto = (id) => {
    listaProductos.value = listaProductos.value.filter(prod => prod.id !== id);
  };

  const productosFiltrados = computed(() => {
    if (!categoriaFiltro.value) {
      return listaProductos.value;
    }
    return listaProductos.value.filter(
      (prod) => prod.categoria === categoriaFiltro.value
    );
  });

  const cantidadTotal = computed(() => {
    return listaProductos.value.reduce((total, p) => total + Number(p.stock), 0);
  });

  const valorTotalInventario = computed(() => {
    return listaProductos.value.reduce((total, p) => total + (Number(p.precio) * Number(p.stock)), 0)
  });
</script>
<template>
  <div class="container">
    <h1 class="titulo">Gestión de inventario</h1>
    
    <div class="layout">
      <ProductForm @agregarProducto="agregarLista"></ProductForm>
      
      <div class="lista">
        <h2>Lista de productos</h2>
        
        <div class="filtro-container">
          <label for="filtroCategoria">Filtrar por categoría:</label>
          <select v-model="categoriaFiltro" id="filtroCategoria" class="select-filtro">
            <option value="">Mostrar todos</option>
            <option value="Electronica">Electrónica</option>
            <option value="Hogar">Hogar</option>
                <option value="Moda">Moda</option>
            <option value="Alimentos">Alimentos</option>
            <option value="Limpieza">Limpieza</option>
            <option value="Musica">Música</option>
            <option value="Tecnologia">Tecnología</option>
            <option value="Joyas">Joyas</option>
          </select>
        </div>

        <ProductList 
          :productos="productosFiltrados" 
          @eliminarProducto="eliminarProducto" 
        />

        <div class="resumen-container">
          <h3>Resumen del Inventario</h3>
          <div class="resumen-item">
            <span>Cantidad total de productos:</span>
            <strong>{{ cantidadTotal }}</strong>
          </div>
          <div class="resumen-item">
            <span>Valor total del inventario:</span>
            <strong>${{ valorTotalInventario }}</strong>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<style>
  * {
    box-sizing: border-box;
  }
  body {
    background-color: #0F0F12;
    margin: 0;
    font-family: system-ui, sans-serif;
    color: #F0F2F5;
    min-height: 100vh;
  }
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 40px 20px;
    min-height: 100vh;
  }
  .titulo {
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #FFFFFF;
    margin-bottom: 40px;
  }
  .layout {
    display: grid;
    grid-template-columns: 1fr 1.5fr;
    gap: 40px;
    width: 100%;
    max-width: 1100px;
    align-items: flex-start;
  }
  .lista {
    background-color: #16161a;
    border-radius: 20px;
    border: 2px solid #ff007f;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.5);
    padding: 35px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 620px;
    gap: 20px;
  }
  .lista h2 {
    color: #FFFFFF;
    font-size: 1.5rem;
    text-align: center;
    text-shadow: 0 0 8px rgba(255, 0, 127, 0.4);
    margin-bottom: 10px;
  }
  .filtro-container {
    display: flex;
    flex-direction: column;
    gap: 6px;
    margin-bottom: 10px;
  }
  .filtro-container label {
    color: #F0F2F5;
    font-weight: 500;
    font-size: 0.9rem;
  }
  .select-filtro {
    background: #121216;
    border: 1px solid #4A4A5A;
    color: #F0F2F5;
    border-radius: 10px;
    padding: 10px;
    width: 100%;
    box-sizing: border-box;
    transition: all 0.3s ease;
  }
  .select-filtro:focus {
    outline: none;
    border-color: #ff007f;
    box-shadow: 0 0 10px #ff007f;
  }
  .mensajeVacio {
    color: #7A7A8C;
    text-align: center;
    font-size: 1.1rem;
    margin: auto 0;
  }
  .TProductos {
    overflow-x: auto;
    overflow-y: auto;
    max-height: 280px;
  }
  .TProductos::-webkit-scrollbar {
  width: 10px;
}
.TProductos::-webkit-scrollbar-track {
  background: #16161a; 
  border-radius: 10px;
}
.TProductos::-webkit-scrollbar-thumb {
  background: #ff007f; 
  border-radius: 10px;
  border: 2px solid #16161a; /* Borde para que no quede pegado a los lados */
}
.TProductos::-webkit-scrollbar-thumb:hover {
  background: #cc0066;
}
  .tablaProductos {
    width: 100%;
    border-collapse: collapse;
    text-align: left;
  }
  .tablaProductos th, .tablaProductos td {
    padding: 12px 15px;
    border-bottom: 1px solid #4A4A5A;
    color: #F0F2F5;
  }
  .tablaProductos th {
    color: #ff007f;
    font-weight: 600;
    font-size: 0.95rem;
  }
  .tablaProductos tbody tr {
    background-color: #121216;
    transition: background-color 0.2s ease;
  }
  .tablaProductos tbody tr:hover {
    background-color: #1a1a24;
  }
  .table-row-enter-active, .table-row-leave-active {
    transition: all 0.5s cubic-bezier(0.68, -0.55, 0.27, 1.55); /* Efecto de rebote elástico */
  }
  .table-row-enter-from {
    opacity: 0;
    transform: translateX(-40px); /* Entra desde la izquierda */
  }
  .table-row-leave-to {
    opacity: 0;
    transform: translateX(40px) scale(0.95); /* Se va hacia la derecha y se contrae levemente */
  }
  .btnEliminar {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
  }
  .btnEliminar:hover {
    background-color: #cc0066;
  }
  .resumen-container {
    margin-top: 15px;
    padding: 15px;
    background-color: #121216;
    border: 1px solid #4A4A5A;
    border-radius: 12px;
    color: #F0F2F5;
  }
  .resumen-container h3 {
    margin-top: 0;
    color: #ff007f;
    font-size: 1.1rem;
    text-align: center;
  }
  .resumen-item {
    display: flex;
    justify-content: space-between;
    margin-bottom: 8px;
    font-size: 0.95rem;
  }
  .resumen-item:last-child {
    margin-bottom: 0;
  }
</style>