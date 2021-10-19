# Comenzando

## Atención acuérdate de intalar los Módulos

Instala los modulos, vete a terminal, nuevo terminal y escribe:

```
npm install
```

## Prerequisitos

Descarga e instala [Node.js](https://nodejs.org/en/)

Descarga e instala [Visual Studio Code](https://code.visualstudio.com/)

## Instalación

1. Instalar

Descarga e instala [Vue.js CLI](https://cli.vuejs.org/guide/installation.html)

```c
npm install -g @vue/cli
```

2. Ver la versión

```c
vue --version
```

3. Actualizar

```c
npm update -g @vue/cli
```

4. Creando un proyecto 
- Usando una interfaz gráfica

```c
vue ui
```

## Instalando módulos Boostrap/Popper y Firestore

1. Instala bootstrap

Instala [Bootstrap](https://getbootstrap.com/docs/5.0/getting-started/download/#npm)

```c
npm i --save bootstrap@^5.1
```

2. Instala también **Popper** en algunos componentes de Bootstrap es necesario

Necesita [Popper](https://getbootstrap.com/docs/5.0/getting-started/parcel/#install-bootstrap)

Instala [Popper](https://popper.js.org/)

```c
npm i @popperjs/core 
```
3. Copia en **main.js**

```html
import "bootstrap";
import "bootstrap/dist/css/bootstrap.min.css";
```

4. Instalar **firebase**

Docuemtación [Firebase](https://firebase.google.com/docs/web/setup?authuser=0)

```
npm install firebase
```

### Modificando el proyecto

1. Cambiar nombre de conponente **HelloWorld** por **Navbar**

Bootstrap [Navbar](https://getbootstrap.com/docs/5.1/components/navbar/)

2. Bootstrap copia un **navbar** y pégalo en el nuevo componente

3. Modifica en **Home.vue** import, components y etiqueta **HelloWorld** por **Navbar** y elimina la imagen.

6. Suprime en **App.vue** la navegación y los estilos

### Firebase firestore



8. Abrir archivo **main.js** en la carpeta **src** y pegar

Para iniciar el proyecto utiliza solo estas:

```js
import { initializeApp } from "firebase/app";
import { getFirestore } from 'firebase/firestore/lite';

const firebaseConfig = {
  apiKey: "AIzaSyDF3MX2SzdanQXlOGWFudqTPjX37JJljDo",
  authDomain: "fir-3cd29.firebaseapp.com",
  projectId: "fir-3cd29",
  storageBucket: "fir-3cd29.appspot.com",
  messagingSenderId: "669805638608",
  appId: "1:669805638608:web:2b40046fd5b4a7c52bbfd5"
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);


export { db };
```
### Firebase Auth Storage

Así quedará al final

```js
import { initializeApp } from "firebase/app";
import { getFirestore } from 'firebase/firestore/lite';
import { getAuth } from "firebase/auth";
import firebase from 'firebase/compat/app';
import 'firebase/compat/storage';

const firebaseConfig = {
    apiKey: "AIzaSyC-yjMEOwF0FAyC4Dsi3IqZcHqoZit4gyI",
    authDomain: "tiendaarduinoes.firebaseapp.com",
    projectId: "tiendaarduinoes",
    storageBucket: "tiendaarduinoes.appspot.com",
    messagingSenderId: "386838491375",
    appId: "1:386838491375:web:518d05af8440e82cb6ab1a",
    measurementId: "G-L90G5CE35V"
  };

firebase.initializeApp(firebaseConfig);
var storage = firebase.storage();
  // Initialize Firebase
const app = initializeApp(firebaseConfig);
const db = getFirestore(app);
const auth = getAuth();

export { db, auth, storage };
```

## GET / OBTENER DATOS / FIRESTORE

### TEMPLATE / Tabla

Crea una tabla para visualizar los datos de la base de datos

Bootstrap Tablas [tables](https://getbootstrap.com/docs/5.1/content/tables/)

Vue.js [v-for](https://es.vuejs.org/v2/guide/list.html)

```html
<template>
  <Navbar/>
  <div class="container">
    <table class="table">
    <thead>
      <tr>
        <th scope="col">id</th>
        <th scope="col">Nombre</th>
        <th scope="col">Correo</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, index) in usuarios" :key="index">
        <th scope="row">{{index}}</th>
        <td>{{item.nombre}}</td>
        <td>{{item.correo}}</td>
      </tr>
    </tbody>
    </table>
  </div>
</template>
```

### SCRIPT / Tabla
Crea un script para obtener los datos de la base de datos

```js
<script>
import Navbar from '../components/Navbar'
import { collection, getDocs } from 'firebase/firestore/lite';
import { db } from "../main";


export default {
  name: 'Home',
  components: {
    Navbar
  },
  data() {
    return {
      usuarios: []
    }
  },
  methods:{
  // GET / OBTENER / Consulta instantánea 
    async obtenerDatos () { 
      const querySnapshot = await getDocs(collection(db, "usuarios"));
        querySnapshot.forEach((doc) => {
        let usuario = doc.data()
        usuario.id = doc.id
        this.usuarios.push(usuario)
        console.log(usuario)
      });
    },
  },
    mounted() {
      this.obtenerDatos();
    },
}
</script>
```

## ADD / AGREGAR DATOS / FIRESTORE

### TEMPLATE / Formulario

Bootstrap Formularios [input group](https://getbootstrap.com/docs/5.1/forms/input-group/)

Bootstrap Botones [buttons](https://getbootstrap.com/docs/5.1/components/buttons/)

Vue.js [v-model](https://es.vuejs.org/v2/guide/components-custom-events.html#Personalizacion-de-componente-v-model)

Vue.js [@click](https://es.vuejs.org/v2/guide/syntax.html)

Vue.js [v-bind](https://es.vuejs.org/v2/guide/syntax.html)

```html
<!-- ////////// formulario Añadir ////////// -->
    <!-- Nombre -->
  <div class="container my-4">
  <form>  
    <div class="input-group mb-3">
    <span class="input-group-text">Nombre</span>
    <input v-model="usuario.nombre" type="text" class="form-control">
    </div>
    <!-- Correo -->
    <div class="input-group mb-3">
    <span class="input-group-text">Correo</span>
    <input v-model="usuario.correo" type="text" class="form-control">
    </div>
    <!-- Botone Guardar -->
    <div class="mt-3">  
    <button @click.prevent="agregarDato()" 
            class="btn btn-primary">Guardar
    </button>
    </div>
  </form>
  </div>
```
### SCRIPT / Formulario

```js
// Importar addDoc
import { collection, 
          getDocs,
          // Añadimos addDoc 
          addDoc } from 'firebase/firestore/lite';

// Añadir en data de v-model
usuario: {
      nombre: '',
      correo: ''
    }
// Añadir en methods
// ADD / AGREGAR / AÑADIR
async agregarDato(){
  const docRef = await addDoc(collection(db, "usuarios"), {
    nombre: this.usuario.nombre,
    correo: this.usuario.correo
  })
    .then(() => {
      console.log("Documento añadido");
    })
    .catch(function(error) {
      console.error("Error al añadir el documento: ", error);
    });
},
```

## DELETE / ELIMINAR / FIRESTORE

### TEMPLATE / Tabla

Añadiendo el botón **Eliminar**

```html
// Añado thead dos columnas: ID y Eliminar
<th scope="col">ID</th>
<th scope="col">Eliminar</th>

// Añado en tbody dos columnas: ID y botón Eliminar
<td>{{item.id}}</td>
<td>
  <button @click.prevent="eliminarDato(item.id)" 
    class="btn btn-danger">Eliminar
  </button>
</td>
```
### SCRIPT / Tabla

```js
import { collection, 
        getDocs, 
        addDoc,
        // Añadimos deleteDoc y doc
        deleteDoc, 
        doc 
        } from 'firebase/firestore/lite';
// DELETE / ELIMINAR / BORRAR
    async eliminarDato (id){
      await deleteDoc(doc(db, "usuarios", id ));
    },
```

## ROUTER GO / ACTUALIZANDO LOS DATOS

Añade router.go a: agregarDato() y eliminarDato()

```js
// Importa el router
import router from '../router/index'
// Añade en la función
router.go('/')
```
