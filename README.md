<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>工程用計算機</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin: 50px; }
        .calculator { display: inline-block; padding: 20px; border: 1px solid #ccc; border-radius: 10px; }
        input, button { font-size: 20px; margin: 5px; }
        .row { display: flex; justify-content: center; }
    </style>
</head>
<body>
    <div class="calculator">
        <h2>工程用計算機</h2>
        <input type="text" id="display" disabled>
        <br>
        <div class="row">
            <button onclick="clearDisplay()">C</button>
            <button onclick="appendToDisplay('(')">(</button>
            <button onclick="appendToDisplay(')')">)</button>
            <button onclick="appendToDisplay('/')">/</button>
        </div>
        <div class="row">
            <button onclick="appendToDisplay('7')">7</button>
            <button onclick="appendToDisplay('8')">8</button>
            <button onclick="appendToDisplay('9')">9</button>
            <button onclick="appendToDisplay('*')">*</button>
        </div>
        <div class="row">
            <button onclick="appendToDisplay('4')">4</button>
            <button onclick="appendToDisplay('5')">5</button>
            <button onclick="appendToDisplay('6')">6</button>
            <button onclick="appendToDisplay('-')">-</button>
        </div>
        <div class="row">
            <button onclick="appendToDisplay('1')">1</button>
            <button onclick="appendToDisplay('2')">2</button>
            <button onclick="appendToDisplay('3')">3</button>
            <button onclick="appendToDisplay('+')">+</button>
        </div>
        <div class="row">
            <button onclick="appendToDisplay('0')">0</button>
            <button onclick="appendToDisplay('.')">.</button>
            <button onclick="calculate()">=</button>
            <button onclick="appendToDisplay('Math.sqrt(')">√</button>
        </div>
        <div class="row">
            <button onclick="appendToDisplay('Math.pow(')">xʸ</button>
            <button onclick="appendToDisplay('Math.sin(')">sin</button>
            <button onclick="appendToDisplay('Math.cos(')">cos</button>
            <button onclick="appendToDisplay('Math.tan(')">tan</button>
        </div>
    </div>
    
    <script>
        function appendToDisplay(value) {
            document.getElementById('display').value += value;
        }
        function clearDisplay() {
            document.getElementById('display').value = '';
        }
        function calculate() {
            try {
                document.getElementById('display').value = eval(document.getElementById('display').value);
            } catch {
                document.getElementById('display').value = '錯誤';
            }
        }
    </script>
</body>
</html>
