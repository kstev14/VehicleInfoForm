<html lang="en"><head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Form Copy App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      margin: 20px;
      color: #fff;
      background-color: #333;
    }
    .disclaimer {
      font-weight: bold;
      color: #ff8c00; /* Orange color */
      margin-bottom: 20px;
      text-align: center;
      font-size: 18px;
    }
    #app-container {
      display: flex;
      width: 100%;
      max-width: 1200px;
    }
    #outputHistory {
      width: 40%;
      margin-right: 20px;
      max-height: 300px;
      overflow-y: auto;
      background-color: #444;
      padding: 20px;
      border-radius: 10px;
    }
    #outputHistory h2 {
      font-size: 20px;
      font-weight: bold;
      margin-bottom: 10px;
      text-align: center;
      position: sticky;
      top: 0;
      background-color: #444;
    }
    #outputHistory ul {
      list-style: none;
      padding: 0;
    }
    .historyItem {
      margin-bottom: 10px;
      border: 1px solid #555;
      padding: 10px;
      border-radius: 5px;
    }
    #formContainer {
      width: 60%;
      background-color: #555;
      padding: 20px;
      border-radius: 10px;
    }
    .formTitle {
      font-size: 20px;
      font-weight: bold;
      margin-bottom: 10px;
    }
    .formSubtitle {
      font-size: 14px;
      color: #ccc;
      margin-bottom: 5px;
      text-align: center;
    }
    label {
      display: block;
      margin-bottom: 5px;
    }
    input, select, textarea {
      width: 100%;
      padding: 8px;
      margin-bottom: 10px;
      box-sizing: border-box;
      background-color: #777;
      color: #fff;
      border: none;
      border-radius: 5px;
    }
    .multiselect {
      width: 100%;
      padding: 8px;
      margin-bottom: 10px;
      box-sizing: border-box;
      background-color: #777;
      color: #fff;
      border: none;
      border-radius: 5px;
    }
    .multiselect option {
      background-color: #555;
    }
    .checkbox-group {
      margin-bottom: 10px;
      background-color: #777;
      padding: 10px;
      border-radius: 5px;
    }
    .checkbox-group div {
      display: flex;
      align-items: center;
      margin-bottom: 5px;
    }
    .checkbox-group input {
      margin-right: 5px;
    }
    button {
      background-color: #ff8c00;
      color: #fff;
      padding: 15px;
      border: none;
      cursor: pointer;
      margin-right: 5px;
      border-radius: 10px;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <div class="disclaimer">
    Before we proceed, I need to advise that your call will be recorded for training, substantiation, and quality assurance purposes.
  </div>

  <div id="app-container">
    <div id="outputHistory">
      <h2 class="formTitle">Output History</h2>
      <ul id="outputHistoryList"></ul>
    </div>

    <div id="formContainer">
      <h2 class="formTitle">Vehicle Information</h2>

      <form id="myForm">
        <!-- Form Fields -->
        <label for="name">Name:</label>
        <input type="text" id="name" name="name" required="">

        <label for="employer">Employer:</label>
        <input type="text" id="employer" name="employer" required="">

        <label for="vehicle">Year, make &amp; model:</label>
        <input type="text" id="vehicle" name="vehicle">

        <label for="employmentType">Type of Employment:</label>
        <select id="employmentType" name="employmentType" required="">
          <option value="PFT">PFT (Full-Time)</option>
          <option value="PPT">PPT (Part-Time)</option>
        </select>

        <label for="leaseTerm">Lease Term:</label>
        <p class="formSubtitle">Ctrl Click to select more than one</p>
        <select id="leaseTerm" name="leaseTerm" class="multiselect" multiple="">
          <option value="1">1 Year</option>
          <option value="2">2 Years</option>
          <option value="3">3 Years</option>
          <option value="4">4 Years</option>
          <option value="5">5 Years</option>
        </select>

        <label for="payCycle">Pay Cycle:</label>
        <select id="payCycle" name="payCycle" required="">
          <option value="Weekly">Weekly</option>
          <option value="Fortnightly">Fortnightly</option>
          <option value="Monthly">Monthly</option>
        </select>

        <label for="grossSalary">Gross Salary (AUD):</label>
        <input type="text" id="grossSalary" name="grossSalary" pattern="^\d+(\.\d{1,2})?$" title="Enter a valid number with up to two decimal places" required="">

        <label for="avgKm">Average Kilometres per year:</label>
        <input type="text" id="avgKm" name="avgKm">

        <label for="timeframe">Timeframe:</label>
        <input type="text" id="timeframe" name="timeframe">

        <label for="notes">Notes:</label>
        <textarea id="notes" name="notes" rows="4"></textarea>

        <button type="button" onclick="copyToClipboard()">Copy</button>
        <button type="button" onclick="resetForm()">Reset</button>
      </form>
    </div>
  </div>

  <script>
    function copyToClipboard() {
      const form = document.getElementById('myForm');
      const formData = new FormData(form);
      let clipboardData = "";

      for (const [key, value] of formData.entries()) {
        if (key === 'leaseTerm' && Array.isArray(value)) {
          const selectedLeases = value.map(term => `${term} Year${term > 1 ? 's' : ''}`);
          clipboardData += `${key}: ${selectedLeases.join(', ')}\n`;
        } else {
          clipboardData += `${key}: ${value}\n`;
        }
      }

      navigator.clipboard.writeText(clipboardData).then(() => {
        alert('Form data copied to clipboard!');
        addToHistory(clipboardData);
      }).catch(err => {
        console.error('Unable to copy to clipboard', err);
      });
    }

    function resetForm() {
      document.getElementById('myForm').reset();
    }

    function addToHistory(data) {
      const historyList = document.getElementById('outputHistoryList');
      const listItem = document.createElement('li');
      listItem.classList.add('historyItem');
      listItem.textContent = data;
      historyList.insertBefore(listItem, historyList.firstChild);
      // Remove last item if more than 6 records
      if (historyList.children.length > 6) {
        historyList.removeChild(historyList.lastChild);
      }
    }
  </script>


</body></html>
