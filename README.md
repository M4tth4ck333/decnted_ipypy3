LICENSE
=======

Copyright 2013 Google Inc. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

Get Ready to Build Chrome Apps!
===============================
Chrome apps are written in HTML5, JavaScript, and CSS, just like web apps.
But they look and behave more like native apps, and they have super-powerful capabilities,
like the ability to interact with network and hardware devices, media tools, and much more.


> This repository contains only the code for the Codelab. Please, make sense of this code by following the step-by-step tutorial in the [Chrome Apps docs site](http://developer.chrome.com/trunk/apps/app_codelab.html)
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, minimum-scale=1" />
    <title>Container & Pyodide HMI Gateway</title>
    <!-- Pyodide Loader für Python in WebAssembly -->
    <script src="https://cdn.jsdelivr.net/pyodide/v0.25.0/full/pyodide.js"></script>
    <style>
        body { font-family: monospace; background: #121212; color: #00ffcc; padding: 20px; }
        #output { background: #1e1e1e; border: 1px solid #333; padding: 15px; min-height: 150px; white-space: pre-wrap; }
        button { background: #00ffcc; color: #121212; border: none; padding: 10px 20px; font-weight: bold; cursor: pointer; margin-top: 10px; }
        button:hover { background: #00b38f; }
    </style>
</head>
<body>
    <h1>[DECENTRALIZED EDGE HMI]</h1>
    <p>Status: Lade Python WebAssembly (Pyodide)...</p>
    
    <div id="output">Initialisiere Pyodide-Kernel...</div>
    <button onclick="runPythonCode()">Python-Logik ausführen</button>

    <script>
        let pyodideInstance = null;

        async function main() {
            pyodideInstance = await loadPyodide();
            document.getElementById('output').innerText = "Pyodide bereit!\nBereit für lokale Berechnungen, Media-Pipelines und IPython-Brücken.";
        }
        main();

        async function runPythonCode() {
            if (!pyodideInstance) {
                alert("Pyodide lädt noch!");
                return;
            }
            
            // Hier läuft echter Python-Code direkt in der WASM-Laufzeitumgebung im Browser
            let code = `
import sys
import js

# Beispiel: Python-Systeminfos & mathematische Simulation
version = sys.version.split()[0]
result = f"Python v{version} läuft lokal im WASM-Container.\\n"
result += "Hardware-Schnittstellen: Verbunden mit Pyodide-WebSocket-Bridge."
result
            `;
            
            try {
                let out = await pyodideInstance.runPythonAsync(code);
                document.getElementById('output').innerText = out;
            } catch (err) {
                document.getElementById('output').innerText = "Fehler: " + err;
            }
        }
    </script>
</body>
</html>
