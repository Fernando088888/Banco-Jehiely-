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
        let saldoActual = 0;
        let historialTransferencias = [];

        const saldoElemento = document.getElementById('numeroActual');
        const listaTransferenciasElemento = document.getElementById('listaTransferencias');

        const socket = new WebSocket('ws://tu-servidor-websocket'); // Reemplaza 'tu-servidor-websocket' con la URL de tu servidor WebSocket

        socket.onmessage = function(event) {
            const data = JSON.parse(event.data);
            if (data.type === 'update') {
                // Actualiza los datos recibidos del servidor
                saldoActual = data.saldo;
                historialTransferencias = data.historial;

                // Actualiza la visualización
                actualizarSaldo();
                actualizarHistorial();
            }
        };

        function sumarNumero() {
            realizarOperacion("Depósito");
        }

        function restarNumero() {
            realizarOperacion("Retiro");
        }

        function realizarOperacion(tipo) {
            const montoIngresado = document.getElementById('numeroInput').value;

            if (!isNaN(montoIngresado) && montoIngresado !== '') {
                const monto = parseInt(montoIngresado);

                socket.send(JSON.stringify({
                    type: 'operation',
                    operation: {
                        tipo: tipo,
                        monto: monto,
                        fecha: new Date().toLocaleString()
                    }
                }));
            } else {
                alert('Ingrese un monto válido');
            }
        }

        function actualizarSaldo() {
            saldoElemento.textContent = saldoActual;
        }

        function actualizarHistorial() {
            listaTransferenciasElemento.innerHTML = '';

            historialTransferencias.forEach(operacion => {
                const itemLista = document.createElement('li');
                itemLista.textContent = `${operacion.tipo} de ${operacion.monto} USD - ${operacion.fecha}`;
                listaTransferenciasElemento.appendChild(itemLista);
            });
        }

        function borrarHistorial() {
            socket.send(JSON.stringify({ type: 'clearHistory' }));
        }
    </script>
</body>
</html>
