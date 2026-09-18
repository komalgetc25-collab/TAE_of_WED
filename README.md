<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Pet Care Scheduler</title>
  <style>
    * { box-sizing: border-box; }

    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #fff4df;
      text-align: center;
      padding: 20px;
    }

    .box {
      max-width: 650px;
      margin: 30px auto;
      padding: 25px 20px;
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    }

    h1 {
      margin-top: 0;
      font-size: 2rem;
    }

    p {
      color: #555;
      margin-bottom: 20px;
    }

    .images {
      display: flex;
      justify-content: center;
      gap: 12px;
      flex-wrap: wrap;
      margin-bottom: 20px;
    }

    img {
      width: 150px;
      height: 100px;
      object-fit: cover;
      border-radius: 8px;
      border: 2px solid #ddd;
    }

    .form-row {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 10px;
      flex-wrap: wrap;
      margin: 15px 0 20px;
    }

    input, select, button {
      padding: 10px 12px;
      font-size: 1rem;
      border-radius: 6px;
      border: 1px solid #ccc;
    }

    input {
      width: 260px;
    }

    select {
      width: 170px;
    }

    button {
      color: white;
      background: #347653;
      border: none;
      cursor: pointer;
      transition: 0.2s ease;
    }

    button:hover {
      background: #285f43;
    }

    ul {
      list-style: none;
      padding: 0;
      margin: 0 auto;
      max-width: 480px;
      text-align: left;
    }

    li {
      margin: 12px 0;
      padding: 12px 14px;
      background: #f9f9f9;
      border-radius: 8px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 10px;
      border: 1px solid #eee;
    }

    .task-text {
      flex: 1;
      word-break: break-word;
    }

    .task-day {
      color: #666;
      font-size: 0.9rem;
      margin-left: 8px;
    }

    .delete-btn {
      background: #d9534f;
      padding: 8px 12px;
      font-size: 0.9rem;
    }

    .delete-btn:hover {
      background: #b63c36;
    }
  </style>
</head>
<body>
  <div class="box">
    <h1>🐾 Pet Care Scheduler</h1>
    <p>Plan your pet's daily care.</p>

    <div class="images">
      <img src="https://images.unsplash.com/photo-1552053831-71594a27632d?auto=format&fit=crop&w=300&q=80" alt="Dog">
      <img src="https://images.unsplash.com/photo-1519052537078-e6302a4968d4?auto=format&fit=crop&w=300&q=80" alt="Cat">
    </div>

    <h2>Add a Task</h2>
    <div class="form-row">
      <input id="task" type="text" placeholder="Example: Feed the dog" />
      <select id="day">
        <option>Monday</option>
        <option>Tuesday</option>
        <option>Wednesday</option>
        <option>Thursday</option>
        <option>Friday</option>
        <option>Saturday</option>
        <option>Sunday</option>
      </select>
      <button onclick="addTask()">Add</button>
    </div>

    <h2>My Tasks</h2>
    <ul id="list"></ul>
  </div>

  <script>
    const taskInput = document.getElementById("task");
    const daySelect = document.getElementById("day");
    const list = document.getElementById("list");

    const STORAGE_KEY = "petCareTasks";

    function loadTasks() {
      const tasks = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
      tasks.forEach(task => renderTask(task.text, task.day));
    }

    function saveTasks() {
      const tasks = [];
      list.querySelectorAll("li").forEach(li => {
        const text = li.dataset.text;
        const day = li.dataset.day;
        tasks.push({ text, day });
      });
      localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks));
    }

    function renderTask(taskText, taskDay) {
      const item = document.createElement("li");
      item.dataset.text = taskText;
      item.dataset.day = taskDay;

      const taskInfo = document.createElement("div");
      taskInfo.className = "task-text";
      taskInfo.innerHTML = `<span>${taskText}</span> <span class="task-day">- ${taskDay}</span>`;

      const deleteBtn = document.createElement("button");
      deleteBtn.textContent = "Delete";
      deleteBtn.className = "delete-btn";
      deleteBtn.onclick = function () {
        item.remove();
        saveTasks();
      };

      item.appendChild(taskInfo);
      item.appendChild(deleteBtn);
      list.appendChild(item);
    }

    function addTask() {
      const taskText = taskInput.value.trim();
      const taskDay = daySelect.value;

      if (taskText === "") {
        alert("Please enter a task.");
        return;
      }

      renderTask(taskText, taskDay);
      saveTasks();

      taskInput.value = "";
      taskInput.focus();
    }

    taskInput.addEventListener("keydown", function (event) {
      if (event.key === "Enter") {
        addTask();
      }
    });

    loadTasks();
  </script>
</body>
</html>
