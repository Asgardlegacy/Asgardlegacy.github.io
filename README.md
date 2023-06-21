<html>
<head>
    <title>Number Manager</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            touch-action: none; /* Prevent double click to zoom */
        }
        .options-grid {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
        }
        .option,
        .button-group button {
            flex: 1 1 48%;
            box-sizing: border-box;
            border: 1px solid #ccc;
            margin: 2px;
            padding: 10px;
            text-align: center;
            cursor: pointer;
        }
        .option:nth-last-child(1):nth-child(odd),
        .option:nth-last-child(2):nth-child(odd) {
            flex: 0 0 100%;
        }
        .option input[type="checkbox"] {
            display: none;
        }
        .option.checked {
            background-color: #ddd;
        }
        .button-group {
        position: fixed;
        bottom: 0;
        width: 100%;
        display: flex;
        justify-content: space-between;
        align-items: stretch;
        background-color: #fff;
        z-index: 1;
        touch-action: none;
    }
        .button-group button {
        flex-grow: 1;
        flex-basis: 0;
        border: none;
        height: 75px;
        font-size: 18px;
        touch-action: manipulation;
    }
        .button-group button:first-child {
            background-color: lightblue;
            border-right: 1px solid black;
        }
        .button-group button:last-child {
            background-color: lightgreen;
        }
        .focused {
            background-color: #f0f0f0;
        }
    </style>
</head>
<body>
    <button onclick="downloadJSON()">Download JSON</button>
    <button onclick="downloadHTML()">Download HTML</button>
    <input type="file" id="loadState" style="display: none" accept="application/json" onchange="loadState(event)" />
    <button onclick="document.getElementById('loadState').click()" ontouchstart="event.stopPropagation()">Load State</button>

    <input type="text" pattern="\d*" id="number-search" oninput="searchNumber()" placeholder="Search number..." />
    <h1 id="number"></h1>
    <div id="options" class="options-grid"></div>

    <input type="file" id="file" style="display: none" accept="image/*" onchange="handleFileSelect(event)" />
    <img id="photo" style="display: none; max-width: 200px; max-height: 200px; margin-bottom: 10px" />
    <div class="button-group">
        <button id="generateReportButton" style="display: none;" onclick="generateReport()">Generate Report</button>
        <button onclick="previousNumber()">Previous</button>
        <button onclick="nextNumber()">Next</button>
    </div>
    <script>
        var doubleTouchStartTimestamp = 0;
        document.addEventListener("touchstart", function (event) {
            var now = Date.now();
            if (doubleTouchStartTimestamp + 500 > now) {
                event.preventDefault();
            }
            doubleTouchStartTimestamp = now;
        });

        var options = [
            "Vacant",
            "Locked",
            "Overlocked",
            "Red Lock Only",
            "No Lock",
            "Locked Open",
            "Needs Repair",
        ];
        var numbers = [
            ...Array(16).keys()].map(n => n + 101)
            .concat([...Array(14).keys()].map(n => n + 121))
            .concat([...Array(45).keys()].map(n => n + 201))
            .concat([...Array(69).keys()].map(n => n + 300))
            .concat([...Array(48).keys()].map(n => n + 400))
            .concat([...Array(36).keys()].map(n => n + 501));
        var records = {};
        var images = {};
        var currentIndex = 0;

        function setNumber() {
            var number = numbers[currentIndex];
            document.getElementById("number").innerText = `Number: ${number}`;
            document.getElementById("number-search").value = "";
            var record = records[number] || [];
            options.forEach((option) => {
                var checkbox = document.querySelector(`#options .option[data-option="${option}"] input`);
                checkbox.checked = record.includes(option);
                checkbox.parentNode.classList.toggle("checked", checkbox.checked);
            });
            document.getElementById("photo").src = images[number] || "";
            document.getElementById("photo").style.display = images[number] ? "block" : "none";
            document.getElementById("generateReportButton").style.display =
                currentIndex === numbers.length - 1 ? "block" : "none";
        }

        function toggleCheckbox(event) {
            var checkbox = event.target.tagName === "INPUT" ? event.target : event.target.querySelector("input");
            checkbox.checked = !checkbox.checked;
            checkbox.parentNode.classList.toggle("checked", checkbox.checked);
            var number = numbers[currentIndex];
            var option = checkbox.parentNode.dataset.option;
            records[number] = records[number] || [];
            if (checkbox.checked) {
                records[number].push(option);
            } else {
                var index = records[number].indexOf(option);
                records[number].splice(index, 1);
            }
        }

        function nextNumber() {
            if (currentIndex < numbers.length - 1) {
                currentIndex = (currentIndex + 1) % numbers.length;
                setNumber();
            } else {
                generateReport();
            }
        }

        function previousNumber() {
            currentIndex = (currentIndex - 1 + numbers.length) % numbers.length;
            setNumber();
        }

        function searchNumber() {
            var searchValue = document.getElementById("number-search").value;
            if (searchValue) {
                var searchIndex = numbers.indexOf(Number(searchValue));
                if (searchIndex !== -1) {
                    currentIndex = searchIndex;
                    setNumber();
                    document.activeElement.blur();
                }
            }
        }

        function handleFileSelect(event) {
            if (event.target.id === "file") {
                var file = event.target.files[0];
                var reader = new FileReader();
                reader.onloadend = function () {
                    images[numbers[currentIndex]] = reader.result;
                    document.getElementById("photo").src = reader.result;
                    document.getElementById("photo").style.display = "block";
                };
                reader.readAsDataURL(file);
            }
        }

        options.forEach((option) => {
            var div = document.createElement("div");
            div.className = "option";
            div.dataset.option = option;
            div.onclick = toggleCheckbox;
            div.innerHTML = `<input type="checkbox"> ${option}`;
            document.getElementById("options").appendChild(div);
        });

        document.getElementById("number").onclick = function () {
            document.getElementById("file").click();
        };

        setNumber();

        function generateReport() {
            var report =
                '<html><head><style>table, th, td {border: 1px solid black;}</style></head><body><table><tr><th>Number</th><th>Options</th><th>Image</th></tr>';
            for (var number in records) {
                var options = records[number].join(", ");
                var image = images[number] ? `<img src="${images[number]}" style="max-width: 100px; max-height: 100px"/>` : "";
                report += `<tr><td>${number}</td><td>${options}</td><td>${image}</td></tr>`;
            }
            report += "</table></body></html>";

            var reportBlob = new Blob([report], { type: "text/html" });
            var reportUrl = URL.createObjectURL(reportBlob);
            var a = document.createElement("a");
            a.href = reportUrl;
            a.download = "report.html";
            a.style.display = "none";
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(reportUrl);

            var state = JSON.stringify({ records: records, images: images, numbers: numbers, currentIndex: currentIndex });
            var stateBlob = new Blob([state], { type: "application/json" });
            var stateUrl = URL.createObjectURL(stateBlob);
            var b = document.createElement("a");
            b.href = stateUrl;
            b.download = "state.json";
            b.style.display = "none";
            document.body.appendChild(b);
            b.click();
            document.body.removeChild(b);
            URL.revokeObjectURL(stateUrl);
        }

        function loadState(event) {
            var file = event.target.files[0];
            var reader = new FileReader();
            reader.onloadend = function () {
                var state = JSON.parse(reader.result);
                records = state.records;
                images = state.images;
                numbers = state.numbers;
                currentIndex = state.currentIndex;

                // Empty the options div and recreate the options for each number
                var optionsDiv = document.getElementById("options");
                optionsDiv.innerHTML = "";
                options.forEach((option) => {
                    var div = document.createElement("div");
                    div.className = "option";
                    div.dataset.option = option;
                    div.onclick = toggleCheckbox;
                    div.innerHTML = `<input type="checkbox"> ${option}`;
                    optionsDiv.appendChild(div);
                });

                setNumber();
            };
            reader.readAsText(file);

            // Reset the value of the file input element
            event.target.value = "";
        }

        let drags = new Set(); //set of all active drags
        document.addEventListener("touchmove", function (event) {
            if (!event.isTrusted) return; //don't react to fake touches
            Array.from(event.changedTouches).forEach(function (touch) {
                drags.add(touch.identifier); //mark this touch as a drag
            });
        });
        document.addEventListener("touchend", function (event) {
            if (!event.isTrusted) return;
            let isDrag = false;
            Array.from(event.changedTouches).forEach(function (touch) {
                if (drags.has(touch.identifier)) {
                    isDrag = true;
                }
                drags.delete(touch.identifier); //touch ended, so delete it
            });
            if (!isDrag && document.activeElement == document.body) {
                //note that double-tap only happens when the body is active
                event.preventDefault(); //don't zoom
                event.stopPropagation(); //don't relay event
                event.target.focus(); //in case it's an input element
                event.target.click(); //in case it has a click handler
                event.target.dispatchEvent(new TouchEvent("touchend", event));
                //dispatch a copy of this event (for other touch handlers)
            }
        });

        document.addEventListener("touchmove", function (event) {
            if (event.scale !== 1) event.preventDefault(); //if a scale gesture, don't
        });

        document.addEventListener("touchstart", function (e) {
            e.preventDefault();
        });

        function downloadJSON() {
            var state = JSON.stringify({ records: records, images: images, numbers: numbers, currentIndex: currentIndex });
            var blob = new Blob([state], { type: "application/json" });
            var url = URL.createObjectURL(blob);

            var a = document.createElement("a");
            a.href = url;
            a.download = "data.json";
            a.style.display = "none";

            document.body.appendChild(a);

            var event = new MouseEvent("click");
            a.dispatchEvent(event);

            document.body.removeChild(a);
            URL.revokeObjectURL(url);
        }

        function downloadHTML() {
            var report =
                '<html><head><style>table, th, td {border: 1px solid black;}</style></head><body><table><tr><th>Number</th><th>Options</th><th>Image</th></tr>';
            for (var number in records) {
                var options = records[number].join(", ");
                var image = images[number] ? `<img src="${images[number]}" style="max-width: 100px; max-height: 100px"/>` : "";
                report += `<tr><td>${number}</td><td>${options}</td><td>${image}</td></tr>`;
            }
            report += "</table></body></html>";

            var blob = new Blob([report], { type: "text/html" });
            var url = URL.createObjectURL(blob);

            var a = document.createElement("a");
            a.href = url;
            a.download = "report.html";
            a.style.display = "none";

            document.body.appendChild(a);

            var event = new MouseEvent("click");
            a.dispatchEvent(event);

            document.body.removeChild(a);
            URL.revokeObjectURL(url);
        }

        document.getElementById("number-search").addEventListener("focus", function () {
            this.parentNode.classList.add("focused");
        });

        document.getElementById("number-search").addEventListener("blur", function () {
            this.parentNode.classList.remove("focused");
        });
    </script>
</body>
</html>
