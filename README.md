<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Time Zone Viewer</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        select {
            margin: 10px;
        }
        .time-display {
            margin-top: 20px;
        }
        .location-link {
            display: inline-block;
            margin-top: 20px;
            font-size: 14px;
            color: blue;
            cursor: pointer;
        }
    </style>
</head>
<body>

<h1>Check the Time in Different Locations</h1>

<label for="location">Select a city:</label>
<select id="location">
    <option value="New York">New York (UTC-5)</option>
    <option value="London">London (UTC+0)</option>
    <option value="Tokyo">Tokyo (UTC+9)</option>
</select>

<div class="time-display" id="timeDisplay"></div>

<p id="currentLocationTime"></p>

<a href="/" class="location-link" id="homeLink" style="display:none;">Go to Homepage</a>

<script>
    // Time zones
    const timeZones = {
        "New York": "America/New_York",
        "London": "Europe/London",
        "Tokyo": "Asia/Tokyo"
    };

    // Function to display time in selected location
    function displayTime(location) {
        const options = {
            timeZone: timeZones[location],
            hour12: true,
            hour: "2-digit",
            minute: "2-digit",
            second: "2-digit",
            year: "numeric",
            month: "short",
            day: "2-digit"
        };
        
        const formatter = new Intl.DateTimeFormat("en-US", options);
        const time = formatter.format(new Date());
        document.getElementById("timeDisplay").innerHTML = `Current time in ${location}: ${time}`;
    }

    // Function to get the user's current location time
    function displayUserTime() {
        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition((position) => {
                const userLat = position.coords.latitude;
                const userLon = position.coords.longitude;
                
                const options = {
                    timeZoneName: "short",
                    timeZone: Intl.DateTimeFormat().resolvedOptions().timeZone,
                    hour12: true,
                    hour: "2-digit",
                    minute: "2-digit",
                    second: "2-digit",
                    year: "numeric",
                    month: "short",
                    day: "2-digit"
                };
                
                const userTimeFormatter = new Intl.DateTimeFormat("en-US", options);
                const userTime = userTimeFormatter.format(new Date());
                document.getElementById("currentLocationTime").innerHTML = `Your current time: ${userTime}`;
            });
        } else {
            alert("Geolocation is not supported by this browser.");
        }
    }

    // Event listener for selecting a location from the dropdown
    document.getElementById("location").addEventListener("change", (event) => {
        displayTime(event.target.value);
        document.getElementById("homeLink").style.display = "inline";
    });

    // Initially display time for New York
    displayTime("New York");

    // Display the user's local time when the page loads
    displayUserTime();
</script>

</body>
</html>
