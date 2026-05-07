<script setup>
defineProps({
    prod: {
        type: Object,
        required: true
    }
});

defineEmits(['eliminarProducto']);
</script>
<template>
    <tr class="itemProducto">
        <td>{{ prod.id }}</td>
        <td>{{ prod.nombre }}</td>
        <td>{{ prod.precio }}</td>
        <td>{{ prod.stock }}</td>
        <td>{{ prod.categoria }}</td>
        <td>
            <button @click="$emit('eliminarProducto', prod.id)" class="btnEliminar">
                🗑️
            </button>
        </td>
    </tr>
</template>
