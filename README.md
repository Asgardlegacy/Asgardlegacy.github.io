<!DOCTYPE html>
<html>
<head>
    <title>Number Tracker</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            margin: 0;
            padding: 0;
        }

        #container {
            width: 100%;
            max-width: 400px;
            padding: 10px;
            box-sizing: border-box;
            position: relative;
        }

        #number {
            font-size: 2em;
            margin-bottom: 20px;
        }

        #photo {
            display: none;
            width: 100%;
            height: auto;
            margin-bottom: 20px;
        }

        #file-selector, #search-box {
            margin-bottom: 20px;
        }

        #options {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 50px;
        }

        .option {
            display: flex;
            align-items: center;
            padding: 10px;
            border: 1px solid #ddd;
            background-color: #fff;
        }

        .option input {
            margin-right: 10px;
        }

        .bottom-buttons {
           
            
            bottom: 0;
            width: 100%;
            border-top: 1px solid black;
			display: grid;
            grid-template-columns: repeat(2, 1fr);
        }

        .control-btn {
            flex: 1;
            padding: 20px;
            font-size: 1em;
            border: none;
            color: white;
            text-align: center;
            background-color: #007BFF;
        }

        .control-btn:active {
            background-color: #0056b3;
        }

        .control-btn:disabled {
            background-color: #cccccc;
        }

        .control-btn:first-child {
            border-right: 1px solid black;
        }

        .full-width {
            grid-column: span 2;
        }
    </style>
</head>
<body>
    <div id="container">
        <h2 id="number"></h2>
        <img id="photo" />
        <input type="file" id="file-selector" accept="image/*" onchange="previewFile()" />
        <input type="number" id="search-box" placeholder="Go to number..." oninput="searchNumber()" />
        <div id="options"></div>
        <div class="bottom-buttons">
            <button class="control-btn" onclick="previousNumber()">Previous</button>
            <button class="control-btn" onclick="nextNumber()">Next</button>
        </div>
        
        <button id="report-btn" class="control-btn" style="display: none;" onclick="generateReport()">Generate Report</button>
    </div>

    <script>
        var numbers = [];
        for (let i = 101; i <= 116; i++) {
            numbers.push(i);
        }
        for (let i = 121; i <= 134; i++) {
            numbers.push(i);
        }
        for (let i = 201; i <= 245; i++) {
            numbers.push(i);
        }
        for (let i = 300; i <= 368; i++) {
            numbers.push(i);
        }
        for (let i = 400; i <= 447; i++) {
            numbers.push(i);
        }
        for (let i = 501; i <= 536; i++) {
            numbers.push(i);
        }

        var currentIndex = 0;
        var records = {};
        var images = {};

        var options = ["Vacant", "Locked", "Overlocked", "Red Lock Only", "No Lock", "Locked Open", "Needs Repair"];
        var optionsContainer = document.getElementById('options');
        options.forEach((option, index) => {
            var div = document.createElement('div');
            div.classList.add('option');
            if (index === options.length - 1 && options.length % 2 !== 0) {
                div.classList.add('full-width');
            }
            div.onclick = toggleCheckbox;
            div.innerHTML = `<input type="checkbox" id="${option.toLowerCase().replace(' ', '')}" value="${option}"><label for="${option.toLowerCase().replace(' ', '')}">${option}</label>`;
            optionsContainer.appendChild(div);
        });

        function setNumber() {
            document.getElementById('number').textContent = numbers[currentIndex];
            document.getElementById('skip-btn').style.display = currentIndex < numbers.length - 1 ? 'block' : 'none';
            document.getElementById('report-btn').style.display = currentIndex == numbers.length - 1 ? 'block' : 'none';
            if (images[numbers[currentIndex]]) {
                document.getElementById('photo').style.display = 'block';
                document.getElementById('photo').src = images[numbers[currentIndex]];
            } else {
                document.getElementById('photo').style.display = 'none';
                document.getElementById('photo').src = "";
            }
            loadOptions();
        }

        function loadOptions() {
            options.forEach((option) => {
                var checkbox = document.getElementById(option.toLowerCase().replace(' ', ''));
                checkbox.checked = records[numbers[currentIndex]] && records[numbers[currentIndex]].includes(option);
            });
        }

        function toggleCheckbox(event) {
            var checkbox;
    if (event.target.tagName === 'INPUT') {
        checkbox = event.target;
    } else {
        checkbox = event.target.querySelector('input');
        checkbox.checked = !checkbox.checked;
            }
            var selectedOptions = Array.from(document.querySelectorAll('#options input:checked')).map(checkbox => checkbox.value);
            if (selectedOptions.length > 0) {
                records[numbers[currentIndex]] = selectedOptions;
            } else {
                delete records[numbers[currentIndex]];
            }
        }

        function nextNumber() {
            if (currentIndex < numbers.length - 1) {
                currentIndex++;
                setNumber();
            }
        }

        function previousNumber() {
            if (currentIndex > 0) {
                currentIndex--;
                setNumber();
            }
        }

        function skipNumber() {
            if (currentIndex < numbers.length - 1) {
                currentIndex++;
                setNumber();
            }
        }

        function searchNumber() {
            var searchNumber = document.getElementById('search-box').value;
            var foundIndex = numbers.indexOf(parseInt(searchNumber));
            if (foundIndex !== -1) {
                currentIndex = foundIndex;
                setNumber();
            }
        }

function generateReport() {
    let htmlContent = `
    <html>
    <head>
        <style>
            table {
                width: 100%;
                border-collapse: collapse;
            }
            th, td {
                border: 1px solid #ddd;
                padding: 8px;
            }
            tr:nth-child(even) {
                background-color: #f2f2f2;
            }
            th {
                padding-top: 12px;
                padding-bottom: 12px;
                text-align: left;
                background-color: #4CAF50;
                color: white;
            }
            img {
                max-width: 100px;
                max-height: 100px;
            }
        </style>
    </head>
    <body>
        <table>
            <tr>
                <th>Number</th>
                <th>Options</th>
                <th>Image</th>
            </tr>`;
    numbers.forEach(number => {
        let record = records[number];
        let image = images[number] ? `<img src="${images[number]}" />` : '';
        if (record) {
            let row = `<tr><td>${number}</td><td>${record.join(' ')}</td><td>${image}</td></tr>`;
            htmlContent += row;
        }
    });
    htmlContent += `</table></body></html>`;
    var encodedUri = 'data:text/html;charset=utf-8,' + encodeURIComponent(htmlContent);
    var link = document.createElement("a");
    link.setAttribute("href", encodedUri);
    link.setAttribute("download", "report.html");
    document.body.appendChild(link);
    link.click();
}



        function previewFile() {
            var file = document.querySelector('input[type=file]').files[0];
            var reader = new FileReader();

            reader.addEventListener("load", function () {
                images[numbers[currentIndex]] = reader.result;
                document.getElementById('photo').style.display = 'block';
                document.getElementById('photo').src = reader.result;
            }, false);

            if (file) {
                reader.readAsDataURL(file);
            }
        }

        setNumber();
    </script>
</body>
</html>
