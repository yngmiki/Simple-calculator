<!DOCTYPE html>
<html>
<head>
  <title>Simple Calculator</title>
  <style>
    body {
      font-family: sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      background-color: #f0f0f0;
    }

    .calculator {
      border: 1px solid #ccc;
      border-radius: 5px;
      width: 300px;
      background-color: #fff;
      padding: 10px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }

    .calculator-screen {
      width: 100%;
      height: 60px;
      border: none;
      background-color: #eee;
      text-align: right;
      padding-right: 10px;
      padding-left: 10px;
      font-size: 2rem;
      border-radius: 5px;
      margin-bottom: 10px;
      box-sizing: border-box; /* Important for padding to not affect width */
    }

    .calculator-keys {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      grid-gap: 10px;
    }

    button {
      height: 50px;
      background-color: #f2f2f2;
      border: 1px solid #ccc;
      font-size: 1.5rem;
      border-radius: 5px;
      cursor: pointer;
      transition: background-color 0.2s ease;
    }

    button:hover {
      background-color: #e0e0e0;
    }

    .operator {
      background-color: #ddd;
    }

    .operator:hover {
      background-color: #ccc;
    }

    .equal-sign {
      grid-column: 3 / 5; /* Spans two columns */
      background-color: #4CAF50; /* Green */
      color: white;
    }

    .equal-sign:hover {
      background-color: #3e8e41;
    }

    .all-clear {
      background-color: #f44336; /* Red */
      color: white;
    }

    .all-clear:hover {
      background-color: #d32f2f;
    }
  </style>
</head>
<body>

  <div class="calculator">
    <input type="text" class="calculator-screen" value="0" disabled />
    <div class="calculator-keys">
      <button class="operator" value="+">+</button>
      <button class="operator" value="-">-</button>
      <button class="operator" value="*">&times;</button>
      <button class="operator" value="/">&divide;</button>

      <button value="7">7</button>
      <button value="8">8</button>
      <button value="9">9</button>
      <button class="all-clear">AC</button>

      <button value="4">4</button>
      <button value="5">5</button>
      <button value="6">6</button>

      <button value="1">1</button>
      <button value="2">2</button>
      <button value="3">3</button>

      <button value="0">0</button>
            <button class="decimal" value=".">.</button>
      <button class="equal-sign" value="=">=</button>
    </div>
  </div>

  <script>
    const calculatorScreen = document.querySelector('.calculator-screen');
    const keys = document.querySelector('.calculator-keys');

    let displayValue = '0';
    let firstValue = null;
    let operator = null;
    let waitingForSecondValue = false;

    updateDisplay(); // Initialize the display

    function updateDisplay() {
      calculatorScreen.value = displayValue;
    }

    keys.addEventListener('click', function(e) {
      const element = e.target;

      if (!element.matches('button')) return;

      if (element.classList.contains('operator')) {
        handleOperator(element.value);
        updateDisplay();
        return;
      }

      if (element.classList.contains('all-clear')) {
        clear();
        updateDisplay();
        return;
      }

      if (element.classList.contains('equal-sign')) {
        calculate();
        updateDisplay();
        return;
      }

      if (element.classList.contains('decimal')) {
        inputDecimal(element.value);
        updateDisplay();
        return;
      }

      inputNumber(element.value);
      updateDisplay();
    });

    function inputNumber(num) {
      if (waitingForSecondValue === true) {
        displayValue = num;
        waitingForSecondValue = false;
      } else {
        displayValue = displayValue === '0' ? num : displayValue + num;
      }
    }

    function inputDecimal(dot) {
      if (!displayValue.includes(dot)) {
        displayValue += dot;
      }
    }

    function handleOperator(nextOperator) {
      const value = parseFloat(displayValue);

      if (firstValue === null) {
        firstValue = value;
      } else if (operator) {
        const result = operate(firstValue, value, operator);

        displayValue = String(result);
        firstValue = result;
      }

      waitingForSecondValue = true;
      operator = nextOperator;
    }

    function calculate() {
      const secondValue = parseFloat(displayValue)

      if(operator) {
        const result = operate(firstValue, secondValue, operator);
        displayValue = String(result);
        firstValue = result;
        operator = null;
        waitingForSecondValue = false;
      }
    }

    function operate(num1, num2, oper) {
      if (oper === '+') {
        return num1 + num2;
      } else if (oper === '-') {
        return num1 - num2;
      } else if (oper === '*') {
        return num1 * num2;
      } else if (oper === '/') {
        return num1 / num2;
      }

      return num2; // Default return if operator is not recognized.
    }

    function clear() {
      displayValue = '0';
      firstValue = null;
      operator = null;
      waitingForSecondValue = false;
    }
  </script>

</body>
</html>
