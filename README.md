<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tiger Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="game-container">
        <div class="tiger"></div>
        <div class="hunter"></div>
    </div>
    <script src="script.js"></script>
</body>
</html>
body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f7f7f7;
    margin: 0;
    overflow: hidden;
}

.game-container {
    position: relative;
    width: 800px;
    height: 200px;
    border: 1px solid #000;
    background-color: #fff;
    overflow: hidden;
}

.tiger {
    position: absolute;
    bottom: 0;
    left: 50px;
    width: 50px;
    height: 50px;
    background-color: orange;
    background-image: url('tiger.png'); /* Add a tiger image */
    background-size: cover;
    transition: 0.1s;
}

.tiger.jump {
    animation: jump 0.3s linear;
    animation-fill-mode: forwards;
}

.tiger.bend {
    height: 25px;
}

@keyframes jump {
    0% { bottom: 0; }
    50% { bottom: 100px; } /* Adjust jump height */
    100% { bottom: 0; }
}

.hunter {
    position: absolute;
    bottom: 0;
    right: 0;
    width: 20px;
    height: 50px;
    background-color: red;
    background-image: url('hunter.png'); /* Add a hunter image */
    background-size: cover;
    animation: moveHunter 2s linear infinite;
}

@keyframes moveHunter {
    0% {
        right: 0;
    }
    100% {
        right: 100%;
    }
}
document.addEventListener('keydown', function(event) {
    if (event.code === 'ArrowUp') {
        jump();
    } else if (event.code === 'ArrowDown') {
        bendDown();
    }
});

document.addEventListener('keyup', function(event) {
    if (event.code === 'ArrowDown') {
        standUp();
    }
});

function jump() {
    const tiger = document.querySelector('.tiger');
    if (!tiger.classList.contains('jump')) {
        tiger.classList.add('jump');
        setTimeout(function() {
            tiger.classList.remove('jump');
        }, 300);
    }
}

function bendDown() {
    const tiger = document.querySelector('.tiger');
    if (!tiger.classList.contains('bend')) {
        tiger.classList.add('bend');
    }
}

function standUp() {
    const tiger = document.querySelector('.tiger');
    tiger.classList.remove('bend');
}

let isAlive = setInterval(function() {
    const tiger = document.querySelector('.tiger');
    const hunter = document.querySelector('.hunter');

    const tigerBottom = parseInt(window.getComputedStyle(tiger).getPropertyValue('bottom'));
    const hunterLeft = parseInt(window.getComputedStyle(hunter).getPropertyValue('left'));

    if (hunterLeft < 100 && hunterLeft > 50 && tigerBottom < 50 && !tiger.classList.contains('bend')) {
        alert('Game Over');
    }
}, 10);
