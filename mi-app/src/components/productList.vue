<script setup>
import ProductItem from './ProductItem.vue';

defineProps({
    productos: {
        type: Array,
        required: true
    }
});

defineEmits(['eliminarProducto']);
</script>
<template>
    <div v-if="productos.length > 0" class="TProductos">
        <table class="tablaProductos">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>NOMBRE</th>
                    <th>PRECIO</th>
                    <th>STOCK</th>
                    <th>CATEGORIA</th>
                    <th>ELIMINAR</th>
                </tr>
            </thead>
            <transition-group name="table-row" tag="tbody">
                <ProductItem 
                    v-for="prod in productos" 
                    :key="prod.id" 
                    :prod="prod"
                    @eliminarProducto="(id) => $emit('eliminarProducto', id)" 
                />
            </transition-group>
            <!--<tbody>
                <ProductItem 
                    v-for="prod in productos" 
                    :key="prod.id" 
                    :prod="prod"
                    @eliminarProducto="(id) => $emit('eliminarProducto', id)" 
                />
            </tbody>-->
        </table>
    </div>
    <p v-else class="mensajeVacio">No hay productos cargados</p>
</template>