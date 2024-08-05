# FetchDataNoReloadJSVanilla
Fetch Data and real time update no refresh page PHP and JS Vanilla

If you want to display data from a MySQL database using PHP and JavaScript without reloading the page, you'll need to set up a PHP script that fetches the data from MySQL and then use JavaScript to make an asynchronous request to that PHP script. Here's a step-by-step guide to achieve this:

1. Server-Side PHP Script
Create a PHP script that connects to your MySQL database and returns data in JSON format.

Example PHP Script (fetch_data.php):

```
<?php
header('Content-Type: application/json');

// Database credentials
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "your_database_name";

// Create a new connection to the MySQL database
$conn = new mysqli($servername, $username, $password, $dbname);

// Check the connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

// Query to select data from a table
$sql = "SELECT id, name FROM your_table_name";
$result = $conn->query($sql);

// Create an array to hold the data
$data = array();

// Fetch data from the result set
if ($result->num_rows > 0) {
    while($row = $result->fetch_assoc()) {
        $data[] = $row;
    }
}

// Close the connection
$conn->close();

// Output data as JSON
echo json_encode($data);
?>


```

2. Client-Side HTML and JavaScript
Create an HTML file that includes JavaScript to fetch data from the PHP script and update the HTML.

Example HTML File (index.html):

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fetch Data Example</title>
    <style>
        /* Simple styling for better visibility */
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
        }
        ul {
            list-style-type: none;
            padding: 0;
        }
        li {
            margin: 5px 0;
        }
    </style>
</head>
<body>
    <h1>Data from MySQL Database</h1>
    <ul id="data-list"></ul>

    <script>
        // Function to fetch data from the PHP script
        function fetchData() {
            fetch('fetch_data.php')
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Network response was not ok');
                    }
                    return response.json();
                })
                .then(data => {
                    updateUI(data);
                })
                .catch(error => {
                    console.error('There was a problem with the fetch operation:', error);
                });
        }

        // Function to update the HTML with fetched data
        function updateUI(data) {
            const dataList = document.getElementById('data-list');
            dataList.innerHTML = ''; // Clear existing content

            data.forEach(item => {
                const listItem = document.createElement('li');
                listItem.textContent = item.name;
                dataList.appendChild(listItem);
            });
        }

        // Fetch data when the page loads
        document.addEventListener('DOMContentLoaded', fetchData);
    </script>
</body>
</html>

```
