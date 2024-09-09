<script>
import axios from "axios";
import User from './components/User.vue'; // Importa el componente User
import Chat from './components/Chat.vue'; // Importa el componente Chat

export default {
  name: 'App',
  components: {
    User, // Registra el componente User
    Chat, // Registra el componente Chat
  },
  data() {
    return {
      usuarios: [], // Almacena los usuarios obtenidos de la API
      mensajes: [], // Almacena los mensajes enviados en el chat
    };
  },
  methods: {
    // Maneja el evento de envío de un mensaje desde el componente User
    enviarMensajeHandler(mensaje) {
      // Crea un nuevo objeto de mensaje con los datos recibidos
      let nuevoMensaje = {
        id: mensaje.id, // ID del usuario que envió el mensaje
        color: mensaje.color, // Color elegido para el mensaje
        nombreCompleto: mensaje.nombreCompleto, // Nombre del usuario
        texto: mensaje.texto // Texto del mensaje
      };
      this.mensajes.push(nuevoMensaje); // Añade el mensaje al array de mensajes
    },

    // Método para obtener los datos de usuarios de la API
    async fetchData() {
      try {
        const url = "https://randomuser.me/api/?results=2"; // URL de la API para obtener dos usuarios aleatorios
        const { data } = await axios.get(url); // Llamada a la API usando Axios
        this.usuarios = data.results; // Asigna los usuarios obtenidos a la variable usuarios
      } catch (error) {
        console.error('Error al obtener los datos:', error); // Muestra un error si la petición falla
      }
    },
  },
  // Hook del ciclo de vida que se ejecuta al montar el componente
  mounted() {
    this.fetchData(); // Llama al método para obtener los datos al cargar la aplicación
  },
};
</script>

<template>
  <div class="container">
    <div class="mt-5">
      <!-- Renderiza el contenido solo si hay usuarios cargados -->
      <div class="row" v-if="usuarios.length">
        <!-- Primer usuario (alineado a la izquierda) -->
        <User :usuario="usuarios[0]" :align="'left'" :defaultColor="'#CBEBFF'" @enviarMensaje="enviarMensajeHandler" />
        <!-- Componente del chat que muestra los mensajes -->
        <Chat :mensajes="mensajes" :usuarios="usuarios" />
        <!-- Segundo usuario (alineado a la derecha) -->
        <User :usuario="usuarios[1]" :align="'right'" :defaultColor="'#2ecc71'" @enviarMensaje="enviarMensajeHandler" />
      </div>

      <!-- Mensaje de carga mientras se obtienen los usuarios -->
      <div class="text-center" v-else>
        <h2>Cargando usuarios...</h2>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
