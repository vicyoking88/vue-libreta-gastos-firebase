# libreta_gastos

## Introduction

- SPA que administra los gastos de los usuarios registrados en Firebase utiliza los servicios de authentication y almacenamiento de colecciones.

## Project setup

- El proyecto se creo utilizando yarn como manejador de paquetes ejecutar el siguiente comanto para instalar las dependencias.

```
yarn install
```

```
usuario para pruebas => `prueba@prueba.com`
contraseña = `123456789`
```

### Compiles and hot-reloads for development

- Ejecutar el siguiente comando para correr una aplicacion local que sirve en el puerto `http://localhost:8080/`

```
yarn serve
```

### Important consideration

- En el componente principal `App.vue` encontrara la variable `firebaseConfig` el cual puede configurar para la cuenta de firebase que desee

### Compiles and minifies for production

```
yarn build
```

### Lints and fixes files

```
yarn lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).
