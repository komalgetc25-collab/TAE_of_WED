<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Pet Care Scheduler</title>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #ffe8d6, #e0f7fa);
            min-height: 100vh;
            padding: 20px;
        }

        header {
            text-align: center;
            background: white;
            padding: 25px;
            border-radius: 20px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            margin-bottom: 25px;
        }

        header h1 {
            color: #ff7043;
            margin-bottom: 8px;
        }

        header p {
            color: #555;
        }

        .container {
            max-width: 1000px;
            margin: auto;
        }

        .card {
            background: white;
            padding: 22px;
            border-radius: 18px;
            margin-bottom: 20px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.08);
        }

        .card h2 {
            color: #333;
            margin-bottom: 15px;
        }

        .pet-info {
            display: flex;
            gap: 20px;
            align-items: center;
            flex-wrap: wrap;
        }

        .pet-icon {
            font-size: 70px;
        }

        .pet-info input {
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 10px;
            margin: 5px;
        }

        .form {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
        }

        input, select {
            width: 100%;
            padding: 12px;
            border: 1px solid #ccc;
            border-radius: 10px;
            font-size: 15px;
        }

        button {
            border: none;
            padding: 12px 18px;
            border-radius: 10px;
            cursor: pointer;
            font-size: 15px;
            font-weight: bold;
        }

        .add-btn {
            background: #ff7043;
            color: white;
            grid-column: span 2;
        }

        .add-btn:hover {
            background: #f4511e;
        }

        .task {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px;
            margin-top: 12px;
            background: #f8f9fa;
            border-left: 5px solid #ff7043;
            border-radius: 12px;
            gap: 10px;
        }

        .task-info h3 {
            color: #333;
            margin-bottom: 5px;
        }

        .task-info p {
            color: #666;
            font-size: 14px;
        }

        .task.completed {
            opacity: 0.55;
            text-decoration: line-through;
        }

        .complete-btn {
            background: #4caf50;
            color: white;
        }

        .delete-btn {
            background: #ef5350;
            color: white;
        }

        .buttons {
            display: flex;
            gap: 8px;
        }

        .stats {
            display: flex;
            justify-content: space-around;
            text-align: center;
            gap: 10px;
        }

        .stat {
            background: #fff3e0;
            padding: 18px;
            border-radius: 15px;
            flex: 1;
        }

        .stat h3 {
            color: #ff7043;
            font-size: 25px;
        }

        footer {
            text-align: center;
            margin-top: 20px;
            color: #666;
        }

        @media (max-width: 600px) {
            .form {
                grid-template-columns: 1fr;
            }

            .add-btn {
                grid-column: span 1;
            }

            .task {
                flex-direction: column;
                align-items: flex-start;
            }

            .buttons {
                width: 100%;
            }

            .buttons button {
                flex: 1;
            }

            .stats {
                flex-direction: column;
            }
        }
    </style>
</head>

<body>

<div class="container">

    <header>
        <h1>🐾 Pet Care Scheduler</h1>
        <p>Take care of your lovely pet on time!</p>
    </header>

    <!-- Pet Profile -->
    <div class="card">
        <h2>🐶 Pet Profile</h2>

        <div class="pet-info">
            <div class="pet-icon">🐕</div>

            <div>
                <input type="text" id="petName" placeholder="Pet Name">
                <input type="text" id="petType" placeholder="Pet Type">
                <button class="add-btn" onclick="savePet()">
                    Save Pet
                </button>
            </div>
        </div>

        <p id="petMessage"></p>
    </div>

    <!-- Statistics -->
    <div class="card">
        <h2>📊 Today's Overview</h2>

        <div class="stats">
            <div class="stat">
                <h3 id="totalTasks">0</h3>
                <p>Total Tasks</p>
            </div>

            <div class="stat">
                <h3 id="completedTasks">0</h3>
                <p>Completed</p>
            </div>

            <div class="stat">
                <h3 id="pendingTasks">0</h3>
                <p>Pending</p>
            </div>
        </div>
    </div>

    <!-- Add Task -->
    <div class="card">

        <h2>➕ Add Care Task</h2>

        <div class="form">

            <input
                type="text"
                id="taskName"
                placeholder="Task name e.g. Give medicine"
            >

            <select id="taskType">
                <option value="🍖 Feeding">🍖 Feeding</option>
                <option value="💊 Medicine">💊 Medicine</option>
                <option value="🛁 Grooming">🛁 Grooming</option>
                <option value="🏃 Exercise">🏃 Exercise</option>
                <option value="🩺 Vet Checkup">🩺 Vet Checkup</option>
                <option value="💧 Water">💧 Water</option>
            </select>

            <input type="date" id="taskDate">

            <input type="time" id="taskTime">

            <input
                type="text"
                id="taskNote"
                placeholder="Extra note (optional)"
            >

            <button class="add-btn" onclick="addTask()">
                Add Task
            </button>

        </div>
    </div>

    <!-- Task List -->
    <div class="card">

        <h2>📅 My Pet's Schedule</h2>

        <div id="taskList">
            <p>No tasks added yet.</p>
        </div>

    </div>

    <footer>
        🐾 Made with ❤️ for happy pets
    </footer>

</div>


<script>

    // Load tasks from browser storage
    let tasks = JSON.parse(localStorage.getItem("petTasks")) || [];

    // Save pet information
    function savePet() {

        let name = document.getElementById("petName").value;
        let type = document.getElementById("petType").value;

        if (name === "" || type === "") {
            alert("Please enter pet name and type.");
            return;
        }

        localStorage.setItem("petName", name);
        localStorage.setItem("petType", type);

        document.getElementById("petMessage").innerHTML =
            "🐾 " + name + " (" + type + ") profile saved!";
    }


    // Add new task
    function addTask() {

        let name = document.getElementById("taskName").value;
        let type = document.getElementById("taskType").value;
        let date = document.getElementById("taskDate").value;
        let time = document.getElementById("taskTime").value;
        let note = document.getElementById("taskNote").value;

        if (name === "" || date === "" || time === "") {
            alert("Please fill task name, date and time.");
            return;
        }

        let newTask = {
            id: Date.now(),
            name: name,
            type: type,
            date: date,
            time: time,
            note: note,
            completed: false
        };

        tasks.push(newTask);

        localStorage.setItem(
            "petTasks",
            JSON.stringify(tasks)
        );

        // Clear form
        document.getElementById("taskName").value = "";
        document.getElementById("taskDate").value = "";
        document.getElementById("taskTime").value = "";
        document.getElementById("taskNote").value = "";

        displayTasks();
    }


    // Display all tasks
    function displayTasks() {

        let taskList = document.getElementById("taskList");

        if (tasks.length === 0) {
            taskList.innerHTML =
                "<p>No tasks added yet.</p>";
            updateStats();
            return;
        }

        // Sort tasks by date and time
        tasks.sort(function(a, b) {
            return (a.date + a.time)
                .localeCompare(b.date + b.time);
        });

        taskList.innerHTML = "";

        tasks.forEach(function(task) {

            let div = document.createElement("div");

            div.className =
                "task " + (task.completed ? "completed" : "");

            div.innerHTML = `

                <div class="task-info">

                    <h3>
                        ${task.type} ${task.name}
                    </h3>

                    <p>
                        📅 ${task.date}
                        &nbsp;&nbsp;
                        ⏰ ${task.time}
                    </p>

                    <p>
                        ${task.note || "No extra note"}
                    </p>

                </div>

                <div class="buttons">

                    <button
                        class="complete-btn"
                        onclick="completeTask(${task.id})">
                        ${task.completed ? "Undo" : "Done"}
                    </button>

                    <button
                        class="delete-btn"
                        onclick="deleteTask(${task.id})">
                        Delete
                    </button>

                </div>
            `;

            taskList.appendChild(div);
        });

        updateStats();
    }


    // Complete / Undo task
    function completeTask(id) {

        tasks = tasks.map(function(task) {

            if (task.id === id) {
                task.completed = !task.completed;
            }

            return task;
        });

        localStorage.setItem(
            "petTasks",
            JSON.stringify(tasks)
        );

        displayTasks();
    }


    // Delete task
    function deleteTask(id) {

        if (confirm("Delete this task?")) {

            tasks = tasks.filter(function(task) {
                return task.id !== id;
            });

            localStorage.setItem(
                "petTasks",
                JSON.stringify(tasks)
            );

            displayTasks();
        }
    }


    // Update statistics
    function updateStats() {

        let total = tasks.length;

        let completed = tasks.filter(function(task) {
            return task.completed;
        }).length;

        let pending = total - completed;

        document.getElementById("totalTasks").innerText = total;
        document.getElementById("completedTasks").innerText = completed;
        document.getElementById("pendingTasks").innerText = pending;
    }


    // Load saved pet information
    window.onload = function() {

        let savedName = localStorage.getItem("petName");
        let savedType = localStorage.getItem("petType");

        if (savedName) {
