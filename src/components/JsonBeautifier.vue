<template>
    <div :class="{'dark-mode': darkMode}" class="json-beautifier">
      <h1>JSON Beautifier</h1>
  
      <div class="json-container">
        <!-- Left Column (Input JSON) -->
        <div class="column">
          <textarea
            v-model="jsonInput"
            @input="beautifyJson"
            placeholder="Paste your JSON here"
            rows="15"
          ></textarea>
        </div>
  
        <!-- Right Column (Beautified JSON) -->
        <div class="column">
          <textarea
            :value="formattedJson"
            @input="syncInput"
            rows="15"
          ></textarea>
        </div>
      </div>
  
      <!-- Error Handling: Displaying highlighted error part -->
      <p v-if="error" class="error">{{ error }}</p>
  
      <!-- Clear Button -->
      <button @click="clearFields">Clear</button>
  
      <!-- Download Button -->
      <button @click="downloadJson" :disabled="!formattedJson">Download JSON</button>
  
      <!-- Dark Mode Toggle -->
      <button @click="toggleDarkMode" class="dark-mode-toggle">
        <i class="fa" :class="darkMode ? 'fa-sun' : 'fa-moon'">{{darkMode ? 'Light' : 'Dark' }} Mode</i> <!-- Font Awesome icon -->
      </button>
    </div>
  </template>
  
  <script>
  // Import the flatted library to handle circular references
//   import { stringify } from 'flatted';
  
  export default {
    data() {
      return {
        jsonInput: '', // User input JSON string
        formattedJson: '', // Beautified JSON string
        error: '', // Error message
        darkMode: false, // Dark mode toggle state
      };
    },
    methods: {
      beautifyJson() {
        try {
          // Try parsing and formatting the JSON
          const parsedJson = JSON.parse(this.jsonInput);
  
          // Custom beautifying function for JSON formatting with better spacing
          this.formattedJson = this.customBeautify(parsedJson);
          this.error = ''; // Clear any previous errors
        } catch (e) {
          // If there's an error parsing JSON, show the error message
          this.error = 'Invalid JSON input or circular reference detected';
          this.formattedJson = ''; // Clear any previous formatted JSON
        }
      },
  
      customBeautify(parsedJson) {
        return JSON.stringify(parsedJson, null, 2)  // Use 2 spaces for indentation
        .replace(/(\[|\{)/g, '\n$1')  // Newline before opening brackets/objects
        .replace(/(\]|\})/g, '$1\n')  // Newline after closing brackets/objects
        .replace(/\n\s*\n/g, '\n');  // Remove unnecessary extra newlines
      },
  
      // Sync input between left and right when pasting beautified JSON
      syncInput(event) {
        this.jsonInput = event.target.value;
        this.beautifyJson(); // Beautify again when pasting into the right
      },
  
      clearFields() {
        this.jsonInput = ''; // Clear the input
        this.formattedJson = ''; // Clear the formatted JSON
        this.error = ''; // Clear the error
      },
  
      downloadJson() {
        // Create a Blob from the beautified JSON
        const blob = new Blob([this.formattedJson], { type: 'application/json' });
  
        // Create an anchor element to trigger the download
        const link = document.createElement('a');
        link.href = URL.createObjectURL(blob);
        link.download = 'beautified.json';
        link.click();
      },
  
      toggleDarkMode() {
        this.darkMode = !this.darkMode;
      },
    },
  };
  </script>
  
  <style scoped>
  /* General Styles */
  .json-beautifier {
    text-align: center;
    padding: 20px;
    font-family: 'Courier New', monospace;
  }
  
  h1 {
    font-size: 2em;
    margin-bottom: 20px;
  }
  
  .json-container {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
  }
  
  .column {
    flex: 1;
    padding: 10px;
    margin: 5px;
  }
  
  textarea {
    width: 100%;
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  
  pre {
    background-color: #f4f4f4;
    padding: 20px;
    border-radius: 5px;
    text-align: left;
    white-space: pre-wrap;
    word-wrap: break-word;
  }
  
  button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 5px;
    margin-bottom: 20px;
    margin-left: 10px;
  }
  
  button:hover {
    background-color: #45a049;
  }
  
  .error {
    color: red;
    font-size: 16px;
    margin-top: 10px;
  }
  
  .dark-mode {
    background-color: #2c3e50;
    color: white;
  }
  
  .dark-mode button {
    background-color: #34495e;
  }
  
  .dark-mode textarea {
    background-color: #34495e;
    color: white;
    border: 1px solid #2c3e50;
  }
  
  .dark-mode pre {
    background-color: #34495e;
    color: white;
  }
  
  button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
  
  /* Highlighting error in JSON input */
  textarea:invalid {
    border-color: red;
  }


  .dark-mode {
    background-color: #2c3e50;
    color: white;
  }
  
  .dark-mode button {
    background-color: #34495e;
  }
  
  .dark-mode textarea {
    background-color: #34495e;
    color: white;
    border: 1px solid #2c3e50;
  }
  
  .dark-mode pre {
    background-color: #34495e;
    color: white;
  }
  
  .dark-mode-toggle {
    position: absolute;
    top: 20px;
    right: 20px;
    background-color: transparent;
    border: none;
    cursor: pointer;
    font-size: 24px;
  }
  
  .dark-mode-toggle i {
    color: #fff;
  }
  
  .dark-mode-toggle i.fa-sun {
    color: yellow;
  }
  
  .dark-mode-toggle i.fa-moon {
    color: lightgray;
  }

  </style>
  