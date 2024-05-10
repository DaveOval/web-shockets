<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Websockets - <small>Status</small></h1>

    <form >
        <input type="text" placeholder="enviar mensaje" >
        <button>Enviar</button>
    </form>

    <ul id="messages">

    </ul>

    <script>
        let socket = null;
        const form = document.querySelector('form');
        const input = document.querySelector('input');
        const messagesElement = document.querySelector('#messages');
        const statusElement = document.querySelector('small');

        function sendMessage( message ) {
            socket && socket.send(message);
        }

        function renderMessage( message ) {
            const li = document.createElement('li');
            li.innerHTML = message;
            messagesElement.prepend(li);
        }

        form.addEventListener("submit", ( event ) => {
            event.preventDefault();
            sendMessage(input.value);
            input.value = "";
        });  

        function connectToServer() {

            socket = new WebSocket("ws://localhost:3000");

            socket.onopen = ( event ) => {
                statusElement.innerText = "Conectado";
            }

            socket.onclose = ( event ) => {
                statusElement.innerText = "Desconectado";
                setTimeout(() => {
                    connectToServer();
                }, 1500);
            }

            socket.onmessage = (event) => {
                const { payload } = JSON.parse(event.data);
                renderMessage( payload );
            }   

        }
        
        connectToServer();


    </script>
</body>
</html>