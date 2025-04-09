---
layout: page
title: SD Road Analysis
search_exclude: false
permalink: /roads
---

<table>
  <thead>
    <tr>
      <th>Field</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <!-- pwidth -->
    <tr>
      <td title="Width of the pavement in feet (e.g., 24, 36, 48).">
        pwidth
      </td>
      <td>
        <input type="number" name="pwidth" step="0.01" placeholder="e.g., 36" />
      </td>
    </tr>
    <!-- pav_length -->
    <tr>
      <td title="Length of the paved section in feet (e.g., 400.63).">
        pav_length
      </td>
      <td>
        <input type="number" name="pav_length" step="0.01" placeholder="e.g., 400.63" />
      </td>
    </tr>
    <!-- paveclass -->
    <tr>
      <td title="Type of pavement classification. For example, AC Improved, Concrete, etc.">
        paveclass
      </td>
      <td>
        <select name="paveclass">
          <option value="">-- Select Pavement Class --</option>
          <option value="AC Improved">AC Improved</option>
          <option value="AC Overlay">AC Overlay</option>
          <option value="Concrete">Concrete</option>
          <option value="PCC Jointed Concrete">PCC Jointed Concrete</option>
          <option value="UnSurfaced">UnSurfaced</option>
          <!-- Add more categories as needed -->
        </select>
      </td>
    </tr>
    <!-- funclass -->
    <tr>
      <td title="Functional classification of the road. For example, CL 2 LANE SUB-COLLECTOR, RES CUL DE SAC, etc.">
        funclass
      </td>
      <td>
        <select name="funclass">
          <option value="">-- Select Functional Class --</option>
          <option value="CL 2 LANE SUB-COLLECTOR">CL 2 LANE SUB-COLLECTOR</option>
          <option value="RES RESIDENTIAL LOCAL STREET">RES RESIDENTIAL LOCAL STREET</option>
          <option value="COLLECTOR">COLLECTOR</option>
          <option value="CL 2 LANE COLLECTOR WITH 2 WAY LEFT TURN">CL 2 LANE COLLECTOR WITH 2 WAY LEFT TURN</option>
          <option value="CL 4 LN COLLECTOR WITH 2 WY LEFT TURN LN">CL 4 LN COLLECTOR WITH 2 WY LEFT TURN LN</option>
          <option value="RES CUL DE SAC">RES CUL DE SAC</option>
          <option value="ALLEY">ALLEY</option>
          <!-- Add more categories as needed -->
        </select>
      </td>
    </tr>
  </tbody>
</table>

<br />
<button onclick="submitData()">Submit</button>

<div id="response-container" style="margin-top: 20px; border: 1px solid #ccc; padding: 10px; display: none;">
  <h3>Server Response</h3>
  <pre id="response-output" style="white-space: pre-wrap;"></pre>
</div>

<script type="module">
  import { pythonURI, fetchOptions } from "{{site.baseurl}}/assets/js/api/config.js";

  // Define the function and attach it to the window object
  window.submitData = function submitData() {
    // Gather form values
    const pwidth = document.querySelector('input[name="pwidth"]').value;
    const pav_length = document.querySelector('input[name="pav_length"]').value;
    const paveclass = document.querySelector('select[name="paveclass"]').value;
    const funclass = document.querySelector('select[name="funclass"]').value;

    // Construct the payload
    const payload = {
      pwidth: parseFloat(pwidth) || 0,
      pav_length: parseFloat(pav_length) || 0,
      paveclass: paveclass,
      funclass: funclass
    };

    // POST request to /api/roads
    fetch(`${pythonURI}/api/roads`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(payload)
    })
      .then(response => response.json())
      .then(data => {
        // Display the response in the frontend
        const responseContainer = document.getElementById('response-container');
        const responseOutput = document.getElementById('response-output');
        responseOutput.textContent = JSON.stringify(data, null, 2);
        responseContainer.style.display = 'block';
      })
      .catch(error => {
        console.error('Error:', error);
        const responseContainer = document.getElementById('response-container');
        const responseOutput = document.getElementById('response-output');
        responseOutput.textContent = 'Error: ' + error.message;
        responseContainer.style.display = 'block';
      });
  };
</script>
