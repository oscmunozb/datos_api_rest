<template>
  <!-- Contenedor de la tarjeta del usuario con centrado horizontal y vertical -->
  <div class="col-3 d-flex justify-content-center align-items-center">
    <div class="card">
      <!-- Imagen de usuario obtenida del objeto usuario -->
      <img class="card-img-top" :src="usuario.picture.large" :alt="'Usuario: ' + nombreCompleto">

      <!-- Cuerpo de la tarjeta con el nombre completo del usuario y el formulario para enviar mensajes -->
      <div class="card-body">
        <!-- Muestra el nombre completo con alineación según la propiedad 'align' -->
        <h5 class="card-title" :style="{ textAlign: align }">{{ nombreCompleto }}</h5>

        <!-- Formulario para seleccionar color del mensaje y escribir el texto -->
        <form>
          <!-- Selector de color para el mensaje, enlazado al modelo 'mensaje.color' -->
          <input class="form-control form-control-color w-100" type="color" v-model="mensaje.color">

          <!-- Área de texto para escribir el mensaje, enlazado al modelo 'mensaje.texto' -->
          <textarea class="form-control mt-2" v-model="mensaje.texto"></textarea>

          <!-- Botón para enviar el mensaje, previene el comportamiento por defecto del formulario -->
          <button class="btn btn-success w-100 mt-2" @click.prevent="enviarMensaje">Enviar</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'User',
  props: {
    usuario: Object, // Recibe el objeto usuario desde el componente padre
    defaultColor: String, // Color predeterminado para el mensaje, proporcionado desde el padre
  },
  data() {
    return {
      // Inicializa el mensaje con un texto vacío y el color predeterminado
      mensaje: {
        texto: "",
        color: this.defaultColor,
      },
      align: "", // Alineación del nombre, que se puede modificar dinámicamente
    };
  },
  computed: {
    // Computed para obtener el nombre completo del usuario concatenando el nombre y el apellido
    nombreCompleto() {
      return `${this.usuario.name.first} ${this.usuario.name.last}`;
    }
  },
  methods: {
    // Método que maneja el envío del mensaje
    enviarMensaje() {
      // Verifica si el campo de texto está vacío antes de emitir el mensaje
      if (this.mensaje.texto.trim() === "") {
        // Muestra un alert si el mensaje está vacío
        alert("El mensaje no puede estar vacío.");
        return; // Si el mensaje está vacío, no se envía y se sale del método
      }

      // Asigna el ID del usuario al mensaje antes de enviarlo
      this.mensaje.id = this.usuario.login.uuid;
      // Asigna el nombre completo del usuario al mensaje
      this.mensaje.nombreCompleto = this.nombreCompleto;

      // Emite el evento 'enviarMensaje' hacia el componente padre con el mensaje procesado
      this.$emit('enviarMensaje', this.mensaje);

      // Limpia el campo de texto del mensaje después de enviarlo
      this.mensaje.texto = "";
    }
  },
};
</script>

<style scoped></style>
