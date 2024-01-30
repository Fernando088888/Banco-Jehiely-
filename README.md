<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Banco Jehiely Bermeo</title>
    <style>
        /* ... (Estilos anteriores) ... */

        #passwordInput {
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
        }

        #loginBtn {
            background-color: #4caf50;
            color: #fff;
            padding: 10px 20px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin-bottom: 10px;
        }

        #loginBtn:hover {
            background-color: #45a049;
        }

        #passwordSection {
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Banco Jehiely Bermeo</h1>

        <div id="passwordSection">
            <label for="passwordInput">Ingrese la contraseña:</label>
            <input type="password" id="passwordInput" placeholder="Ingrese la contraseña" required>
            <button id="loginBtn" onclick="login()">Ingresar</button>
        </div>

        <form id="transactionForm" style="display: none;">
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
        let saldoActual = obtenerSaldoGuardado();
        let historialTransferencias = obtenerHistorialTransferencias();
        let passwordCorrecta = false;

        const saldoElemento = document.getElementById('numeroActual');
        const listaTransferenciasElemento = document.getElementById('listaTransferencias');
        const passwordSection = document.getElementById('passwordSection');
        const transactionForm = document.getElementById('transactionForm');

        // Puedes establecer tu contraseña aquí
        const contraseñaCorrecta = '6996';

        function login() {
            const passwordInput = document.getElementById('passwordInput').value;
            if (passwordInput === contraseñaCorrecta) {
                passwordCorrecta = true;
                passwordSection.style.display = 'none';
                transactionForm.style.display = 'flex';
            } else {
                alert('Contraseña incorrecta');
            }
        }

        function sumarNumero() {
            if (passwordCorrecta) {
                realizarOperacion("Depósito");
            } else {
                alert('Por favor, inicie sesión primero.');
            }
        }

        function restarNumero() {
            if (passwordCorrecta) {
                realizarOperacion("Retiro");
            } else {
                alert('Por favor, inicie sesión primero.');
            }
        }

        function realizarOperacion(tipo) {
            const montoIngresado = document.getElementById('numeroInput').value;

            if (!isNaN(montoIngresado) && montoIngresado !== '') {
                const monto = parseInt(montoIngresado);
                if (tipo === "Retiro" && monto > saldoActual) {
                    alert('Saldo insuficiente para realizar el retiro');
                    return;
                }

                saldoActual = tipo === "Depósito" ? saldoActual + monto : saldoActual - monto;

                const operacion = {
                    tipo: tipo,
                    monto: monto,
                    fecha: new Date().toLocaleString()
                };
                historialTransferencias.push(operacion);
                localStorage.setItem('historialTransferencias', JSON.stringify(historialTransferencias));
                localStorage.setItem('saldoGuardado', saldoActual);

                document.getElementById('numeroInput').value = '';

                actualizarSaldo();
                actualizarHistorial();
            } else {
                alert('Ingrese un monto válido');
            }
        }

        function obtenerSaldoGuardado() {
            return parseInt(localStorage.getItem('saldoGuardado')) || 0;
        }

        function obtenerHistorialTransferencias() {
            return JSON.parse(localStorage.getItem('historialTransferencias')) || [];
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
            historialTransferencias = [];
            localStorage.removeItem('historialTransferencias');
            actualizarHistorial();
        }
    </script>
</body>
</html>

