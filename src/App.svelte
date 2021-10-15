<script>
  /*import { onMount } from "svelte";

  let myTodo;
  onMount(async () => {
    const response = await fetch("http://localhost:3000/info");
    const todo = await response.json();
    myTodo = todo;
  });*/
  let datos = [];

  let datosAgregar = {
    id:null,
    estado: "",
    tamano: "",
    coordenadax: "",
    coordenaday: "",
  };

  /*Agregar Datos*/
  let agregarDatos = async () => {
    const nuevoDato = {
      estado: datosAgregar.estado,
      tamano: datosAgregar.tamano,
      coordenadax: datosAgregar.coordenadax,
      coordenaday: datosAgregar.coordenaday,
    };
    fetch("http://localhost:3000/info/", {
      method: "POST",
      body: JSON.stringify(nuevoDato),
      headers: {
        "Content-Type": "application/json",
      },
    }).then((respuesta) => respuesta.json());

    mostrarDatos();
  };

  /*mostrar empleados*/
  let mostrarDatos = () => {
    fetch("http://localhost:3000/info")
      .then((respuesta) => respuesta.json())
      .then((datosRespuesta) => {
        datos = datosRespuesta;
        datosAgregar = {
          estado: "",
          tamano: "",
        };
        console.log(datos);
      })
      .catch(console.log);
  };

  /**borrar*/
  let borrarDato = (id) => {
    fetch("http://localhost:3000/info/" + id, {
      method: "DELETE",
    })
      .then((respuesta) => respuesta.json())
      .then((datosRespuesta) => {
        mostrarDatos();
      })
      .catch(console.log);
  };

  /*Actualizar*/
  let editarDato = (datos) => {
    datosAgregar = datos;
  };

  let actualizarDato = () => {
    fetch("http://localhost:3000/info/" + datosAgregar.id, {
      method: "PUT",
      body: JSON.stringify(datosAgregar),
      headers: {
        "Content-Type": "application/json",
      },
    }).then((respuesta) => respuesta.json());

    mostrarDatos();
  };

  /*Ejecutar funciones*/
  mostrarDatos();
</script>

<div>
  <h1 class="bg-success">WATER WORLD</h1>
  <div class="container text-center">
    <form action="">
      <div class="row">
        <div class="col-6 p-1">
          <input
            type="text"
            class="form-control"
            bind:value={datosAgregar.tamano}
            placeholder="tamaño"
          />
        </div>
        <div class="col-6 p-1">
          <input
            type="text"
            class="form-control"
            bind:value={datosAgregar.estado}
            placeholder="estado"
          />
        </div>
        <div class="col-6 p-1">
          <input
            type="text"
            class="form-control"
            bind:value={datosAgregar.coordenadax}
            placeholder="coordenada X"
          />
        </div>
        <div class="col-6 p-1">
          <input
            type="text"
            class="form-control"
            bind:value={datosAgregar.coordenaday}
            placeholder="coordenada Y"
          />
        </div>
      </div>
      <button
        class="btn btn-success"
        type="button"
        on:click|preventDefault={agregarDatos}>Enviar nuevos datos</button
      >
      <button
        class="btn btn-success"
        type="button"
        on:click|preventDefault={actualizarDato}>Actualizar datos</button
      >
    </form>
    {#if datos}
      <div class="row">
        {#each datos as water}
          <div
            class="card bg-transparent border col-6 col-md-4 m-2"
            style="width: 18rem;"
          >
            <!--<img src="..." class="card-img-top" alt="...">-->
            <div class="card-body bg-transparent">
              <h5 class="card-title text-info">Informacion:</h5>
              <p class="card-text">Tamaño:{water.tamano}</p>
            </div>
            <ul class="list-group list-group-flush bg-transparent">
              <li class="list-group-item bg-transparent text-warning">
                Id:{water.id}
              </li>
              <li class="list-group-item bg-transparent text-warning">
                CORDENADA X:{water.coordenadax}
              </li>
              <li class="list-group-item bg-transparent text-warning">
                CORDENADA Y:{water.coordenaday}
              </li>
              <li class="list-group-item bg-transparent text-warning">
                ESTADO: {water.estado}
              </li>
              <button
                type="submit"
                class="btn btn-sm btn-danger m-2"
                on:click={borrarDato(water.id)}>eliminar</button
              >
              <button
                type="submit"
                class="btn btn-sm btn-success m-2"
                on:click={editarDato(water)}>editar</button
              >
            </ul>
          </div>
          <br />
        {/each}
      </div>
    {:else}
      <p>loading.....</p>
    {/if}
  </div>
</div>
