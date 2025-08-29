<!DOCTYPE html>
<html lang="es">
<head>
 <meta charset="UTF-8" />
 <title>Expediente Clínico</title>
 <style>
  body {
   font-family: Arial, sans-serif;
   background-image: url('IMAGEN.png');
   background-size: cover;
   background-position: center;
   background-repeat: no-repeat;
   margin: 0;
   padding: 0;
  }
  .container {
   width: 90%;
   max-width: 700px;
   margin: 30px auto;
   background: #345dcf;
   padding: 20px;
   border-radius: 10px;
   box-shadow: 0px 4px 8px rgba(0,0,0,0.2);
   color: white;
  }
  h2 {
   text-align: center;
   color: white;
  }
  label {
   display: block;
   margin-top: 10px;
   font-weight: bold;
   color: white;
  }
  input, select {
   width: 100%;
   padding: 8px;
   margin-top: 5px;
   border: 1px solid #ccc;
   border-radius: 5px;
   box-sizing: border-box;
  }
  .hidden {
   display: none;
  }
  button {
   margin-top: 15px;
   width: 100%;
   padding: 10px;
   background: #4CAF50;
   color: white;
   border: none;
   border-radius: 5px;
   cursor: pointer;
  }
  button:hover {
   background: #45a049;
  }
  #mensajeFinal {
    text-align: center;
    font-size: 1.2em;
    font-weight: bold;
    color: #00ff00;
  }
  #historial {
    margin-top: 20px;
    max-height: 300px;
    overflow-y: auto;
    background: #223a8f;
    padding: 10px;
    border-radius: 8px;
  }
  #historial h3 {
    margin-top: 0;
  }
  #historial table {
    width: 100%;
    border-collapse: collapse;
    color: white;
  }
  #historial th, #historial td {
    border: 1px solid #ccc;
    padding: 6px;
    text-align: left;
  }
  #historial th {
    background: #1a2e6a;
  }
  .btn-group button {
    width: 48%;
    display: inline-block;
  }
 </style>
</head>
<body>

 <!-- LOGIN -->
 <div class="container" id="loginBox">
  <h2>REGISTRO CLÍNICO</h2>
  <label for="user">Usuario:</label>
  <input type="text" id="user" required />

  <label for="pass">Contraseña:</label>
  <input type="password" id="pass" required />
  
  <button onclick="login()">Entrar</button>
 </div>

 <!-- FORMULARIO -->
 <div class="container hidden" id="formBox">
  <h2>EXPEDIENTE CLÍNICO</h2>
  
  <h3>Datos Generales</h3>
  <label>Nombre:</label>
  <input type="text" id="nombre" />

  <label>Edad:</label>
  <input type="number" id="edad" />

  <label>Fecha de Nacimiento:</label>
  <input type="date" id="fechaNacimiento" />

  <label>Fecha de Ingreso:</label>
  <input type="date" id="fechaIngreso" />
  
  <label>Expediente:</label>
  <input type="text" id="expediente" />
  
  <label>Diagnóstico:</label>
  <input type="text" id="diagnostico" />
  
  <h3>Sección para Menores de Edad</h3>
  <label>Padre:</label>
  <input type="text" id="padre" />
  <label>Edad:</label>
  <input type="number" id="edadPadre" />
  
  <label>Madre:</label>
  <input type="text" id="madre" />
  <label>Edad:</label>
  <input type="number" id="edadMadre" />
  
  <label>Tutor(a) Legal:</label>
  <input type="text" id="tutor" />
  <label>Edad:</label>
  <input type="number" id="edadTutor" />

  <h3>Datos de Localización</h3>
  
  <label for="codigoPais">Código de País:</label>
  <select id="codigoPais" onchange="actualizarLongitudTelefono()">
    <option value="+51" data-length="9">Perú (+51)</option>
    <option value="+1" data-length="10">Estados Unidos (+1)</option>
    <option value="+52" data-length="10">México (+52)</option>
    <option value="+34" data-length="9">España (+34)</option>
    <option value="+44" data-length="10">Reino Unido (+44)</option>
  </select>

  <label>Teléfono:</label>
  <input type="tel" id="telefono" placeholder="Número sin código" />

  <label>Domicilio:</label>
  <input type="text" id="domicilio" />

  <button onclick="guardarRegistro()">Guardar</button>
 </div>

 <!-- PANTALLA FINAL -->
 <div class="container hidden" id="finalBox">
  <h2>Registro Completo</h2>
  <p id="mensajeFinal">El expediente médico ha sido registrado con éxito.</p>
  <div class="btn-group">
    <button onclick="nuevoRegistro()">Registrar otro expediente</button>
    <button onclick="cerrarSesion()">Finalizar</button>
  </div>

  <div id="historial">
    <h3>Historial de Expedientes</h3>
    <table id="tablaHistorial">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Edad</th>
          <th>Fecha Nac.</th>
          <th>Fecha Ingreso</th>
          <th>Expediente</th>
          <th>Diagnóstico</th>
          <th>Teléfono</th>
          <th>Domicilio</th>
        </tr>
      </thead>
      <tbody>
        <!-- Aquí se añaden los registros -->
      </tbody>
    </table>
  </div>
 </div>

 <script>
  const historialExpedientes = [];

  function login() {
   const user = document.getElementById("user").value;
   const pass = document.getElementById("pass").value;

   if (user === "604751" && pass === "6047") {
    document.getElementById("loginBox").classList.add("hidden");
    document.getElementById("formBox").classList.remove("hidden");
    actualizarLongitudTelefono();
   } else {
    alert("Usuario o contraseña incorrectos");
   }
  }

  function actualizarLongitudTelefono() {
    const selectPais = document.getElementById("codigoPais");
    const telefonoInput = document.getElementById("telefono");

    const opcionSeleccionada = selectPais.options[selectPais.selectedIndex];
    const longitud = opcionSeleccionada.getAttribute("data-length");

    telefonoInput.value = "";
    telefonoInput.setAttribute("maxlength", longitud);
    telefonoInput.setAttribute("minlength", longitud);
    telefonoInput.placeholder = `Número de ${longitud} dígitos sin código`;
  }

  function guardarRegistro() {
    // Validaciones teléfono
    const codigoPais = document.getElementById("codigoPais").value;
    const telefonoInput = document.getElementById("telefono");
    const telefono = telefonoInput.value.trim();
    const longitudEsperada = parseInt(telefonoInput.getAttribute("maxlength"));

    if (telefono === "") {
      alert("Por favor, ingresa un número de teléfono.");
      return;
    }
    const soloDigitos = /^\d+$/;
    if (!soloDigitos.test(telefono)) {
      alert("El teléfono debe contener solo números.");
      return;
    }
    if (telefono.length !== longitudEsperada) {
      alert(`El número de teléfono debe tener exactamente ${longitudEsperada} dígitos.`);
      return;
    }

    // Recolectar datos
    const registro = {
      nombre: document.getElementById("nombre").value.trim(),
      edad: document.getElementById("edad").value.trim(),
      fechaNacimiento: document.getElementById("fechaNacimiento").value,
      fechaIngreso: document.getElementById("fechaIngreso").value,
      expediente: document.getElementById("expediente").value.trim(),
      diagnostico: document.getElementById("diagnostico").value.trim(),
      padre: document.getElementById("padre").value.trim(),
      edadPadre: document.getElementById("edadPadre").value.trim(),
      madre: document.getElementById("madre").value.trim(),
      edadMadre: document.getElementById("edadMadre").value.trim(),
      tutor: document.getElementById("tutor").value.trim(),
      edadTutor: document.getElementById("edadTutor").value.trim(),
      codigoPais,
      telefono: telefono,
      domicilio: document.getElementById("domicilio").value.trim(),
    };

    // Agregar al historial
    historialExpedientes.push(registro);

    // Mostrar pantalla final y actualizar tabla historial
    document.getElementById("formBox").classList.add("hidden");
    document.getElementById("finalBox").classList.remove("hidden");
    actualizarHistorial();
  }

  function actualizarHistorial() {
    const tbody = document.querySelector("#tablaHistorial tbody");
    tbody.innerHTML = ""; // Limpiar tabla

    historialExpedientes.forEach((reg, index) => {
      const tr = document.createElement("tr");

      tr.innerHTML = `
        <td>${reg.nombre}</td>
        <td>${reg.edad}</td>
        <td>${reg.fechaNacimiento}</td>
        <td>${reg.fechaIngreso}</td>
        <td>${reg.expediente}</td>
        <td>${reg.diagnostico}</td>
        <td>${reg.codigoPais} ${reg.telefono}</td>
        <td>${reg.domicilio}</td>
      `;

      tbody.appendChild(tr);
    });
  }

  function nuevoRegistro() {
    // Limpiar formulario y regresar a la pantalla de formulario
    document.getElementById("finalBox").classList.add("hidden");
    document.getElementById("formBox").classList.remove("hidden");

    // Limpiar inputs
    document.querySelectorAll("#formBox input").forEach(input => input.value = "");
    document.getElementById("codigoPais").selectedIndex = 0;
    actualizarLongitudTelefono();
  }

  function cerrarSesion() {
    // Ocultar todo y mostrar login
    document.getElementById("finalBox").classList.add("hidden");
    document.getElementById("loginBox").classList.remove("hidden");

    // Limpiar todo
    historialExpedientes.length = 0;
    document.querySelectorAll("#formBox input").forEach(input => input.value = "");
    document.getElementById("codigoPais").selectedIndex = 0;
  }
 </script>

</body>
</html>
