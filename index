<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Value Chain Puzzle Game</title>
    <style>
        :root {
            --rd: #a6c9ec;
            --design: #b0dfb4;
            --prod: #e04b4b;
            --mkt: #fce8a6;
            --dist: #bce0fd;
            --serv: #f7d5b4;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f7f6;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        h1 {
            color: #2c3e50;
            margin-bottom: 5px;
        }

        p.desc {
            color: #7f8c8d;
            margin-bottom: 20px;
        }

        #timer {
            font-size: 24px;
            font-weight: bold;
            color: #e74c3c;
            background: #fff;
            padding: 10px 25px;
            border-radius: 30px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
            margin-bottom: 30px;
        }

        /* Slots (Drop Zone) */
        .chain-container {
            display: flex;
            gap: 5px;
            background: #e0e0e0;
            padding: 15px;
            border-radius: 12px;
            box-shadow: inset 0 2px 6px rgba(0,0,0,0.15);
            margin-bottom: 40px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .slot {
            width: 140px;
            height: 80px;
            border: 2px dashed #b0b0b0;
            border-radius: 6px;
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: #f9f9f9;
            position: relative;
        }

        .slot::after {
            content: attr(data-step);
            position: absolute;
            font-size: 12px;
            color: #aaa;
            bottom: 5px;
        }

        /* Blocks (Draggable Items) */
        .blocks-pool {
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
            justify-content: center;
            max-width: 900px;
        }

        .block {
            width: 140px;
            height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            font-weight: bold;
            font-size: 13px;
            color: #2c3e50;
            cursor: grab;
            user-select: none;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            transition: transform 0.2s, box-shadow 0.2s;
            clip-path: polygon(0% 0%, 85% 0%, 100% 50%, 85% 100%, 0% 100%, 15% 50%);
            padding: 0 15px;
            box-sizing: border-box;
        }

        .block:active {
            cursor: grabbing;
            transform: scale(1.05);
        }

        /* Block Colors matching the diagram */
        .block[data-id="1"] { background-color: var(--rd); }
        .block[data-id="2"] { background-color: var(--design); }
        .block[data-id="3"] { background-color: var(--prod); color: white; }
        .block[data-id="4"] { background-color: var(--mkt); }
        .block[data-id="5"] { background-color: var(--dist); }
        .block[data-id="6"] { background-color: var(--serv); }

        .btn {
            background-color: #27ae60;
            color: white;
            border: none;
            padding: 12px 30px;
            font-size: 16px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            margin-top: 20px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }

        .btn:hover {
            background-color: #219150;
        }

        #message {
            margin-top: 15px;
            font-size: 18px;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <h1>Assemble the Value Chain</h1>
    <p class="desc">Drag and drop the blocks into the correct sequence before time runs out!</p>

    <div id="timer">Time Remaining: <span id="time">60</span>s</div>

    <!-- Drop Zones -->
    <div class="chain-container">
        <div class="slot" data-order="1" data-step="Step 1"></div>
        <div class="slot" data-order="2" data-step="Step 2"></div>
        <div class="slot" data-order="3" data-step="Step 3"></div>
        <div class="slot" data-order="4" data-step="Step 4"></div>
        <div class="slot" data-order="5" data-step="Step 5"></div>
        <div class="slot" data-order="6" data-step="Step 6"></div>
    </div>

    <!-- Draggable Blocks -->
    <div class="blocks-pool" id="pool">
        <div class="block" draggable="true" data-id="3">Production</div>
        <div class="block" draggable="true" data-id="1">Research and Development</div>
        <div class="block" draggable="true" data-id="6">Customer Service</div>
        <div class="block" draggable="true" data-id="2">Design of Products and Processes</div>
        <div class="block" draggable="true" data-id="5">Distribution</div>
        <div class="block" draggable="true" data-id="4">Marketing</div>
    </div>

    <button class="btn" onclick="checkWin()">Submit Sequence</button>
    <div id="message"></div>

    <script>
        let timeLeft = 60;
        let timerId;
        const timerElement = document.getElementById('time');
        const messageElement = document.getElementById('message');
        let gameActive = true;

        // Timer Functionality
        function startTimer() {
            timerId = setInterval(() => {
                timeLeft--;
                timerElement.textContent = timeLeft;
                if (timeLeft <= 0) {
                    clearInterval(timerId);
                    gameActive = false;
                    messageElement.textContent = "Time's up! Try again.";
                    messageElement.style.color = "#e74c3c";
                    disableDrag();
                }
            }, 1000);
        }

        // Drag and Drop Logic
        const blocks = document.querySelectorAll('.block');
        const slots = document.querySelectorAll('.slot');
        const pool = document.getElementById('pool');

        blocks.forEach(block => {
            block.addEventListener('dragstart', dragStart);
        });

        slots.forEach(slot => {
            slot.addEventListener('dragover', dragOver);
            slot.addEventListener('drop', dropBlock);
        });

        pool.addEventListener('dragover', dragOver);
        pool.addEventListener('drop', (e) => {
            e.preventDefault();
            const id = e.dataTransfer.getData('text/plain');
            const draggedBlock = document.querySelector(`.block[data-id='${id}']`);
            pool.appendChild(draggedBlock);
        });

        function dragStart(e) {
            if (!gameActive) return;
            e.dataTransfer.setData('text/plain', e.target.dataset.id);
        }

        function dragOver(e) {
            e.preventDefault();
        }

        function dropBlock(e) {
            e.preventDefault();
            if (!gameActive) return;
            
            const id = e.dataTransfer.getData('text/plain');
            const draggedBlock = document.querySelector(`.block[data-id='${id}']`);
            
            if (this.children.length === 0) {
                this.appendChild(draggedBlock);
            }
        }

        function disableDrag() {
            blocks.forEach(block => block.setAttribute('draggable', 'false'));
        }

        // Validation
        function checkWin() {
            if (!gameActive) return;

            let correctCount = 0;
            slots.forEach(slot => {
                const child = slot.firstElementChild;
                if (child && child.dataset.id === slot.dataset.order) {
                    correctCount++;
                }
            });

            if (correctCount === 6) {
                clearInterval(timerId);
                messageElement.textContent = "Great job! You successfully built the Value Chain!";
                messageElement.style.color = "#27ae60";
                gameActive = false;
            } else {
                messageElement.textContent = "Incorrect order. Check the sequence and try again!";
                messageElement.style.color = "#e67e22";
            }
        }

        startTimer();
    </script>
</body>
</html>
