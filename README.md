Geniuses Of Eleectric and Solutions
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Banco Jehiely Bermeo</title>
    <style>
        /* Estilos... */
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

        <form id="transactionForm">
            <label for="numeroInput">Ingrese un monto:</label>
            <input type="number" id="numeroInput" placeholder="Ingrese el monto" required>
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

        const saldoElemento = document.getElementById('numeroActual');
        const listaTransferenciasElemento = document.getElementById('listaTransferencias');
        const nuevoMesBtn = document.getElementById('nuevoMesBtn');
        const passwordSection = document.getElementById('passwordSection');
        const transactionForm = document.getElementById('transactionForm');

        const urlParams = new URLSearchParams(window.location.search);
        const fromQR = urlParams.get('fromQR');

        console.log('fromQR:', fromQR);

        if (fromQR === 'true') {
            console.log('Abriendo desde código QR...');
            passwordSection.style.display = 'none';
            transactionForm.style.display = 'block';
        }

        actualizarSaldo();
        actualizarHistorial();

        function login() {
            const passwordInput = document.getElementById('passwordInput').value;
            if (passwordInput === '6996') {
                passwordSection.style.display = 'none';
                transactionForm.style.display = 'block';
            } else {
                alert('Contraseña incorrecta');
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

                saldoActual = saldoActual - monto;

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
