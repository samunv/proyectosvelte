<script>
  // Estado inicial del tema
  let temaCambiado = false;

  //Src inicial del icono
  let srcIconoTema = "/img/wb_sunny_24dp_WHITE_FILL0_wght400_GRAD0_opsz24.png";

  // Función para cambiar el tema
  function cambiarTema() {
    // Alternamos el estado del tema
    temaCambiado = !temaCambiado;

    // Accedemos al elemento :root
    const root = document.documentElement;

    // Cambiamos las propiedades CSS personalizadas según el estado
    if (temaCambiado) {
      srcIconoTema = "/img/dark_mode_24dp_BLACK_FILL0_wght400_GRAD0_opsz24.png";
      root.style.setProperty("--main-color", "white");
      root.style.setProperty("--secondary-color", "#E3E3E3");
      root.style.setProperty("--text-color", "black");
    } else {
      srcIconoTema = "/img/wb_sunny_24dp_WHITE_FILL0_wght400_GRAD0_opsz24.png";
      root.style.setProperty("--main-color", "#333336");
      root.style.setProperty("--secondary-color", "black");
      root.style.setProperty("--text-color", "white");
    }
  }
</script>

<header>
  <h1>Buscador de Clientes</h1>

  <div id="contenedor-cambiar-tema">
    <p>Cambiar a </p>
    
    <img
      src={srcIconoTema}
      alt="Cambiar tema"
      on:click={() => cambiarTema()}
      width="30"
      height="30"
    />
  </div>
</header>
