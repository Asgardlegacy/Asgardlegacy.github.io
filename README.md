<html>
<head>
    <title>Number Manager</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            touch-action: none; /* Prevent double click to zoom */
            user-select: none;
        }
        input {
    user-select: auto;
}
        
         .top-button-group {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        display: flex;
        justify-content: space-between;
        background-color: #fff;
        padding: 1px;
        border: 1px;
        z-index: 1;
        touch-action: none;
    }

    .top-button-group button {
        flex-basis: 20%;
        border: 1px solid black;
       
        height: 35px;
        font-size: 14px;
        touch-action: manipulation;
        border-radius: 5px;
    }
        
        
        
        
        .options-grid {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
        }
        .option,
        .button-group button {
            flex: 1 1 50%;
            box-sizing: border-box;
            border: 1px solid #ccc;
            margin: 0px;
            padding: 0px;
            text-align: center;
            cursor: pointer;
        }
        .option {
          flex: 1 1 48%;
            box-sizing: border-box;
            border: 1px solid #ccc;
            margin: 1px;
            padding: 10px;
            text-align: center;
            cursor: pointer;
            border-radius: 5px;
          
          
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
        left: 0;
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

        .picture-upload {
            position: relative;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
        .picture-upload input[type="file"] {
            display: none;
        }

        .picture-preview {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
            max-width: 100%;
            max-height: 100%;
            border-radius: 5px;
}
  .option2 {
          flex: 1 1 30%;
            box-sizing: border-box;
            border: 1px solid #ccc;
            height: 200px;
            margin: 0px;
            padding: 0px;
            text-align: center;
            cursor: pointer;
            border-radius: 5px;
            }
            .notext{
            user-select: none; /* standard syntax */
    -webkit-user-select: none; /* Chrome, Safari, and Opera syntax */
    -moz-user-select: none; /* Firefox syntax */
    -ms-user-select: none; /* IE and Edge syntax */
}
 .option3 {
          position:absolute;
            flex: 1 1 30%;
          top:95px;
          right: 10px;
            box-sizing: border-box;
            border: 1px solid #ccc;
            height: 100px;
            margin: 0px;
            padding: 0px;
            text-align: center;
            cursor: pointer;
            border-radius: 5px;
            }
            .notext{
            user-select: none; /* standard syntax */
    -webkit-user-select: none; /* Chrome, Safari, and Opera syntax */
    -moz-user-select: none; /* Firefox syntax */
    -ms-user-select: none; /* IE and Edge syntax */
}
#personalInfo {
  position: absolute;
  left: 0;
  top: 20px;
    display: flex;
    justify-content: space-between;
    width: 100%;
    margin: 0 auto;
    padding: 20px 0;
}

#personalInfo input {
    width: 48%; /* Slightly less than half to allow for some space between the inputs */
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
}
            #background-box {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 90px;
    background: white;
     /* Ensures the div is behind all other elements */
}
    </style>
</head>
<body>
 <div id="background-box"></div>
    
<div class="top-button-group">
        <button onclick="downloadJSON()">JSON</button>
        <button onclick="downloadHTML()">HTML</button>
        <button onclick="document.getElementById('loadState').click()" ontouchstart="event.stopPropagation()">Load</button>
   <button onclick="clearDataForNumber()">Clear</button>
    </div>
    
    <input type="file" id="loadState" style="display: none" accept="application/json" onchange="loadState(event)" />

<form id="personalInfo" class="form-group">
    <input type="text" id="fullName" placeholder="First and Last Name" required />
    <input type="tel" id="phoneNumber" placeholder="Phone Number" required />
</form>


    

    <input type="text" pattern="\d*" id="number-search" oninput="searchNumber()" placeholder="Search Unit..." />
    <h1 id="number"></h1>
    <div id="options" class="options-grid"></div>

   <div id="commentArea">
    <label for="comment">Comment:</label><br>
    <textarea id="comment" name="comment" rows="4" cols="50"></textarea>
</div>

   
    <input type="file" id="file" style="display: none" accept="image/*" onchange="handleFileSelect(event)" />
    <img id="photo" style="display: none; max-width: 200px; max-height: 200px; margin-bottom: 10px" />

<div id="options2" class="options-grid">
        <div class="option2">
            <label class="picture-upload">
               <input type="file" accept="image/*" onchange="handlePictureSelect(event, numbers[currentIndex], 1)" />
                Upload Picture
                <img id="picture-preview-1" class="picture-preview" style="display: none;" onclick="handlePictureUpload(numbers[currentIndex], 1)" />
            </label>
        </div>

        <div class="option2">
            <label class="picture-upload">
              <input type="file" accept="image/*" onchange="handlePictureSelect(event, numbers[currentIndex], 2)" />
                Upload Picture
                <img id="picture-preview-2" class="picture-preview" style="display: none;" onclick="handlePictureUpload(numbers[currentIndex], 2)" />
            </label>
        </div>

        <div class="option2">
            <label class="picture-upload">
                               <input type="file" accept="image/*" onchange="handlePictureSelect(event, numbers[currentIndex], 3)" />
                Upload Picture
                <img id="picture-preview-3" class="picture-preview" style="display: none;" onclick="handlePictureUpload(numbers[currentIndex], 3)" />
            </label>
        </div>
       
        <div class="option3">
            <label class="picture-upload">
                               <input type="file" accept="image/*" onchange="handlePictureSelect(event, numbers[currentIndex], 4)" />
                Upload Picture
                <img id="picture-preview-4" class="picture-preview" style="display: none;" onclick="handlePictureUpload(numbers[currentIndex], 4)" />
            </label>
        </div>
       
    </div>

 
 
   
   <div class="button-group">
        
        <button class="notext" onclick="previousNumber()">Previous</button>
        <button class="notext" onclick="nextNumber()">Next</button>
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

        var options = ["Locked", "Vacant", "Overlocked", "No Lock", "Locked Open", "Needs Repair"];
        
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
        var pictureUploads = ['', '', ''];
        
       function setNumber() {
    var number = numbers[currentIndex];
    document.getElementById('number').innerText = `Unit: ${number}`;
    document.getElementById('number-search').value = '';
   var record = records[number] || {options: [], personalInfo: {fullName: "", phoneNumber: ""}};
    options.forEach(option => {
        var checkbox = document.querySelector(`#options .option[data-option="${option}"] input`);
        checkbox.checked = record.options.includes(option);
        checkbox.parentNode.classList.toggle('checked', checkbox.checked);
    });
    var commentBox = document.getElementById("comment");
    commentBox.value = records[number] ? records[number].comment : '';
    updatePicturePreview(number);
     document.getElementById('fullName').value = record.personalInfo.fullName;
    document.getElementById('phoneNumber').value = record.personalInfo.phoneNumber;

}

        function toggleCheckbox(event) {
            var checkbox = event.target.tagName === "INPUT" ? event.target : event.target.querySelector("input");
            checkbox.checked = !checkbox.checked;
            checkbox.parentNode.classList.toggle("checked", checkbox.checked);
            var number = numbers[currentIndex];
            var option = checkbox.parentNode.dataset.option;
             records[number] = records[number] || {options: [], personalInfo: {fullName: "", phoneNumber: ""}};
    if (checkbox.checked) {
        records[number].options.push(option);
    } else {
        var index = records[number].options.indexOf(option);
        records[number].options.splice(index, 1);
    }
        }

        function nextNumber() {
            
                currentIndex = (currentIndex + 1) % numbers.length;
                setNumber();
                saveStateToIndexedDB();  // Save state when the number changes
            } 
        

        function previousNumber() {
            currentIndex = (currentIndex - 1 + numbers.length) % numbers.length;
            setNumber();
            saveStateToIndexedDB();  // Save state when the number changes
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

 function compressImage(file, callback) {
            var img = document.createElement("img");
            var canvas = document.createElement("canvas");
            var ctx = canvas.getContext("2d");
            img.src = URL.createObjectURL(file);
            img.onload = function () {
                var MAX_WIDTH = 400;  // Adjust as needed
                var MAX_HEIGHT = 400;  // Adjust as needed
                var width = img.width;
                var height = img.height;

                if (width > height) {
                    if (width > MAX_WIDTH) {
                        height *= MAX_WIDTH / width;
                        width = MAX_WIDTH;
                    }
                } else {
                    if (height > MAX_HEIGHT) {
                        width *= MAX_HEIGHT / height;
                        height = MAX_HEIGHT;
                    }
                }
                canvas.width = width;
                canvas.height = height;
                ctx.drawImage(img, 0, 0, width, height);

                canvas.toBlob(function (blob) {
                    var newFile = new File([blob], file.name, {type: "image/jpeg", lastModified: Date.now()});
                    callback(newFile);
                }, "image/jpeg", 0.75);
            };
        }


        function handleFileSelect(event) {
            if (event.target.id === "file") {
                var file = event.target.files[0];
                compressImage(file, function (newFile) {
                    var reader = new FileReader();
                    reader.onloadend = function () {
                        images[numbers[currentIndex]] = reader.result;
                        document.getElementById("photo").src = reader.result;
                        document.getElementById("photo").style.display = "block";
                    };
                    reader.readAsDataURL(newFile);
                });
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
/*
       document.getElementById("number").onclick = function () {
            document.getElementById("file").click();
        };
*/
        setNumber();
document.getElementById('personalInfo').addEventListener('change', function() {
    var fullName = document.getElementById('fullName').value;
    var phoneNumber = document.getElementById('phoneNumber').value;

    var number = numbers[currentIndex];
    records[number] = records[number] || {options: [], personalInfo: {fullName: "", phoneNumber: ""}};
    records[number].personalInfo = {fullName, phoneNumber};
});
       

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
/*
        document.addEventListener("touchstart", function (e) {
            e.preventDefault();
        });
*/
        function downloadJSON() {
            var state = JSON.stringify({ records: records, images: images, numbers: numbers, currentIndex: currentIndex });
            var blob = new Blob([state], { type: "application/json" });
            var url = URL.createObjectURL(blob);
var json = JSON.stringify(records);
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
// start of html generation
   function downloadHTML() {
    var report =
        '<html><head><style>table, th, td {border: 1px solid black;} .option {border: 1px solid red; padding: 5px; margin: 5px;}</style></head><body><table><tr><th>Number</th><th>Options</th><th>Name</th><th>Phone</th><th>Image</th><th>Comment</th></tr>';
    for (var number in records) {
        var personalInfo = records[number].personalInfo;
        var fullName = personalInfo.fullName;
        var phoneNumber = personalInfo.phoneNumber;
        var comment = records[number].comment;
         
        var options = '';
        for (let i = 0; i < records[number].options.length; i++) {
            options += `<span class="option">${records[number].options[i]}</span>`;
        }
        
        var image = "";
        for (let i = 1; i <= 4; i++) {
            var pictureKey = `${number}-${i}`;
            if (images[pictureKey]) {
                image += `<img src="${images[pictureKey]}" style="max-width: 100px; max-height: 100px"/>`;
            }
        }
        report += `<tr><td>${number}</td><td>${options}</td><td>${fullName}</td><td>${phoneNumber}</td><td>${image}</td><td>${comment}</td></tr>`;
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
    saveStateToIndexedDB();  // Save state when the number changes
}

// end of html grneration
        document.getElementById("number-search").addEventListener("focus", function () {
            this.parentNode.classList.add("focused");
        });

        document.getElementById("number-search").addEventListener("blur", function () {
            this.parentNode.classList.remove("focused");
        });
        
        function handlePictureUpload(index) {
      var uploadInput = document.getElementById(`picture-upload-${index}`);
      uploadInput.click();
    }
    
    function updatePicturePreview(number) {
    for (let i = 1; i <= 4; i++) {
        var preview = document.getElementById(`picture-preview-${i}`);
        preview.src = images[`${number}-${i}`] || '';
        preview.style.display = images[`${number}-${i}`] ? 'block' : 'none';
    }
}

  function handlePictureSelect(event, number, index) {
    var file = event.target.files[0];
    compressImage(file, function(compressedFile) {
        var reader = new FileReader();
        reader.onloadend = function() {
            images[`${number}-${index}`] = reader.result;
            updatePicturePreview(number);
        };
        reader.readAsDataURL(compressedFile);
    });
}

    
function generateReport() {
    var report = '<html><head><style>table, th, td {border: 1px solid black;}</style></head><body><table><tr><th>Number</th><th>Options</th><th>Image</th></tr>';
    for (var number in records) {
        var options = records[number].join(', ');
        var image = '';
        for (let i = 1; i <= 4; i++) {
            var pictureKey = `${number}-${i}`;
            if (images[pictureKey]) {
                image += `<img src="${images[pictureKey]}" style="max-width: 100px; max-height: 100px"/>`;
            }
        }
        report += `<tr><td>${number}</td><td>${options}</td><td>${image}</td></tr>`;
    }
    report += '</table></body></html>';

    var blob = new Blob([report], { type: 'text/html' });
    var url = URL.createObjectURL(blob);

    var a = document.createElement('a');
    a.href = url;
    a.download = 'report.html';
    a.style.display = 'none';

    document.body.appendChild(a);

    var event = new MouseEvent('click');
    a.dispatchEvent(event);

    document.body.removeChild(a);
    URL.revokeObjectURL(url);
}

  document.getElementById("file").addEventListener("change", handleFileSelect, false);
 document.getElementById('phoneNumber').addEventListener('input', function(e) {
    var value = e.target.value.replace(/\D/g, ''); // remove non-digits
    if (value.length >= 4) {
        value = value.slice(0, 3) + '-' + value.slice(3);
    }
    if (value.length >= 8) {
        value = value.slice(0, 7) + '-' + value.slice(7);
    }
    e.target.value = value;
});
document.addEventListener('touchstart', function(e){
    if (e.target.tagName === 'INPUT') {
        e.preventDefault(); 
        e.target.focus();
        window.setTimeout(function () {
            var sel, range;
            if (window.getSelection && (sel = window.getSelection()).rangeCount) {
                range = sel.getRangeAt(0);
                range.collapse(true);
                range.setEnd(e.target, e.target.value.length);
                range.setStart(e.target, e.target.value.length);
                sel.removeAllRanges();
                sel.addRange(range);
            } else if (e.target.setSelectionRange) {
                e.target.setSelectionRange(e.target.value.length, e.target.value.length);
            }
        }, 1);
    }
});
var db;

  var request = indexedDB.open("storage", 1);

  request.onerror = function(event) {
    alert("Why didn't you allow my web app to use IndexedDB?!");
  };

  request.onsuccess = function(event) {
    db = event.target.result;
    loadStateFromIndexedDB();
  };

  request.onupgradeneeded = function(event) { 
    var db = event.target.result;
  
    if (!db.objectStoreNames.contains('state')) {
      var objectStore = db.createObjectStore('state', { keyPath: 'id' });
    }
  };

  function saveStateToIndexedDB() {
    var state = {
      id: 1,
      records: records,
      images: images,
      numbers: numbers,
      currentIndex: currentIndex
    };

    var transaction = db.transaction(["state"], "readwrite");

    transaction.oncomplete = function(event) {
      console.log("All done!");
    };

    transaction.onerror = function(event) {
      console.log("Error saving state to indexedDB:", event);
    };

    var objectStore = transaction.objectStore("state");
    var request = objectStore.put(state);
    request.onsuccess = function(event) {
      console.log("State saved to indexedDB!");
    };
  }

  function loadStateFromIndexedDB() {
    var transaction = db.transaction(["state"]);
    var objectStore = transaction.objectStore("state");
    var request = objectStore.get(1);

    request.onerror = function(event) {
      console.log("Error loading state from indexedDB:", event);
    };

    request.onsuccess = function(event) {
      if (request.result) {
        records = request.result.records;
        images = request.result.images;
        numbers = request.result.numbers;
        currentIndex = request.result.currentIndex;
        setNumber();
      } else {
        console.log("No data record");
      }
    };
  }
 function clearData() {
    // Clear pictures
    let picArea = document.getElementById("picArea");
    while (picArea.firstChild) {
        picArea.firstChild.remove();
    }

    // Clear checkboxes
    let checkboxesArea = document.getElementById("checkboxesArea");
    while (checkboxesArea.firstChild) {
        checkboxesArea.firstChild.remove();
    }
}
function clearDataForNumber() {
    const number = numbers[currentIndex];
    if (!number) return;
    
    // Clear the data in the records and images objects
    delete records[number];
    for (let i = 1; i <= 4; i++) {
        delete images[`${number}-${i}`];
    }
    
    // Update the UI
    setNumber();

    // Save the state to IndexedDB
    saveStateToIndexedDB();
}
function handleCommentChange(event, number) {
    var comment = event.target.value;
    if (!records[number]) {
        records[number] = { options: [], personalInfo: {}, comment: '' };
    }
    records[number].comment = comment;
    saveStateToIndexedDB();
}
document.getElementById('comment').addEventListener('input', function(e) {
    handleCommentChange(e, numbers[currentIndex]);
});
    </script>
</body>
</html>
