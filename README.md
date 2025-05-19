<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>ACLS 記錄工具</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    button { margin: 5px; padding: 10px 20px; }
    #log { margin-top: 20px; white-space: pre-line; border-top: 1px solid #ccc; padding-top: 10px; }
  </style>
</head>
<body>

<h2>ACLS 記錄工具</h2>

<button onclick="recordEvent('CPR 開始')">Start CPR</button>
<button onclick="aedAnalyze()">AED Analyze</button>
<button onclick="recordEvent('Epinephrine 給藥')">Epinephrine</button>
<button onclick="recordEvent('Amiodarone 給藥')">Amiodarone</button>
<button onclick="switchCPR()">Switch CPR</button>
<button onclick="recordEvent('ROSC')">ROSC</button>
<button onclick="resetLog()">Reset</button>

<div id="log"></div>

<script>
  let startTime = null;
  let cprSwitchTimer = null;

  function getCurrentTime() {
    return new Date();
  }

  function formatTimeDiff(ms) {
    let totalSeconds = Math.floor(ms / 1000);
    let minutes = String(Math.floor(totalSeconds / 60)).padStart(2, '0');
    let seconds = String(totalSeconds % 60).padStart(2, '0');
    return `${minutes}:${seconds}`;
  }

  function recordEvent(event) {
    const now = getCurrentTime();
    if (!startTime) {
      startTime = now;
    }
    const elapsed = formatTimeDiff(now - startTime);
    const log = document.getElementById("log");
    log.innerText += `[${elapsed}] ${event}\n`;
  }

  function aedAnalyze() {
    recordEvent("AED 分析開始（倒數 10 秒）");
    setTimeout(() => {
      recordEvent("AED 分析結束");
    }, 10000);
  }

  function switchCPR() {
    recordEvent("CPR 換人");
    if (cprSwitchTimer) {
      clearTimeout(cprSwitchTimer);
    }
    cprSwitchTimer = setTimeout(() => {
      alert("提醒：2 分鐘已到，建議 CPR 換人！");
    }, 120000);
  }

  function resetLog() {
    if (confirm("確定要清除所有紀錄？")) {
      document.getElementById("log").innerText = "";
      startTime = null;
      if (cprSwitchTimer) {
        clearTimeout(cprSwitchTimer);
      }
    }
  }
</script>

</body>
</html>
