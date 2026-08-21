# Ex04 Simple Calculator - React Project
## Date:21.08.2026
## Name : Harikrishnan P
## Reg No : 212225040112

## AIM
To  develop a Simple Calculator using React.js with clean and responsive design, ensuring a smooth user experience across different screen sizes.

## ALGORITHM
### STEP 1
Create a React App.

### STEP 2
Open a terminal and run:
  <ul><li>npx create-react-app simple-calculator</li>
  <li>cd simple-calculator</li>
  <li>npm start</li></ul>

### STEP 3
Inside the src/ folder, create a new file Calculator.js and define the basic structure.

### STEP 4
Plan the UI: Display screen, number buttons (0-9), operators (+, -, *, /), clear (C), and equal (=).

### STEP 5
Create a new file Calculator.css in src/ and add the styling.

### STEP 6
Open src/App.js and modify it.

### STEP 7
Start the development server.
  npm start

### STEP 8
Open http://localhost:3000/ in the browser.

### STEP 9
Test the calculator by entering numbers and operations.

### STEP 10
Fix styling issues and refine content placement.

### STEP 11
Deploy the website.

### STEP 12
Upload to GitHub Pages for free hosting.

## PROGRAM

App.js

````
import { useState } from "react";

function useCalculator() {
  const [display, setDisplay] = useState("0");
  const [previousValue, setPreviousValue] = useState(null);
  const [operator, setOperator] = useState(null);
  const [waitingForOperand, setWaitingForOperand] = useState(false);


  const inputNumber = (number) => {
    if (display === "Error") {
      setDisplay(number);
      setPreviousValue(null);
      setOperator(null);
      setWaitingForOperand(false);
      return;
    }

    if (waitingForOperand) {
      setDisplay(number);
      setWaitingForOperand(false);
    } else {
      setDisplay(display === "0" ? number : display + number);
    }
  };

  const inputDecimal = () => {
    if (display === "Error") {
      setDisplay("0.");
      setPreviousValue(null);
      setOperator(null);
      setWaitingForOperand(false);
      return;
    }

    if (waitingForOperand) {
      setDisplay("0.");
      setWaitingForOperand(false);
      return;
    }

    if (!display.includes(".")) {
      setDisplay(display + ".");
    }
  };

  const clearCalculator = () => {
    setDisplay("0");
    setPreviousValue(null);
    setOperator(null);
    setWaitingForOperand(false);
  };

  const deleteNumber = () => {
    if (display === "Error" || waitingForOperand) return;

    if (display.length === 1) {
      setDisplay("0");
    } else {
      setDisplay(display.slice(0, -1));
    }
  };

  const calculate = (first, second, operation) => {
    switch (operation) {
      case "+":
        return first + second;

      case "-":
        return first - second;

      case "×":
        return first * second;

      case "÷":
        return second === 0 ? "Error" : first / second;

      default:
        return second;
    }
  };

  const handleOperator = (nextOperator) => {
    const inputValue = parseFloat(display);

    if (isNaN(inputValue)) return;

    if (operator && waitingForOperand) {
      setOperator(nextOperator);
      return;
    }

    if (previousValue === null) {
      setPreviousValue(inputValue);
    } else if (operator) {
      const result = calculate(
        previousValue,
        inputValue,
        operator
      );

      if (result === "Error") {
        setDisplay("Error");
        setPreviousValue(null);
        setOperator(null);
        setWaitingForOperand(true);
        return;
      }

      setDisplay(String(result));
      setPreviousValue(result);
    }

    setOperator(nextOperator);
    setWaitingForOperand(true);
  };

  const handleEquals = () => {
    if (operator === null || previousValue === null) {
      return;
    }

    const inputValue = parseFloat(display);

    const result = calculate(
      previousValue,
      inputValue,
      operator
    );

    if (result === "Error") {
      setDisplay("Error");
    } else {
      setDisplay(String(result));
    }

    setPreviousValue(null);
    setOperator(null);
    setWaitingForOperand(true);
  };

  const handlePercentage = () => {
    const value = parseFloat(display);

    if (isNaN(value)) return;

    setDisplay(String(value / 100));
  };

  return {
    display,
    previousValue,
    operator,
    inputNumber,
    inputDecimal,
    clearCalculator,
    deleteNumber,
    handleOperator,
    handleEquals,
    handlePercentage,
  };
}

export default useCalculator;
````

App.jsx

````
import { useEffect } from "react";
import useCalculator from "./App";
import "./App.css";

function App() {
  const {
    display,
    previousValue,
    operator,
    inputNumber,
    inputDecimal,
    clearCalculator,
    deleteNumber,
    handleOperator,
    handleEquals,
    handlePercentage,
  } = useCalculator();

  useEffect(() => {
    const handleKeyDown = (event) => {
      const key = event.key;

      if (key >= "0" && key <= "9") {
        inputNumber(key);
      } 
      else if (key === ".") {
        inputDecimal();
      } 
      else if (key === "+") {
        handleOperator("+");
      } 
      else if (key === "-") {
        handleOperator("-");
      } 
      else if (key === "*") {
        handleOperator("×");
      } 
      else if (key === "/") {
        event.preventDefault();
        handleOperator("÷");
      } 
      else if (key === "%") {
        handlePercentage();
      } 
      else if (key === "Enter" || key === "=") {
        handleEquals();
      } 
      else if (key === "Backspace") {
        deleteNumber();
      } 
      else if (key === "Escape") {
        clearCalculator();
      }
    };

    window.addEventListener("keydown", handleKeyDown);

    return () => {
      window.removeEventListener("keydown", handleKeyDown);
    };
  });

  const buttons = [
    ["AC", "action"],
    ["DEL", "action"],
    ["%", "action"],
    ["÷", "operator"],

    ["7", "number"],
    ["8", "number"],
    ["9", "number"],
    ["×", "operator"],

    ["4", "number"],
    ["5", "number"],
    ["6", "number"],
    ["-", "operator"],

    ["1", "number"],
    ["2", "number"],
    ["3", "number"],
    ["+", "operator"],

    ["0", "number zero"],
    [".", "number"],
    ["=", "equals"],
  ];

  const handleButtonClick = (value) => {
    if (value >= "0" && value <= "9") {
      inputNumber(value);
    } 
    else if (value === ".") {
      inputDecimal();
    } 
    else if (value === "AC") {
      clearCalculator();
    } 
    else if (value === "DEL") {
      deleteNumber();
    } 
    else if (value === "%") {
      handlePercentage();
    } 
    else if (
      value === "+" ||
      value === "-" ||
      value === "×" ||
      value === "÷"
    ) {
      handleOperator(value);
    } 
    else if (value === "=") {
      handleEquals();
    }
  };

  return (
    <div className="app">

      <div className="calculator">

        <div className="calculator-header">
          <span>CALCULATOR</span>
          <div className="status-dot"></div>
        </div>

        <div className="display">

          <div className="previous-display">
            {previousValue !== null
              ? `${previousValue} ${operator || ""}`
              : ""}
          </div>

          <div className="current-display">
            {display}
          </div>

        </div>

        <div className="buttons">

          {buttons.map(([value, type], index) => (
            <button
              key={index}
              className={`calc-button ${type}`}
              onClick={() => handleButtonClick(value)}
            >
              {value}
            </button>
          ))}

        </div>

        <div className="keyboard-hint">
          KEYBOARD SUPPORTED
        </div>

      </div>

    </div>
  );
}

export default App;
````

App.css

````
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, sans-serif;
  background: #09090d;
}

.app {
  min-height: 100vh;

  display: flex;
  justify-content: center;
  align-items: center;

  padding: 20px;

  background:
    radial-gradient(
      circle at 20% 20%,
      #20202d 0%,
      transparent 30%
    ),
    radial-gradient(
      circle at 80% 80%,
      #17172a 0%,
      transparent 30%
    ),
    #09090d;
}

.calculator {
  width: 370px;

  padding: 22px;

  background: rgba(25, 25, 32, 0.9);

  border: 1px solid rgba(255, 255, 255, 0.08);

  border-radius: 28px;

  box-shadow:
    0 30px 80px rgba(0, 0, 0, 0.6),
    inset 0 1px 1px rgba(255, 255, 255, 0.05);
}

/* HEADER */

.calculator-header {
  display: flex;
  justify-content: space-between;
  align-items: center;

  margin-bottom: 15px;

  color: #777783;

  font-size: 11px;
  font-weight: bold;

  letter-spacing: 2px;
}

.status-dot {
  width: 7px;
  height: 7px;

  border-radius: 50%;

  background: #6ee7b7;

  box-shadow: 0 0 10px #6ee7b7;
}

/* DISPLAY */

.display {
  min-height: 135px;

  display: flex;
  flex-direction: column;

  justify-content: flex-end;
  align-items: flex-end;

  padding: 20px 8px 25px;

  overflow: hidden;
}

.previous-display {
  min-height: 25px;

  color: #686873;

  font-size: 15px;

  margin-bottom: 5px;
}

.current-display {
  width: 100%;

  color: white;

  font-size: 48px;
  font-weight: 300;

  text-align: right;

  white-space: nowrap;

  overflow: hidden;

  text-overflow: ellipsis;
}

/* BUTTON GRID */

.buttons {
  display: grid;

  grid-template-columns: repeat(4, 1fr);

  gap: 11px;
}

/* ALL BUTTONS */

.calc-button {
  height: 68px;

  border: none;

  border-radius: 18px;

  color: white;

  font-size: 20px;
  font-weight: 500;

  cursor: pointer;

  background: #202027;

  transition:
    transform 0.12s ease,
    background 0.15s ease,
    box-shadow 0.15s ease;
}

.calc-button:hover {
  background: #292930;
}

.calc-button:active {
  transform: scale(0.92);
}

/* NUMBERS */

.calc-button.number {
  background: #1b1b22;
}

.calc-button.number:hover {
  background: #27272f;
}

/* OPERATORS */

.calc-button.operator {
  color: #a78bfa;

  background: #25222f;

  font-size: 25px;
}

.calc-button.operator:hover {
  background: #302b3d;
}

/* ACTION BUTTONS */

.calc-button.action {
  color: #b7b7c2;

  background: #292930;

  font-size: 15px;

  font-weight: 600;
}

.calc-button.action:hover {
  background: #34343d;
}

/* EQUALS */

.calc-button.equals {
  color: #09090d;

  background: #a78bfa;

  font-size: 26px;

  font-weight: 700;

  box-shadow:
    0 8px 25px rgba(167, 139, 250, 0.25);
}

.calc-button.equals:hover {
  background: #b49afc;

  box-shadow:
    0 10px 30px rgba(167, 139, 250, 0.35);
}

/* ZERO */

.calc-button.zero {
  grid-column: span 2;
}

/* FOOTER */

.keyboard-hint {
  text-align: center;

  color: #555560;

  font-size: 10px;

  letter-spacing: 1px;

  margin-top: 18px;
}

/* MOBILE */

@media (max-width: 450px) {

  .calculator {
    width: 100%;
    max-width: 370px;

    padding: 18px;
  }

  .calc-button {
    height: 62px;
  }

  .current-display {
    font-size: 40px;
  }
}
````

index.css

````
html,
body,
#root {
  margin: 0;
  min-height: 100%;
}
````

index.js

````
import React from "react";
import ReactDOM from "react-dom/client";

import "./index.css";

import App from "./App.jsx";

const root = ReactDOM.createRoot(
  document.getElementById("root")
);

root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
````

## OUTPUT

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/068eaf02-0d62-466c-b360-cd30e988487c" />


## RESULT
The program for developing a simple calculator in React.js is executed successfully.
