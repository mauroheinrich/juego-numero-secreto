# Juego del Número Secreto

Este es un juego simple en JavaScript donde el usuario debe adivinar un número secreto generado aleatoriamente por el programa. El usuario ingresa un número y el programa le indica si el número secreto es mayor, menor o si ha acertado.
## Despliegue

Puedes probar el juego en vivo en el siguiente enlace: [Juego del Número Secreto](https://mauroheinrich.github.io/juego-numero-secreto/)

## Estructura del Proyecto

El proyecto está compuesto por los siguientes archivos:

- `index.html`: Contiene la estructura básica del HTML para la interfaz del usuario.
- `style.css`: Archivo CSS para el diseño y estilos de la interfaz.
- `app.js`: Archivo JavaScript que contiene la lógica del juego.

## Requisitos

Para ejecutar este proyecto, necesitas un navegador web moderno.

## Uso

1. **Clona el repositorio:**

    ```bash
    git clone https://github.com/mauroheinrich/juego-numero-secreto.git
    cd juego-numero-secreto
    ```

2. **Abre el archivo `index.html` en tu navegador:**

    ```bash
    open index.html
    ```

3. **Cómo jugar:**

    - Ingresa un número del 1 al 10 en el campo de entrada.
    - Haz clic en el botón "Intentar" para verificar si adivinaste el número secreto.
    - El programa te indicará si el número secreto es mayor o menor.
    - Si aciertas, el programa te mostrará en cuántos intentos lo lograste y habilitará el botón "Nuevo juego" para reiniciar el juego.

## Estructura del Código

### HTML

El archivo `index.html` contiene la estructura básica de la interfaz:

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">
    <title>JS Game</title>
</head>
<body>
    <div class="container">
        <div class="container__contenido">
            <div class="container__informaciones">
                <div class="container__texto">
                    <h1></h1>
                    <p class="texto__parrafo"></p>
                </div>
                <input type="number" id="valorUsuario" min="1" max="10" class="container__input">
                <div class="chute container__botones">
                    <button onclick="verificarIntento();" class="container__boton">Intentar</button>
                    <button onclick="reiniciarJuego();" class="container__boton" id="reiniciar" disabled>Nuevo juego</button>
                </div>
            </div>
            <img src="./img/ia.png" alt="Una persona mirando a la izquierda" class="container__imagen-persona" />
        </div>
    </div>
    <script src="app.js" defer></script>
</body>
</html>
### CSS
El archivo style.css contiene los estilos para la interfaz:
/* style.css */
body {
    font-family: 'Inter', sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
}

.container {
    text-align: center;
}

.container__input {
    margin: 10px 0;
}

.container__botones {
    display: flex;
    justify-content: center;
    gap: 10px;
}
### JavaScript
El archivo app.js contiene la lógica del juego:
let numeroSecreto = 0;
let intentos = 0;
let listaNumerosSorteados = [];
let numeroMaximo = 10;

console.log('Número Secreto al inicio:', numeroSecreto);

function asignarTextoElemento(selector, texto) {
    let elementoHtml = document.querySelector(selector);
    if(elementoHtml){
        elementoHtml.innerHTML = texto;
        console.log(`Texto asignado a ${selector}: ${texto}`);
    } else {
        console.error(`Elemento no encontrado: ${selector}`);
    }
    return;
}

function verificarIntento() {
    let numeroDeUsuario = parseInt(document.getElementById('valorUsuario').value);
    console.log(`Número de usuario: ${numeroDeUsuario}`);

    if (numeroDeUsuario === numeroSecreto) {
        asignarTextoElemento('.texto__parrafo', `Acertaste el número en ${intentos} ${intentos === 1 ? `intento` : `intentos`}`);
        document.getElementById('reiniciar').removeAttribute('disabled');
    } else {
        if (numeroDeUsuario < numeroSecreto) {
            asignarTextoElemento('.texto__parrafo', 'El número secreto es mayor');
        } else {
            asignarTextoElemento('.texto__parrafo', 'El número secreto es menor');
        }
        intentos++;
        limpiarCaja();          
    }
    return;
}

function limpiarCaja() {
   document.querySelector('#valorUsuario').value = "";
}

function generarNumeroSecreto() {
    let numeroGenerado =  Math.floor(Math.random() * numeroMaximo) + 1;
    console.log(`Número generado: ${numeroGenerado}`);
    console.log(`Lista de números sorteados: ${listaNumerosSorteados}`);

    if (listaNumerosSorteados.length == numeroMaximo) {
        asignarTextoElemento('.texto__parrafo', 'Ya se sortearon todos los números posibles');
    } else {
        if (listaNumerosSorteados.includes(numeroGenerado)) {
            return generarNumeroSecreto();
        } else {
            listaNumerosSorteados.push(numeroGenerado);
            return numeroGenerado;
        }
    }
}

function condicionesIniciales() {
    asignarTextoElemento('h1', "Juego del número secreto");
    asignarTextoElemento('.texto__parrafo', `Indica un número del 1 al ${numeroMaximo}`);
    numeroSecreto = generarNumeroSecreto();
    intentos = 1;
    console.log(`Número secreto generado: ${numeroSecreto}`);
}

function reiniciarJuego() {
    limpiarCaja();
    condicionesIniciales();
    document.querySelector('#reiniciar').setAttribute('disabled', 'true');
}

condicionesIniciales();
