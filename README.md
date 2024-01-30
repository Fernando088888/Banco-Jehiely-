<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Banco Jehiely Bermeo</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #e3f2fd;
            margin: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
        }

        .container {
            text-align: center;
            background-color: #00417d;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
            max-width: 400px;
            width: 100%;
            color: #fff;
            margin: 20px;
        }

        h1 {
            color: #f8f8f8;
            margin-bottom: 20px;
        }

        form {
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        label {
            font-size: 18px;
            margin-bottom: 10px;
            color: #f8f8f8;
        }

        input {
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
        }

        button {
            background-color: #4c84b3;
            color: #fff;
            padding: 10px 20px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin-bottom: 10px;
        }

        button:hover {
            background-color: #3a5a7f;
        }

        #numeroActual {
            font-size: 24px;
            font-weight: bold;
            color: #4c84b3;
            margin-bottom: 10px;
        }

        #historial {
            text-align: left;
            margin-top: 20px;
        }

        #nuevoMesBtn {
            background-color: #f44336;
            color: #fff;
            padding: 10px 20px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        #nuevoMesBtn:hover {
            background-color: #d32f2f;
        }
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
        // (Código JavaScript existente...)

        function actualizarPagina() {
            // Obtener saldo e historial actualizados
            saldoActual = obtenerSaldoGuardado();
            historialTransferencias = obtenerHistorialTransferencias();

            // Actualizar la visualización del saldo e historial
            actualizarSaldo();
            actualizarHistorial();
        }

        // Agregar la función para actualizar la página automáticamente cada 5 segundos
        setInterval(actualizarPagina, 5000); // Actualizar cada 5 segundos (puedes ajustar el intervalo según tus preferencias)
    </script>
</body>
</html>
