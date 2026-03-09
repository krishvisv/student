---
layout: post
toc: true
title: Titanic ML Data Structure HW
description: Frontend to call the data structure in backend
permalink: /titanic-ml/
---

<style>
  .titanic-container {
    font-family: sans-serif;
    max-width: 600px;
    margin: 0 auto;
  }
  .form-group {
    margin-bottom: 14px;
  }
  .form-group label {
    display: block;
    font-weight: bold;
    margin-bottom: 4px;
  }
  .form-group input, .form-group select {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 6px;
    font-size: 14px;
    box-sizing: border-box;
    color: white;
  }
  .form-row {
    display: flex;
    gap: 16px;
  }
  .form-row .form-group {
    flex: 1;
  }
  #predict-btn {
    background: #2ecc71;
    color: white;
    border: none;
    padding: 12px 28px;
    border-radius: 6px;
    font-size: 16px;
    cursor: pointer;
    width: 100%;
    margin-top: 8px;
  }
  #predict-btn:hover { background: #27ae60; }
  #result-box {
    display: none;
    margin-top: 24px;
    padding: 16px;
    border-radius: 8px;
    border: 1px solid #ddd;
    background: #303030ff;
  }
  #result-box h3 { margin: 0 0 10px; }
  .bar-wrap {
    background: #eee;
    border-radius: 6px;
    overflow: hidden;
    height: 28px;
    margin: 8px 0;
    display: flex;
  }
  .bar-die     { background: #e74c3c; display:flex; align-items:center; justify-content:center; color:white; font-size:12px; font-weight:bold; }
  .bar-survive { background: #2ecc71; display:flex; align-items:center; justify-content:center; color:white; font-size:12px; font-weight:bold; }
  .verdict {
    font-size: 1.1em;
    font-weight: bold;
    margin-top: 10px;
  }
  .error-box {
    color: #c0392b;
    background: #fdecea;
    padding: 10px;
    border-radius: 6px;
    margin-top: 16px;
  }
</style>

<div class="titanic-container">

  <p>Fill in the passenger details below and click <strong>Predict</strong> to see the survival probability.</p>

  <div class="form-group">
    <label for="name">Passenger Name</label>
    <input type="text" id="name" value="John Mortensen" />
  </div>

  <div class="form-row">
    <div class="form-group">
      <label for="pclass">Passenger Class</label>
      <select id="pclass">
        <option value="1">1st Class</option>
        <option value="2" selected>2nd Class</option>
        <option value="3">3rd Class</option>
      </select>
    </div>
    <div class="form-group">
      <label for="sex">Sex</label>
      <select id="sex">
        <option value="male" selected>Male</option>
        <option value="female">Female</option>
      </select>
    </div>
  </div>

  <div class="form-row">
    <div class="form-group">
      <label for="age">Age</label>
      <input type="number" id="age" value="66" min="1" max="100" />
    </div>
    <div class="form-group">
      <label for="fare">Fare ($)</label>
      <input type="number" id="fare" value="16.00" min="0" step="0.01" />
    </div>
  </div>

  <div class="form-row">
    <div class="form-group">
      <label for="sibsp">Siblings / Spouses Aboard</label>
      <input type="number" id="sibsp" value="1" min="0" max="10" />
    </div>
    <div class="form-group">
      <label for="parch">Parents / Children Aboard</label>
      <input type="number" id="parch" value="1" min="0" max="10" />
    </div>
  </div>

  <div class="form-row">
    <div class="form-group">
      <label for="embarked">Port of Embarkation</label>
      <select id="embarked">
        <option value="S" selected>Southampton</option>
        <option value="C">Cherbourg</option>
        <option value="Q">Queenstown</option>
      </select>
    </div>
    <div class="form-group">
      <label for="alone">Travelling Alone?</label>
      <select id="alone">
        <option value="false" selected>No</option>
        <option value="true">Yes</option>
      </select>
    </div>
  </div>

  <button id="predict-btn" onclick="predict()">🔮 Predict Survival</button>

  <div id="result-box"></div>
  <div id="error-box"></div>

</div>

<script>
const API_URL = "http://127.0.0.1:8001/api/titanic/predict";

async function predict() {
  const btn = document.getElementById("predict-btn");
  const resultBox = document.getElementById("result-box");
  const errorBox  = document.getElementById("error-box");

  btn.textContent = "⏳ Predicting...";
  btn.disabled = true;
  resultBox.style.display = "none";
  errorBox.innerHTML = "";

  const passenger = {
    name:     document.getElementById("name").value,
    pclass:   parseInt(document.getElementById("pclass").value),
    sex:      document.getElementById("sex").value,
    age:      parseFloat(document.getElementById("age").value),
    sibsp:    parseInt(document.getElementById("sibsp").value),
    parch:    parseInt(document.getElementById("parch").value),
    fare:     parseFloat(document.getElementById("fare").value),
    embarked: document.getElementById("embarked").value,
    alone:    document.getElementById("alone").value === "true"
  };

  try {
    const response = await fetch(API_URL, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(passenger)
    });

    if (!response.ok) throw new Error(`Server error: ${response.status}`);

    const data = await response.json();
    const survive = data.survive;
    const die     = data.die;
    const verdict = survive > 0.5
      ? "🟢 This passenger likely SURVIVED"
      : "🔴 This passenger likely DID NOT SURVIVE";

    const survPct = (survive * 100).toFixed(1);
    const diePct  = (die     * 100).toFixed(1);

    resultBox.innerHTML = `
      <div class="bar-wrap">
        <div class="bar-die"     style="width:${diePct}%">${diePct}% Die</div>
        <div class="bar-survive" style="width:${survPct}%">${survPct}% Survive</div>
      </div>
      <p>💀 Die probability: <strong>${diePct}%</strong></p>
      <p>🛟 Survive probability: <strong>${survPct}%</strong></p>
      <div class="verdict">${verdict}</div>
    `;
    resultBox.style.display = "block";

  } catch (err) {
    errorBox.innerHTML = `<div class="error-box">❌ ${err.message} — is your Flask server running?</div>`;
  } finally {
    btn.textContent = "🔮 Predict Survival";
    btn.disabled = false;
  }
}
</script>