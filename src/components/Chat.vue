<template>
    <!-- Contenedor principal del chat que muestra los mensajes -->
    <div class="col-6 mensaje">
        <!-- Itera sobre cada mensaje en el array 'mensajes' -->
        <div v-for="(mensaje, index) in mensajes" :key="index">
            <!-- Muestra el nombre completo del remitente, alineado a la izquierda o derecha según el ID -->
            <p :style="{ textAlign: mensaje.id === usuarios[0].login.uuid ? 'left' : 'right' }">
                {{ mensaje.nombreCompleto }}
            </p>
            <!-- Contenedor para el mensaje, alineado según el ID del usuario (propio o del otro) -->
            <div class="d-flex"
                :style="{ justifyContent: mensaje.id === usuarios[0].login.uuid ? 'flex-start' : 'flex-end' }">
                <!-- Muestra el contenido del mensaje, con un fondo de color dinámico -->
                <p class="rounded px-2 text-break" :style="{ backgroundColor: mensaje.color }">
                    {{ mensaje.texto }}
                </p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Chat", // Nombre del componente
    props: {
        mensajes: Array, // Array de mensajes enviados en el chat
        usuarios: Array, // Array de usuarios participantes en el chat
    }
};
</script>

<style scoped>
/* Estilos para el contenedor del chat */
.mensaje {
    box-shadow: 0px 0px 19px 2px rgba(0, 0, 0, 1);
    height: 450px;
    overflow-y: scroll;
    background-color: #EFE6DD;
}
</style>
