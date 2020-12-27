## Iniciar un proyecto de React desde cero
 
 En esta sesión se enseñara a iniciar un proyecto de React desde cero utilizando Webpack y Babel.

 ### Paso 1: Crear un archivo package.json

* Crear carpeta con el nombre del proyecto
  
  ``` 
  mkdir lucho-project
  cd lucho-project
  ```

* Iniciar el projecto de npm
  ```
  npm init --yes
  ```

  ### Paso 2: Instalar React, Babel y Webpack
```
npm i react react-dom 

npm i -D @babel/preset-react @babel/preset-env @babel/core babel-loader @babel/plugin-proposal-class-properties

npm i -D webpack webpack-cli webpack-dev-server html-webpack-plugin path
```

### Paso 3: Crear archivo de configuración de Babel. 
\
Babel nos ayuda a transcompilar código de Javascript moderno (ES5) a un JS que cualquier navegador pueda comprender. Babel funciona mediante plugins, con los cuales indicamos qué cosas queremos que transforme. Estos plugins se definen en un archivo de configuración llamado `.babelrc`. En este caso, instalamos el plugin `preset-react` en el paso 2 para decirle a Babel que compile los archivos de React a JS plano.

Primero crearemos el archivo de configuración .babelrc
```
touch .babelrc
```

Editamos el contenido de .babelrc
```
nano .babelrc
```

Copiamos este contenido
```
{
  "presets": [
 [ "@babel/preset-env", {
   "modules": false,
   "targets": {
  "browsers": [
    "last 2 Chrome versions",
    "last 2 Firefox versions",
    "last 2 Safari versions",
    "last 2 iOS versions",
    "last 1 Android version",
    "last 1 ChromeAndroid version",
    "ie 11"
  ]
   }
 } ],
 "@babel/preset-react"
  ],
  "plugins": [ "@babel/plugin-proposal-class-properties" ]
}
```

### Paso 4: Crear directorios src y public
Crearemos los directorios `src` y `public` con sus respectivos contenidos iniciales

```
mkdir src public
touch src/index.js src/App.js public/index.html
```

### Paso 5: Setear archivo de configuración webpack.config.js
\
Webpack es un sistema de `bundling` que sirve para preparar nuestra aplicación para un ambiente de producción. Básicamente se encarga de empaquetas las dependencias y el codigo para poder migrarlo facilmente al ambiente de producción.

Instalamos los loaders de css, estilo y archivos de imagenes.
```
npm install style-loader css-loader file-loader
```

Crear el archivo de configuracion
```
touch webpack.config.js
```

Editamos el contenido y copiamos el contenido
```
const HtmlWebPackPlugin = require( 'html-webpack-plugin' );
const path = require( 'path' );
module.exports = {
   context: __dirname,
   entry: './src/index.js',
   output: {
      path: path.resolve( __dirname, 'dist' ),
      filename: 'main.js',
      publicPath: '/',
   },
   devServer: {
      historyApiFallback: true
   },
   module: {
      rules: [
         {
            test: /\.js$/,
            use: 'babel-loader',
         },
         {
            test: /\.css$/,
            use: ['style-loader', 'css-loader'],
         },         {
            test: /\.(png|j?g|svg|gif)?$/,
            use: 'file-loader'
         }
]
   },
   plugins: [
      new HtmlWebPackPlugin({
         template: path.resolve( __dirname, 'public/index.html' ),
         filename: 'index.html'
      })
   ]
};
```

### Paso 6: Crear un archivo App.js en el directorio de src

```
import React from 'react';class App extends React.Component {
 render() {
  return(
   <div>
    My App Component
   </div>
  );
 }
}export default App
```

### Paso 7:Crear un archivo index.html en el directorio public

Mediante el `div#root` sabremos en donde queremos que se muestre el contenido de nuestra aplicación de React.

```
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>React App</title>
</head>
<body>
<div id="root"></div>
<script type="text/javascript" src="main.js"></script></body>
</html>
```

### Paso 8: Insertamos el componente App.js dentro del DOM

```
import React from 'react';
import ReactDOM from 'react-dom';
import App from "./App";

ReactDOM.render( <App/>, document.getElementById('root') );
```

### Paso 9: Añadimos los scripts de webpack en el package.json
```
"scripts": {
    "webpack-dev-server": "webpack-dev-server",
    "dev": "webpack-dev-server --mode=development",
    "prod": "webpack --mode=production"
  },
```

### Paso 10: Corremos el proyecto a la verga
```
npm run dev
```