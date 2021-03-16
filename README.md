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