<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Banco Jehiely Bermeo</title>
    <style>
        /* Estilos CSS existentes... */
    </style>
</head>
<body>
    <div class="container">
        <h1>Banco Jehiely Bermeo</h1>
        
        <form>
            <label for="numeroInput">Ingrese un monto:</label>
            <input type="number" id="numeroInput" placeholder="Ingrese el monto" required>
            <button type="button" onclick="sumarNumero()">Depositar</button>
            <button type="button" onclick="restarNumero()">Retirar</button>
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
        actualizarSaldo();
        actualizarHistorial();

        function sumarNumero() {
            realizarOperacion("Depósito");
        }

        function restarNumero() {
            realizarOperacion("Retiro");
        }

        function realizarOperacion(tipo) {
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
        }

        function obtenerSaldoGuardado() {
            // Obtener el saldo guardado desde localStorage
            return parseInt(localStorage.getItem('saldoGuardado')) || 0;
        }

        function obtenerHistorialTransferencias() {
            // Obtener el historial de transferencias desde localStorage
            return JSON.parse(localStorage.getItem('historialTransferencias')) || [];
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

        // Agregar la función para actualizar la página automáticamente cada 5 segundos
        setInterval(function() {
            // Obtener saldo e historial actualizados
            saldoActual = obtenerSaldoGuardado();
            historialTransferencias = obtenerHistorialTransferencias();

            // Actualizar la visualización del saldo e historial
            actualizarSaldo();
            actualizarHistorial();
        }, 5000); // Actualizar cada 5 segundos (puedes ajustar el intervalo según tus preferencias)
    </script>
</body>
</html>
