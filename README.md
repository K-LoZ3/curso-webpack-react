# curso-webpack-react
#### Creamos el proyecto de react.
Lo primero es iniciar npm e instalar react y react-dom como dependencias normales.
   ~~~
   npm init -y
   npm i react react-dom -S
   ~~~
1. Creamos la estructura del proyecto.
   ~~~
   node_modules
   public
      index.html
   scr
      components
         App.jsx
      index.js
   .gitignore
   package.json
   README.md
   ~~~
2. En index.js importamos react y react-dom para renderizar la app en el html.
   ~~~
   import React from 'react';
   import reacDOM from 'react-dom';
   import App from './components/App';

   reacDOM.render(<App />, document.getElementById('app'));
   ~~~
3. En el index.html creamos la estructura del html:5 y en el body un div con el id=app, div#app.
4. En App.jsx creamos un componente sencillo.
   ~~~
   import React from 'react';
   const App = () => <h1>Hello React!</h1>
   export default App;
   ~~~
#### Configuración de Webpack 5 para React.js
1. Instalamos las dependencias que vamos a usar con webpack y ademas babel.
   ~~~
   npm install -D @babel/core @babel/preset-env @babel/preset-react babel-loader
   ~~~
   ~~~
   npm install -D webpack webpack-cli webpack-dev-server
   ~~~
2. Cramos el archivo de configuracion de babel ".babelrc".
   ~~~
   {
      "presets": [
         "@babel/preset-env",
         "@babel/preset-react"
      ]
   }
   ~~~
2. Creamos el archivo de webpack.config.js.
   - Importamos path ya que es necesario para las rutas.
   - entry es para decirle el archivo principal o punto de entrada del proyecto (toda la app esta importada en este archivo).
   - output (objeto) tendra la configuracion de archivo resultara de ejecutar webpack.
      - path para la ruta donde va a quedar.
      - filename para el nombre que tendra el archivo.
   - resolve para que resuelva conflicto de extenciones del proyecto. para este caso .js y .jsx.
      - extensions para poner estas extenciones en un array de string.
   - module para crear las reglas
      - rules con un array de objetos donde cada objeto es una regla
         - El primer objeto/regla es el que le dice a webpack que trabaje con babel para que transpile el codigo.
            - test para decirle con una exprecion regular cuales son las extenciones.
               ~~~
               test: /\.(js|jsx)$/,
               ~~~
            - exclude para excluir carpetas o archivos que no necesitamos que transpile babel en este caso solo "/node_modules/".
            - use para darle un loader o el paquete que se encargara de transpilar o cargar los archivos para transpilarlos con la configuracion del .babelrc.
               ~~~
               use: {
                  loader: 'babel-loader',
               },
               ~~~
   - devServer (fuera de module) para tener la configuracion de desarrollo y que se quede escuchando cambios en el proyecto.
      - contentBase para poner la ruta que estara mostrando o desplegando en el servidor local. Esto con path para que sea una ruta exacta.
         ~~~
         contentBase: path.join(__dirname, 'dist'),
         ~~~
      - compress por si lo queremos comprimir. con true o false es suficiente.
      - port para expecificar el puerto que queremos.