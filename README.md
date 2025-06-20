# todo.my

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Todo App with Notifications</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.3/dist/tailwind.min.css" rel="stylesheet">
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            background-color: #1a202c;
            color: #e2e8f0;
        }

        .container {
            max-width: 600px;
            margin: 0 auto;
            padding: 20px;
        }
    </style>
</head>

<body>
    <div class="container">
        <h1 class="text-3xl font-bold mb-6 text-center">Todo App with Notifications</h1>
        <form id="todoForm" class="bg-gray-800 p-6 rounded-lg shadow-lg">
            <div class="mb-4">
                <label for="taskDescription" class="block text-sm font-medium">Task Description</label>
                <input type="text" id="taskDescription" name="taskDescription" class="w-full p-3 mt-2 bg-gray-900 rounded text-gray-100">
            </div>
            <div class="mb-4">
                <label for="dueDate" class="block text-sm font-medium">Due Date</label>
                <input type="date" id="dueDate" name="dueDate" class="w-full p-3 mt-2 bg-gray-900 rounded text-gray-100">
            </div>
            <div class="mb-4">
                <label for="reminderTime" class="block text-sm font-medium">Reminder Time</label>
                <input type="time" id="reminderTime" name="reminderTime" class="w-full p-3 mt-2 bg-gray-900 rounded text-gray-100">
            </div>
            <button type="submit" class="w-full mt-4 bg-pink-600 hover:bg- p-3 rounded text-white font-bold">Done it 🚀</button>
        </form>
        <div id="output" class="mt-6 bg-gray-700 p-4 rounded-lg shadow-lg text-gray-100"></div>
    </div>

    <footer class="text-center mt-8">
        <a href="#" class="text-pink-500 hover:text-pink-400">Made by Rayyan</a>
    </footer>

    <script>
        document.getElementById("todoForm").addEventListener("submit", function (event) {
            event.preventDefault();
            const taskDescription = document.getElementById("taskDescription").value;
            const dueDate = document.getElementById("dueDate").value;
            const reminderTime = document.getElementById("reminderTime").value;
            const substitutedPrompt = `Please generate a reminder for me to complete the task "${taskDescription}" by "${dueDate}" at "${reminderTime}". Once the task is completed, mark it as done and transfer it to the completed tab.`;

            const output = document.getElementById("output");
            output.innerText = ''; // Clear the output section first

            const websocket = new WebSocket("wss://backend.buildpicoapps.com/ask_ai_streaming_v2");

            websocket.addEventListener("open", () => {
                websocket.send(JSON.stringify({
                    appId: "relationship-go",
                    prompt: substitutedPrompt,
                }));
            });

            websocket.addEventListener("message", (event) => {
                console.log(event.data);
                output.innerText = `${output.innerText}${event.data}`;
            });

            websocket.addEventListener("close", (event) => {
                console.log("Connection closed", event.code, event.reason);
                if (event.code != 1000) {
                    alert("Oops, we ran into an error. Refresh the page and try again.");
                }
            });

            websocket.addEventListener("error", (error) => {
                console.log('WebSocket error', error);
                alert("Oops, we ran into an error. Refresh the page and try again.");
            });
        });
    </script>
</body>

</html>
```
