<script setup>
    import { ref } from 'vue';

    const nombre = ref('');
    const precio = ref(0);
    const stock = ref(0);
    const categoria = ref('');

    const emit = defineEmits(['agregarProducto'])

    const enviarFormulario = () => {
        if (!nombre.value || !categoria.value) {
            alert('Por favor, completá todos los campos');
            return; 
        }
        if (precio.value <= 0 || stock.value < 0) {
            alert('El precio debe ser mayor a 0 y el stock no puede ser negativo');
            return;
        }

        const nuevoProducto = {
            nombre: nombre.value,
            precio: precio.value,
            stock: stock.value,
            categoria: categoria.value
        };

        emit('agregarProducto', nuevoProducto);

        nombre.value = '';
        precio.value = 0;
        stock.value = 0;
        categoria.value = '';
    };
</script>
<template>
    <div class="main-container">
        <div class="decor-section">
            <div class="icon-placeholder">📦</div>
        </div>
        <form @submit.prevent="enviarFormulario">
        <h2>Nuevo producto</h2>
        <div>
            <label for="nombre">Nombre</label>
            <input v-model="nombre" type="text" id="nombre" name="nombre" required>
        </div>

        <div>
            <label for="precio">Precio</label>
            <input v-model="precio" type="number" id="precio" name="precio" min="0" required>
        </div>

        <div>
            <label for="stock">Stock</label>
            <input v-model="stock" type="number" id="stock" name="stock" min="0" required>
        </div>

        <div>
            <label for="categoria">Categoría</label>
            <select v-model="categoria" id="categoria" required>
                <option value="">Seleccione...</option>
                <option value="Electronica">Electrónica</option>
                <option value="Hogar">Hogar</option>
                <option value="Moda">Moda</option>
                <option value="Alimentos">Alimentos</option>
                <option value="Limpieza">Limpieza</option>
                <option value="Musica">Musica</option>
                <option value="Tecnologia">Tecnologia</option>
                <option value="Joyas">Joyas</option>
            </select>
        </div>

        <button type="submit">AGREGAR PRODUCTOS</button>
    </form>
    </div>
</template>
<style>
    .main-container {
        background-color: #16161a;
        backdrop-filter: blur(12px);
        border-radius: 20px;
        border: 2px solid #ff007f;
        padding: 35px;
        width: 100%;
        max-width: 400px;
        box-sizing: border-box;
        display: flex;
        flex-direction: column;
        gap: 20px;
        margin: 0 auto;
    }
    .decor-section {
        display: flex;
        justify-content: center;
        margin-bottom: 10px;
    }
    .icon-placeholder {
        font-size: 3rem;
        background: #121216;
        padding: 15px;
        border-radius: 50%;
        box-shadow: 0 0 15px rgba(255, 0, 127, 0.4);
    }
    form {
        display: flex;
        flex-direction: column;
        gap: 15px;
    }
    h2 {
        color: #ffffff;
        text-align: center;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        font-size: 1.5rem;
        margin-bottom: 5px;
        text-shadow: 0 0 8px rgba(255, 0, 127, 0.4);
    }
    form > div {
        display: flex;
        flex-direction: column;
        gap: 6px;
    }
    label {
        color: #F0F2F5;
        font-weight: 500;
        font-size: 0.9rem;
    }
    input, select {
        background: #121216;
        border: 1px solid #4A4A5A;
        color: #F0F2F5;
        border-radius: 10px;
        padding: 12px;
        width: 100%;
        box-sizing: border-box;
        transition: all 0.3s ease;
    }
    input:focus, select:focus {
        outline: none;
        border-color: #ff007f;
        box-shadow: 0 0 10px #ff007f;
    }
    button {
        background-color: #ff007f;
        color: #ffffff;
        font-weight: bold;
        border: none;
        border-radius: 12px;
        padding: 14px;
        width: 100%;
        cursor: pointer;
        box-shadow: 0 0 10px rgba(255, 0, 127, 0.3);
        transition: all 0.3s ease;
        margin-top: 10px;
    }
    button:hover {
        background: #ff33a1;
        box-shadow: 0 0 18px #ff007f;
        transform: translateY(-1px);
    }
</style>