# JSONScript

**Version:** 0.1 Beta

JSONScript is a simple format for defining a sequence of steps to be executed by an application. Each step can include commands to be executed in the terminal, creation of files, and optional comments for documentation.

## Structure

The JSONScript format is an array of objects. Each object represents a step in the process and can have the following fields:

- `cmd`: A command to be executed in the terminal.
- `file`: An object with two properties:
  - `name`: The name of the file to be created, including a path if necessary (defaults to the same directory).
  - `data`: The content to be written into the file.
- `comment`: A description or explanation of the step.

The steps are executed in the order they appear in the array, from top to bottom.

## Example

Here is an example JSONScript file:

```json
[
  {
    "comment": "Step 1: Install the required npm package 'axios' for downloading files.",
    "cmd": "npm install axios"
  },
  {
    "comment": "Step 2: Create a JavaScript file that will contain the code to download the file.",
    "file": {
      "name": "downloadFile.js",
      "data": "// This script downloads a file from a given URL\nconst axios = require('axios');\nconst fs = require('fs');\nconst url = 'https://example.com/file.txt';\nconst path = 'downloaded_file.txt';\naxios({\n  method: 'get',\n  url: url,\n  responseType: 'stream'\n}).then(function (response) {\n  response.data.pipe(fs.createWriteStream(path));\n  console.log('File downloaded successfully.');\n}).catch(function (error) {\n  console.log('Error downloading the file:', error);\n});"
    }
  },
  {
    "comment": "Step 3: Execute the JavaScript file to download the file from the given URL.",
    "cmd": "node downloadFile.js"
  },
  {
    "comment": "Step 4: Create a README file with instructions.",
    "file": {
      "name": "README.md",
      "data": "# Node.js File Downloader\nThis application downloads a file from a specified URL using Node.js.\n\n## Setup\n1. Install the required npm package:\n```\nnpm install axios\n```\n2. Run the script to download the file:\n```\nnode downloadFile.js\n```"
    }
  }
]
```
## Using JSONScript

	1.	Create a JSON file (e.g., steps.json) containing your JSONScript configuration.
	2.	Create a Node.js script (e.g., processSteps.js) to process the JSONScript file.

# Sample Node.js Script
```
const fs = require('fs');
const { exec } = require('child_process');

const steps = require('./steps.json'); // Assuming the JSON is saved as steps.json

steps.forEach((step, index) => {
  if (step.comment) {
    console.log(`Step ${index + 1}: ${step.comment}`);
  }
  if (step.cmd) {
    exec(step.cmd, (error, stdout, stderr) => {
      if (error) {
        console.error(`Error executing command: ${error.message}`);
        return;
      }
      if (stderr) {
        console.error(`Error output: ${stderr}`);
        return;
      }
      console.log(`Output: ${stdout}`);
    });
  }
  if (step.file) {
    fs.writeFile(step.file.name, step.file.data, (err) => {
      if (err) {
        console.error(`Error creating file ${step.file.name}: ${err.message}`);
        return;
      }
      console.log(`File ${step.file.name} created successfully.`);
    });
  }
});
```

	3.	Run the script using Node.js:
```
node processSteps.js
```

This will execute the steps defined in your JSONScript file.

# Contribution

Feel free to contribute to the JSONScript format and its implementation. Fork the repository and submit a pull request with your improvements.

# License

This project is licensed under the MIT License.

```
MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
