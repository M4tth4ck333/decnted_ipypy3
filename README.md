<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, minimum-scale=1" />
    <title>Pyodide WASM HMI Gateway</title>
    <!-- Pyodide Core laden -->
    <script src="https://cdn.jsdelivr.net/pyodide/v0.25.0/full/pyodide.js"></script>
    <style>
        body {
            font-family: monospace;
            background-color: #0d1117;
            color: #58a6ff;
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 800px;
            margin: auto;
            background: #161b22;
            border: 1px solid #30363d;
            border-radius: 6px;
            padding: 20px;
        }
        h1 { font-size: 1.5rem; color: #f0f6fc; }
        textarea {
            width: 100%;
            height: 120px;
            background: #0d1117;
            color: #c9d1d9;
            border: 1px solid #30363d;
            padding: 10px;
            font-family: monospace;
            border-radius: 4px;
            resize: vertical;
        }
        button {
            background-color: #238636;
            color: white;
            border: none;
            padding: 10px 16px;
            font-weight: bold;
            border-radius: 4px;
            cursor: pointer;
            margin-top: 10px;
        }
        button:hover { background-color: #2ea043; }
        #output {
            background: #0d1117;
            border: 1px solid #30363d;
            padding: 15px;
            margin-top: 15px;
            border-radius: 4px;
            white-space: pre-wrap;
            min-height: 80px;
            color: #7ee787;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>WASM & Pyodide App-Sandbox</h1>
        <p>Dieses Interface ersetzt alte Chrome-Packaged-Apps durch einen standardisierten WebAssembly-Container.</p>
        
        <label for="code">Python-Code eingeben:</label><br>
        <textarea id="code">
import sys
import js

version = sys.version.split()[0]
output_msg = f"Erfolgreich ausgeführt in Python v{version} (WASM)\n"
output_msg += "System-Status: Bereit für WebSocket-Bridge und Sensoren."
output_msg
        </textarea><br>
        
        <button onclick="runPython()">Code ausführen</button>

        <h3>Ausgabe:</h3>
        <div id="output">Initialisiere Python-Umgebung...</div>
    </div>

    <script>
        let pyodide = null;

        async function initPyodide() {
            let outputDiv = document.getElementById("output");
            try {
                pyodide = await loadPyodide();
                outputDiv.innerText = "Pyodide erfolgreich geladen! Bereit für Berechnungen.";
            } catch (err) {
                outputDiv.innerText = "Fehler beim Laden von Pyodide: " + err;
            }
        }

        initPyodide();

        async function runPython() {
            let outputDiv = document.getElementById("output");
            let code = document.getElementById("code").value;
            
            if (!pyodide) {
                outputDiv.innerText = "Pyodide ist noch nicht bereit.";
                return;
            }

            try {
                let result = await pyodide.runPythonAsync(code);
                outputDiv.innerText = result;
            } catch (err) {
                outputDiv.innerText = "Python-Fehler:\n" + err;
            }
        }
    </script>
</body>
</html>
