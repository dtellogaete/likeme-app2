# README - Frontend

## Instalación

Para utilizar el frontend de forma correcta, se debe ejecutar el siguiente comando en `likeme-app2\frontend\`:

1. Ejecutar el siguiente comando para instalar las dependencias necesarias:
   ```
   npm i -y
   ```

## Uso

Una vez instaladas las dependencias, se debe ejecutar el siguiente comando en `likeme-app2\frontend\`:
   ```
   npm run dev
   ```

Visualización del frontend en uso:

![Frontend Like Me](/likeme.png)

# README - Backend

El backend de esta aplicación consta de dos funciones principales:

1. `likePost()`:
   La función `likePost()` se encarga de actualizar el número de likes de un post en la base de datos. Toma un parámetro `id` que representa el identificador único del post que se va a actualizar. Aquí está el código:   

   ```javascript
   const likePost = async (id) => {
     try {
       console.log(id);
       const query = 'UPDATE posts SET likes = likes + 1 WHERE id = $1';
       const values = [id];
       const result = await pool.query(query, values);
       console.log(result);
       return result;      
     } catch (error) {
       throw error;
     }
   };
   ```

2. `deletePost()`:
   La función `deletePost()` se encarga de eliminar un post de la base de datos. Toma un parámetro `id` que representa el identificador único del post que se va a eliminar. Aquí está el código:

   ```javascript
   const deletePost = async (id) => {
     try {
       console.log(id);
       const query = 'DELETE FROM posts WHERE id = $1';
       const values = [id];
       const result = await pool.query(query, values);
       console.log(result);
       return result;
     } catch (error) {
       throw error;
     }
   }
   ```

Con Express, se implementan las siguientes rutas para interactuar con los posts:

1. PUT Posts:
   Esta función define la ruta para las solicitudes PUT en la URL '/posts/like/:id'. Cuando se recibe una solicitud PUT en esta ruta, se extrae el parámetro `id` de la URL utilizando `req.params.id`. Luego, se asigna a la variable `postId` y se le agrega a la función `likePost`.

   ```javascript
   app.put('/posts/like/:id', async (req, res) => {
     try {
       const postId = req.params.id;
       console.log(postId);
       await likePost(postId);
       res.status(200).json({ message: 'Like actualizado' });
     } catch (error) { 
       res.status(500).json({ error: 'Internal Server Error', errorMessage: error.message });
       res.status(500).send('Internal Server Error', error.message);
       console.log(error);
     }
   });
   ```

2. DELETE Posts:
   Esta función define una ruta para manejar las solicitudes DELETE en la URL '/posts/:id'. Cuando se recibe una solicitud DELETE en esta ruta, se extrae el parámetro `id` y se ingresa a la función `deletePost`.

   ```javascript
   app.delete('/posts/:id', async (req, res) => {
     try {
       const postId = req.params.id;
       await deletePost(postId);
       res.status(200).json({ message: 'Post eliminado' });
     } catch (error) { 
       res.status

## Base de datos

Este backend utiliza una base de datos Postgresql llamada `likeme` y una conexión a través de `pool`. 

La estructura de la base de datos es la siguiente: 

```sql
CREATE TABLE posts (
  id SERIAL,
  titulo VARCHAR(25),
  img VARCHAR(1000),
  descripcion VARCHAR(255),
  likes INT
);
```
