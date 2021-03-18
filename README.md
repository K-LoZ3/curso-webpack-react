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
#### Configuración de plugins y loaders para React
Necesitamos el loader y el plugin para trabajar con html.
1. Intalamos estos paquetes.
   ~~~
   npm install -D html-loader html-webpack-plugin
   ~~~
2.  Importamos dentro de webpack el plugin y lo incorporamos en un campo plugins dentro de webpack. Este recibe un array con los plugins.
   ~~~
   plugins: [
      new HtmlWebpackPlugin({
         template: './public/index.html',
         filename: './index.html'
      })
   ]
   ~~~
3. Cremos una regla/objeto-en-rules para que trabaje trabaje con html y el loader.
   - test para decirle cuales extenciones seran usadas por el loader.
      ~~~
      test: /\.html$/,
      ~~~
   - use para decirle que loader usara.
      ~~~
      use: [
         {
            loader: 'html-loader',
         }
      ]
      ~~~
      o
      ~~~
      use: {
         loader: 'html-loader',
      }
      ~~~
4. Creamos el script dentro del package.json para ejecutar esta configuracion al compilar el proyecto. Para ejecutar el proyecto y mostrarlo en el servidor local tambien.
   ~~~
   "start": "webpack serve",
   "build": " webpack --mode production"
   ~~~
#### Configuración de Webpack para CSS en React
Para configurar el css intallamos los plugin de css y loaders como dependencias de desarrollo. Necesitamos el minimizador de css y ademas los precompiladores como sass.
1. Instalamos los paquetes necesarios.
   ~~~
   npm install -D mini-css-extract-plugin css-loader style-loader sass sass-loader
   ~~~
2. Configuramos todo en webpack.config.js
   - Importamos el plugin de css "mini-css-extract-plugin"
      ~~~
      const MiniCssExtractPlugin = require("mini-css-extract-plugin");
      ~~~
   - Creamos la regla para el css y trabajar con los loader.
      ~~~
      {
         test: /\.s[ac]sss$/,
         use: [
            'style-loader',
            'css-loader',
            'sass-loader'
         ],
      },
      ~~~
      - Test para la exprecion regular que le dice que extencion revizar.
      - use con un array con el nombre de cada loader para que sepa cuales usar.
   - Agregamos el plugin en el apartado de plugins.
      ~~~
      new MiniCssExtractPlugin({
         filename: '[name].css',
      }),
      ~~~
      - Pasamos un objeto con solo la configuracion del nombre que tendra el css despues de configurarlo. El "[name]" es para decirle que le ponga el mismo nombre y .css es para que quede fijo con esa extencion.
3. Creamos una carpeta styles dentro de src con un archivo global.scss para probar si todo el css funciona.
   ~~~
   $base-color: #c6538c;
   $color: rgba(black, 0.88);

   body {
      background-color: $base-color;
      color: $color;
   }
   ~~~
4. Importamos/enlazamos el css dentro del index.js.
#### Optimización de Webpack para React
Necesitamos dejar todo listo para produccion. Limpiar el dist minimizar los archivos y demas, tambien separamos las configuraciones de desarrollo de las de produccion.
1. Cramos el archivo webpack.config.dev.js, pegamos toda la configuracion como esta y añadimos el modo de dessarrollo en este.
   ~~~
   // ...
   mode: 'development',
   module: {
      // ...
   ~~~
2. Instalamos los plugins para las optimizaciones.
   ~~~
   npm install -D css-minimizer-webpack-plugin terser-webpack-plugin clean-webpack-plugin
   ~~~
3. En las configuraciones de produccion hacemos los cambios de optimizacion.
   - Eliminamos la configuracion de devServer ya que solo es para desarrollo.
   - Importamos los plugins minimizer de css, terser y clean.
      ~~~
      const CssMinimizerPlugin = require('css-minimizer-webpack-plugin');
      const TerserPlugin = require('terser-webpack-plugin');
      const { CleanWebpackPlugin } = require('clean-webpack-plugin');
      ~~~
   - Creamos dentro de output un publicPath ya que es necesario que se sepa que ruta tendra por defecto el proyecto al publicarlo.
      ~~~
      publicPath: "/",
      ~~~
   - Creamos los alias (Para las rutas que importamos en el proyecto. De esta manera quedan como variables.). Esto queda dentro de resolve como un objeto en alias.
      ~~~
      resolve: {
         extensions: ['.js', '.jsx'],
         alias: {
            '@components': path.resolve(__dirname, 'src/components/'),
            '@styles': path.resolve(__dirname, 'src/styles/'),
         }
      }
      ~~~
   - Añadimos la parte de optimizacion con un objeto que tendra las configuraciones.
      - minimize que recive un boolean.
      - minimizer que recive la configuracion de los plugin para esto.
      ~~~
      optimization: {
         minimize: true,
         minimizer: [
            new CssMinimizerPlugin(),
            new TerserPlugin(),
         ],
      },
      ~~~
   - Incluimos el plugin de limpieza del proyecto.
      ~~~
      new CleanWebpackPlugin(),
      ~~~
4. Incluimos el modo de produccion en estas configuraciones.
   ~~~
   mode: 'production',
   ~~~
5. Creamos el script en el package.json.
   - Modificamos el star para que use las configuraciones de desarrollo.
      ~~~
      "start": "webpack serve --config webpack.config.dev.js",
      ~~~
   -modificamos el build para que sea el que use las configuraciones de produccion.
      ~~~
      "build": "webpack --config webpack.config.js"
      ~~~