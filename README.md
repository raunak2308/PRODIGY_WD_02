HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stopwatch</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="stopwatch">
        <div id="display">00:00:00</div>
        <div class="controls">
            <button id="startStop">Start</button>
            <button id="reset">Reset</button>
            <button id="lap">Lap</button>
        </div>
        <ul id="laps"></ul>
    </div>
    <script src="script.js"></script>
</body>
</html>

CSS
body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
}

.stopwatch {
    text-align: center;
    background-color: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

#display {
    font-size: 3em;
    margin-bottom: 20px;
}

.controls button {
    font-size: 1em;
    padding: 10px 20px;
    margin: 5px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

#laps {
    list-style-type: none;
    padding: 0;
    margin-top: 20px;
}

#laps li {
    background-color: #e0e0e0;
    margin: 5px 0;
    padding: 10px;
    border-radius: 5px;
}

JS
let startTime, updatedTime, difference, tInterval, savedTime, running = false;
const display = document.getElementById("display");
const startStopButton = document.getElementById("startStop");
const resetButton = document.getElementById("reset");
const lapButton = document.getElementById("lap");
const lapsContainer = document.getElementById("laps");

function startTimer() {
    if (!running) {
        startTime = new Date().getTime() - (savedTime || 0);
        tInterval = setInterval(getShowTime, 1);
        startStopButton.innerHTML = "Pause";
        running = true;
    } else {
        clearInterval(tInterval);
        savedTime = new Date().getTime() - startTime;
        startStopButton.innerHTML = "Start";
        running = false;
    }
}

function resetTimer() {
    clearInterval(tInterval);
    savedTime = 0;
    difference = 0;
    running = false;
    startStopButton.innerHTML = "Start";
    display.innerHTML = "00:00:00";
    lapsContainer.innerHTML = "";
}

function lapTimer() {
    if (running) {
        const lapTime = display.innerHTML;
        const lapElement = document.createElement("li");
        lapElement.textContent = lapTime;
        lapsContainer.appendChild(lapElement);
    }
}

function getShowTime() {
    updatedTime = new Date().getTime();
    difference = updatedTime - startTime;

    const hours = Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    const minutes = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
    const seconds = Math.floor((difference % (1000 * 60)) / 1000);

    display.innerHTML =
        (hours < 10 ? "0" + hours : hours) + ":" +
        (minutes < 10 ? "0" + minutes : minutes) + ":" +
        (seconds < 10 ? "0" + seconds : seconds);
}

startStopButton.addEventListener("click", startTimer);
resetButton.addEventListener("click", resetTimer);
lapButton.addEventListener("click", lapTimer);
