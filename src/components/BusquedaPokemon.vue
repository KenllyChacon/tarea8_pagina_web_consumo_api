<template>
  <head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">

    <title>Pokemon</title>
    <meta name="description" content="esta pagina busca lo pokemons">
    <meta name="author" content="Kenlly Chacon">
    <meta name="keywords" content="busqueda, pokemons, cual es este pokemon">
    <meta name="google-site-verification" content="">
    <meta name="googlebot" content="">
  </head>


  <body>
  <header>
    <div class="container">
      <h1>Busqueda Pokemon</h1>
    </div>
  </header>

  <nav>
  </nav>

  <section>
    <img id="imagenMuesta" v-if="mostrarImagen" :src="imagenPokemon" alt="">

    <div id="fondo"></div>

    <div class="container">
      <h1>Busqueda Pokemon</h1>
      <input v-model="text" type="text" placeholder="Ingresa el nombre de tu pokemon" v-on:click="borrar()">
      <button v-on:click="busquedaNombre(text)">Buscar</button>
      <img id="carga" v-if="mostrarCarga" src="../img/carga.gif" alt="">
      <img id="carga" v-if="noEncontrado" src="../img/no_encontrado.gif" alt="">

      <div v-if="mostrarImagen" id="res">
        <p>Nombre: {{ nombrePokemon }}</p>
        <p>Id: {{ idPokemon }}</p>
      </div>
    </div>
  </section>

  <footer>
    <div class="container">
      Derechos Reservados 2023-2024
    </div>
  </footer>

  </body>
</template>


<script>
export default {
  name: "BusquedaPokemon",
  data() {
    return {
      nombrePokemon: "",
      idPokemon: null,
      imagenPokemon: null,
      mostrarCarga: false,
      mostrarImagen: false,
      noEncontrado: false,
      text: ""
    }
  },
  methods: {
    async consumirApi() {
      let vectorRes = []
      for (let i = 1; i <= 600; i++) {
        const {id, name} = await fetch("https://pokeapi.co/api/v2/pokemon/" + i).then(r => r.json())

        const pokemon = {
          idPokemon: id,
          nombre: name,
          urlImagenPokemon: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/" + id + ".svg"
        }
        vectorRes.unshift(pokemon)
      }
      return vectorRes
    },
    async busquedaNombre(nombrePokemon) {
      this.noEncontrado = false
      this.nombrePokemon = ""
      this.imagenPokemon = null
      this.idPokemon = null
      this.mostrarImagen = true
      this.mostrarImagen = false
      this.mostrarCarga = true
      const data = await this.consumirApi()
      for (const valor in data) {
        const {nombre} = data[valor]
        const {urlImagenPokemon} = data[valor]
        const {idPokemon} = data[valor]
        if (nombre === nombrePokemon) {
          this.nombrePokemon = nombre
          this.imagenPokemon = urlImagenPokemon
          this.idPokemon = idPokemon
          this.mostrarImagen = true
        }
        this.mostrarCarga = false
      }
      if (this.nombrePokemon === "") {
        this.noEncontrado = true
      }

    },
    borrar(){
      this.text = ""
    }
  }
}
</script>

<style scoped>

.container {
  margin: 4%;
}

input, label, p {
  text-align: center;
  display: grid;
  width: 25%;
  margin: 0 auto;
  font-size: 25px;
  padding: 7px;
  border-radius: 10px;
}

.container {
  z-index: 99;
  position: relative;
}

#carga {
  width: 500px;
  height: 400px;
  margin: 15px;
}

#fondo {
  background: rgb(0, 0, 0, 0.5);
}

#imagenMuesta, #fondo {
  height: 100vh;
  width: 204vh;
  position: fixed;
  left: 0;
  top: 0;
  z-index: 0;
}

section button {
  transition-duration: 0.4s;
  padding: 15px;
  margin: 25px auto;
  display: block;
  text-align: center;
  font-size: 16px;
  border-radius: 10px;
  width: 25%;
  background-color: #3E4251;
  cursor: pointer;
  color: white;
}

button:hover {
  background-color: #1F2E3C; /* Green */
  color: white;
}


header, nav, section, footer, aside, article {
  display: block;
}

h1 {
  margin: 25px;
  text-align: center;
}

footer {
  font-size: 18px;
  margin: 25px auto;
  clear: both;
}

body {
  background-color: #1B1F27;
}

</style>