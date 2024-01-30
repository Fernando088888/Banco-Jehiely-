<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Banco Jehiely Bermeo</title>
    <style>
        /* Estilos CSS aquí... */
    </style>
</head>
<body>
    <div class="container">
        <h1>Banco Jehiely Bermeo</h1>
        
        <form>
            <label for="numeroInput">Ingrese un monto:</label>
            <input type="number" id="numeroInput" placeholder="Ingrese el monto" required>
            <button type="button" onclick="realizarOperacion('Depósito')">Depositar</button>
            <button type="button" onclick="realizarOperacion('Retiro')">Retirar</button>
        </form>

        <p>Saldo actual: <span id="numeroActual">0</span> USD</p>

        <div id="historial">
            <h2>Transferencias</h2>
            <ul id="listaTransferencias"></ul>
        </div>

        <button id="nuevoMesBtn" onclick="borrarHistorial()">Nuevo mes</button>
    </div>

    <script>
        let saldoActual = obtenerSaldoGuardado(); // Obtener el saldo guardado al cargar la página
        let historialTransferencias = obtenerHistorialTransferencias(); // Obtener el historial de transferencias

        const saldoElemento = document.getElementById('numeroActual');
        const listaTransferenciasElemento = document.getElementById('listaTransferencias');
        const nuevoMesBtn = document.getElementById('nuevoMesBtn');
        const accessCode = obtenerParametroUrl('accessCode');

        // Verificar acceso directo sin contraseña
        if (accessCode === '1234') {
            // Permitir acceso directo sin contraseña
            permitirAccesoDirecto();
        }

        actualizarSaldo();
        actualizarHistorial();

        function sumarNumero() {
            realizarOperacion("Depósito");
        }

        function restarNumero() {
            realizarOperacion("Retiro");
        }

        function realizarOperacion(tipo) {
            if (verificarAcceso()) {
                const montoIngresado = document.getElementById('numeroInput').value;

                if (!isNaN(montoIngresado) && montoIngresado !== '') {
                    // Realizar la operación (suma o resta) al saldo guardado
                    const monto = parseInt(montoIngresado);
                    if (tipo === "Retiro" && monto > saldoActual) {
                        alert('Saldo insuficiente para realizar el retiro');
                        return;
                    }

                    saldoActual = tipo === "Depósito" ? saldoActual + monto : saldoActual - monto;

                    // Guardar la operación en el historial
                    const operacion = {
                        tipo: tipo,
                        monto: monto,
                        fecha: new Date().toLocaleString()
                    };
                    historialTransferencias.push(operacion);
                    localStorage.setItem('historialTransferencias', JSON.stringify(historialTransferencias));

                    // Guardar el nuevo saldo en localStorage
                    localStorage.setItem('saldoGuardado', saldoActual);

                    // Limpiar el campo de entrada
                    document.getElementById('numeroInput').value = '';

                    // Actualizar el contenido del elemento
                    actualizarSaldo();
                    actualizarHistorial();
                } else {
                    alert('Ingrese un monto válido');
                }
            } else {
                // Mostrar formulario de contraseña
                mostrarFormularioContraseña();
            }
        }

        function permitirAccesoDirecto() {
            // Lógica para permitir acceso directo sin contraseña
            // Puedes realizar otras acciones según tus necesidades
            console.log('Acceso directo permitido sin contraseña');
        }

        function verificarAcceso() {
            // Verificar si se ingresó la contraseña o si es acceso directo
            return obtenerContraseñaIngresada() === '6996' || accessCode === '1234';
        }

        function obtenerContraseñaIngresada() {
            // Obtener la contraseña ingresada por el usuario
            return prompt('Ingrese la contraseña:');
        }

        function mostrarFormularioContraseña() {
            // Mostrar formulario de contraseña si no se proporcionó acceso directo
            const contraseñaIngresada = obtenerContraseñaIngresada();
            if (contraseñaIngresada === '6996') {
                // Contraseña correcta, permitir acceso
                alert('Contraseña correcta. Acceso permitido.');
            } else {
                // Contraseña incorrecta, mostrar mensaje y no permitir acceso
                alert('Contraseña incorrecta. Acceso denegado.');
            }
        }

        function obtenerSaldoGuardado() {
            // Obtener el saldo guardado desde localStorage
            return parseInt(localStorage.getItem('saldoGuardado')) || 0;
        }

        function obtenerHistorialTransferencias() {
            // Obtener el historial de transferencias desde localStorage
            return JSON.parse(localStorage.getItem('historialTransferencias')) || [];
        }

        function obtenerParametroUrl(parametro) {
            // Obtener un parámetro específico de la URL
            const urlParams = new URLSearchParams(window.location.search);
            return urlParams.get(parametro);
        }

        function actualizarSaldo() {
            // Actualizar el contenido del elemento con el saldo actual
            saldoElemento.textContent = saldoActual;
        }

        function actualizarHistorial() {
            // Limpiar la lista de transferencias antes de actualizarla
            listaTransferenciasElemento.innerHTML = '';

            // Construir y mostrar la lista de transferencias
            historialTransferencias.forEach(operacion => {
                const itemLista = document.createElement('li');
                itemLista.textContent = `${operacion.tipo} de ${operacion.monto} USD - ${operacion.fecha}`;
                listaTransferenciasElemento.appendChild(itemLista);
            });
        }

        function borrarHistorial() {
            // Borrar el historial y actualizar la vista
            historialTransferencias = [];
            localStorage.removeItem('historialTransferencias');
            actualizarHistorial();
        }
    </script>
</body>
</html>
