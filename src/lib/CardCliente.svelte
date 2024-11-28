<script>
  let clientes = JSON.parse(sessionStorage.getItem("clientes")) || []; // Inicializamos correctamente como array
  let cargando = true;
  let valor = "";
  let clientesEliminados =
    JSON.parse(sessionStorage.getItem("clientesEliminados")) || []; // Traemos los clientes eliminados, y los convertimos en objeto con la funcion parse

  // Función asíncrona para cargar los datos
  const cargarClientes = async () => {
    try {
      const response = await fetch("/clientes.json");
      clientes = await response.json();

      // Filtramos los clientes eliminados para no mostrarlos
      clientes = clientes.filter(
        (cliente) => !clientesEliminados.includes(cliente.nombre)
      );

      cargando = false; // Cambiar el estado a no cargando una vez que los datos están listos
    } catch (error) {
      console.error("Error al cargar los datos:", error);
      cargando = false;
    }
  };

  // Llamamos a la función para cargar los datos nada más empezar
  cargarClientes();

  // Filtra los elementos según la búsqueda
  const clientesFiltrados = () => {
    return clientes.filter((cliente) =>
      cliente.nombre.toLowerCase().includes(valor.toLowerCase())
    );
  };

  let ventanaActiva = false;
  let overlayActivo = false;
  let clienteSeleccionado = null;
  let ventanaEditarActiva = false;

  function mostrarInfo(cliente) {
    clienteSeleccionado = cliente;
    overlayActivo = true;
    ventanaActiva = true;
  }

  function cerrarVentana() {
    ventanaActiva = false;
    overlayActivo = false;
    clienteSeleccionado = null; // Reseteamos el cliente seleccionado
  }

  // Función para eliminar un cliente del array
  function eliminarCliente(clienteNombre) {
    alert("Se ha eliminado el cliente: " + clienteNombre);
    // Agregar el cliente a la lista de eliminados en sessionStorage
    clientesEliminados.push(clienteNombre);
    sessionStorage.setItem(
      "clientesEliminados",
      JSON.stringify(clientesEliminados)
    );

    // Filtrar el cliente a eliminar (sin mutar directamente el array original)
    clientes = clientes.filter((cliente) => cliente.nombre !== clienteNombre);

    console.log("Clientes después de eliminar:", clientes); // Verifica el array después de la eliminación

    window.location.reload();
  }

  let nombreEditado = "";
  let edadEditado = "";
  let paisEditado = "";

  function actualizarCliente(e, cliente) {
    e.preventDefault();
    // Actualizamos el cliente con los nuevos valores
    const indice = clientes.findIndex((c) => c.nombre === cliente.nombre);

    if (indice !== -1) {
      // Actualizamos los valores del cliente en el array
      clientes[indice].nombre = nombreEditado;
      clientes[indice].edad = edadEditado;
      clientes[indice].pais = paisEditado;

      // Guardar el array actualizado en sessionStorage
      sessionStorage.setItem("clientes", JSON.stringify(clientes));

      // Actualizar la vista sin recargar la página
      ventanaEditarActiva = false; // Cerrar la ventana de edición
      clienteSeleccionado = null; // Limpiar el cliente seleccionado

      // Aquí puedes realizar una actualización de la interfaz sin recargar la página
      // Esto asegurará que el cliente actualizado se muestre en la interfaz
      console.log("Cliente actualizado:", clientes[indice]);
    } else {
      console.error("Cliente no encontrado");
    }
  }
</script>

<div id="contenedor-buscador">
  <img
    src="/img/search_24dp_CDCDCD_FILL0_wght400_GRAD0_opsz24.png"
    alt=""
    width="24"
    height="24"
  />
  <input
    type="text"
    bind:value={valor}
    placeholder="Buscar..."
    id="buscador-input"
  />
</div>

<div id="contenedor-principal">
  {#if cargando}
    <p>Cargando clientes...</p>
    <!-- Mensaje mientras se cargan los datos -->
  {:else}
    {#each clientesFiltrados() as cliente}
      <div id="card" class="cards">
        <div class="foto-texto" on:click={() => mostrarInfo(cliente)}>
          <img src={cliente.foto} alt="" width="54" height="54" />
          <div class="texto">
            <h3 class="nombre">{cliente.nombre}</h3>
            <p class="direccion">{cliente.pais}</p>
          </div>
        </div>

        <div class="iconos">
          <img
            src="/img/delete_24dp_EA3323_FILL0_wght400_GRAD0_opsz24 (1).png"
            alt="Eliminar"
            on:click={() => eliminarCliente(cliente.nombre)}
          />
        </div>
      </div>
    {/each}
  {/if}
</div>

<!-- Ventana que muestra la información del cliente -->
{#if ventanaActiva}
  <div id="ventana" class="activo">
    <div id="linea-oculta"></div>
    <p>Nombre: {clienteSeleccionado.nombre}</p>
    <p>Edad: {clienteSeleccionado.edad}</p>
    <p>País: {clienteSeleccionado.pais}</p>
    <button on:click={cerrarVentana}>Cerrar Ventana</button>
  </div>
{/if}

<div
  id="overlay"
  class={overlayActivo ? "overlay-activo" : "o verlay-inactivo"}
></div>
