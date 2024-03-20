# Aplication todo_list

### DESCRICION: 

en este trabajo, realizamos la construcción de una aplicación de un Todo_List en donde tendra como funciones el crear, ver, eliminar y completar elementos.

## Tabla de contenido

- [Descripción](#descricion)
- [Objetivos](#objetivos)
- [Añadir una tarea](#añadir-una-tarea)
- [Eliminar una tarea completada de la lista](#eliminar-una-tarea-completada-de-la-lista)
- [Agregando un CheckBox y función de toggle](#agregando-un-checkbox-y-función-de-toggle)
- [Editar una tarea para añadirla y enviarla](#editar-una-tarea-para-añadirla-y-enviarla)
- [Agregar useEffect hook.](#agrega-el-useeffect-hook)
- [Interfaz de usuario](#interfaz-de-usuario)
- [Conclusiones](#conclusiones-generales)
- [Enlaces](#enlace-utilizado)

## Objetivos

- Utiliza un manejador de eventos para crear la tarea "añadir tarea por hacer".

- Elimina la tarea completada de la lista de tareas por hacer utilizando el método de filtro.

- Agrega la función de alternancia y el cuadro de verificación para marcar la tarea como completada o no completada.

- Edita una tarea por hacer añadida y envíala utilizando la función de mapa.

- Utiliza el gancho UseEffect para guardar nuevas tareas por hacer en el almacenamiento local.

## Añadir una tarea

A continuación mostraremos el código que fue utilizado para la creación de una tarea

```sh
   function handleSubmit(e) {
    # Prevenir que el formulario se envíe y recargue la página
    e.preventDefault();

    # Obtener el valor del campo de entrada de la tarea
    let todo = document.getElementById('todoAdd').value;

    # Crear un nuevo objeto para representar la nueva tarea
    const newTodo = {
      id: new Date().getTime(), # Generar un ID único basado en la marca de tiempo
      text: todo.trim(), # Obtener el texto de la tarea, eliminando los espacios en blanco al inicio y al final
      completed: false, # Establecer la tarea como no completada inicialmente
    };

    # Verificar si el texto de la tarea no está vacío
    if (newTodo.text.length > 0) {
      # Si la tarea es válida, agregarla al estado 'todos'
      setTodos([...todos].concat(newTodo));
    } else {
      # Si el texto de la tarea está vacío, mostrar una alerta
      alert("Enter Valid Task");
    }

    # Limpiar el campo de entrada después de enviar el formulario
    document.getElementById('todoAdd').value = "";
   }

```

Esta función maneja la entrada de nuevas tareas, verifica que la entrada no esté vacía y luego agrega la tarea al estado de las tareas (todos).

## Eliminar una tarea completada de la lista

A continuación mostraremos el código que fue utilizado para la eliminación de una tarea

``` r

    # Copiar el array 'todos' para evitar mutar el estado original
   function deleteTodo(id) {
    let updatedTodos = [...todos].filter((todo) => todo.id !== id);
    setTodos(updatedTodos);
    # Actualizar el estado 'todos' con las tareas filtradas
  } 

```

La función deleteTodo elimina una tarea completada de la lista de tareas. Utiliza el método filter para crear un nuevo array sin la tarea específica que se desea eliminar, basándose en su id, y luego actualiza el estado de las tareas con este nuevo array.

## Agregando un CheckBox y función de toggle.

A continuación mostraremos el código que fue utilizado para añadir un CheckBox y la función toggle 

```r

  function toggleComplete(id) {
    # Crear un nuevo array de tareas utilizando map para iterar sobre cada tarea
    let updatedTodos = [...todos].map((todo) => {
      # Verificar si el ID de la tarea actual coincide con el ID proporcionado
      if (todo.id === id) {
        # Si coincide, invertir el estado de completitud de la tarea
        todo.completed = !todo.completed;
      }
      # Devolver la tarea actual, modificada o sin cambios
      return todo;
    });
     # Actualizar el estado 'todos' con el nuevo array que contiene las tareas actualizadas
    setTodos(updatedTodos);
  }

```

Esta funcion solo nos ayuda agregando un checkBox para verificar que la tarea este completada con el metodo map

## Editar una tarea para añadirla y enviarla.

A continuación mostraremos el código que fue utilizado para la modificación de una tarea y como guardarla al final.

```r

function submitEdits(newtodo) {  # Crear un nuevo array de tareas utilizando map para iterar sobre cada tarea
    const updatedTodos = [...todos].map((todo) => { # Si coincide, actualizar el texto de la tarea con el valor del campo de entrada
      if (todo.id === newtodo.id) {
        todo.text = document.getElementById(newtodo.id).value;
        # Devolver la tarea actual, modificada o sin cambios
        }
        return todo;
      });
      # Actualizar el estado 
      setTodos(updatedTodos);
      # Restablecer el estado de 'todoEditing' a null
      setTodoEditing(null);
    }

```

Esta función nos deja editar cualquier tarea que nos aparezca completada y poderla guardar sin ningun error 

## Agrega el useEffect hook.

A continuación mostraremos el código que fue utilizado para guardar en memoria local las tareas que demos como realizadas

```r

 import React, {useState,useEffect} from "react"; #Importamos el useEffect de react

useEffect(() => {
    # Obtener las tareas almacenadas en el localStorage
    const json = localStorage.getItem("todos");
    # Convertir las tareas almacenadas de JSON a un array de objetos
    const loadedTodos = JSON.parse(json);
    # Verificar si hay tareas cargadas y establecerlas como estado 'todos'
    if (loadedTodos) {
      setTodos(loadedTodos);
    }
}, []); # Este efecto se ejecuta solo una vez, al cargar el componente

useEffect(() => {
    # Verificar si hay tareas en el estado 'todos'
    if(todos.length > 0) {
        # Convertir las tareas en formato JSON
        const json = JSON.stringify(todos);
        # Guardar las tareas en el localStorage
        localStorage.setItem("todos", json);
    }
}, [todos]); # Este efecto se ejecuta cada vez que el estado 'todos' cambia
```

En pocas palabras, esta función nos ayuda a guardar en memoria local las tareas que estemos realizando.

## Interfaz de usuario
A continuación mostraremos el código que fue utilizado la creación de la interfaz de usuario a lo largo de este trabajo, llevando a acabo todos los métodos y funciones que seguimos paso a paso.

```r
        <div id="todo-list">
          <h1>Todo List</h1>
          <form onSubmit={handleSubmit}>
            <input
              type="text"
              id = 'todoAdd' # Campo de entrada para agregar nuevas tareas
            />
            <button type="submit">Add Todo</button> # Botón para enviar el formulario y agregar la tarea
          </form>
        {todos.map((todo) => (

          <div key={todo.id} className="todo">
            <div className="todo-text">
              {/* Add checkbox for toggle complete */}
              <input
                type="checkbox"
                id="completed"
                checked={todo.completed} # Estado de completitud de la tarea
                onChange={() => toggleComplete(todo.id)} # Función para cambiar el estado de completitud de la tarea
              />
              {/* if it is edit mode, display input box, else display text */}
              {todo.id === todoEditing ? # Verificar si la tarea está en modo de edición
                (<input
                  type="text"
                  id = {todo.id}
                  defaultValue={todo.text} # Mostrar el texto de la tarea en el campo de entrada
                />) :
                (<div>{todo.text}</div>)
              }
            </div>
            <div className="todo-actions">
              {/* if it is edit mode, allow submit edit, else allow edit */}
              {todo.id === todoEditing ?
              (
                <button onClick={() => submitEdits(todo)}>Submit Edits</button> # Botón para enviar las ediciones de la tarea
        ) :
              ) :
              (
                <button onClick={() => setTodoEditing(todo.id)}>Edit</button> # Botón para activar el modo de edición de la tarea
              )}

              <button onClick={() => deleteTodo(todo.id)}>Delete</button> # Botón para eliminar la tarea
             </div>
          </div>
        ))}
        </div>
```

Este código crea una interfaz de usuario para gestionar una lista de tareas por hacer, permitiendo al usuario agregar, editar, marcar como completadas y eliminar tareas.

## Conclusiones Generales

En este proyecto, se creó una aplicación web con la ayuda de la biblioteca React de javaScript. En donde todos los conocimientos fueron puestos a prueba en este trabajo, enfrentando desafíos y resolviendo problemas que nos saltaban en el código, se logro cumplir con los objetivos que tuvimos planeados al incio de esta práctica, y gracias al tutorial de Skills Network, se pudo llevar adelante el proyecto de manera exitosa.

## Enlace utilizado

> **IMPORTANTE**
> La elaboración de este curso fue desarrollado mediante el siguiente link de la plataforma llamada `Skills Netowrk` :

- [Skills Netowrk](https://acortar.link/2cbbrE)