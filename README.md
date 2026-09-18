<!DOCTYPE html>
<html>
<head>
	<title>Pet Care Scheduler</title>
	<style>
		body { font-family: Arial; background: #fff4df; text-align: center; }
		.box { max-width: 600px; margin: 30px auto; padding: 20px; background: white; border-radius: 10px; }
		img { width: 150px; height: 100px; object-fit: cover; border-radius: 8px; margin: 5px; }
		input, select, button { padding: 10px; margin: 5px; }
		button { color: white; background: #347653; border: 0; border-radius: 5px; cursor: pointer; }
		li { margin: 10px; text-align: left; }
	</style>
</head>
<body>
	<div class="box">
		<h1>🐾 Pet Care Scheduler</h1>
		<p>Plan your pet's daily care.</p>

		<img src="https://images.unsplash.com/photo-1552053831-71594a27632d?auto=format&fit=crop&w=300&q=80" alt="Dog">
		<img src="https://images.unsplash.com/photo-1519052537078-e6302a4968d4?auto=format&fit=crop&w=300&q=80" alt="Cat">

		<h2>Add a Task</h2>
		<input id="task" placeholder="Example: Feed the dog">
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

		<h2>My Tasks</h2>
		<ul id="list"></ul>
	</div>

	<script>
		function addTask() {
			var taskText = document.getElementById("task").value;
			var taskDay = document.getElementById("day").value;

			if (taskText == "") {
				alert("Please enter a task.");
				return;
			}

			var item = document.createElement("li");
			item.innerHTML = taskText + " - " + taskDay +
				' <button onclick="this.parentElement.remove()">Delete</button>';
			document.getElementById("list").appendChild(item);
			document.getElementById("task").value = "";
		}
	</script>
</body>
</html>
