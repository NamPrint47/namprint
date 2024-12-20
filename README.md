# namprint
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NAM PRINT - Print Price Calculator</title>
    <style>
        body {
            font-family: 'Comic Sans MS', 'Comic Sans', cursive;
            background-color: #D0C9B5;
            color: #7f423d;
            margin: 0;
            padding: 0;
        }

        .container {
            max-width: 600px;
            margin: 50px auto;
            padding: 20px;
            background: #fff;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        h1 {
            text-align: center;
            font-weight: bold;
            color: #7f423d;
        }

        label {
            display: block;
            margin: 10px 0 5px;
            font-weight: bold;
        }

        input, select {
            width: 100%;
            padding: 8px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }

        .output {
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 15px;
            color: #7f423d;
        }

        .warning {
            color: red;
            font-weight: bold;
        }

        button {
            background-color: #7f423d;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 1em;
        }

        button:hover {
            background-color: #5a2f26;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>NAM PRINT</h1>
        <form id="printCalculator">
            <label for="numPages">จำนวนหน้า</label>
            <input type="number" id="numPages" min="1" required>

            <label for="paperWeight">ความหนากระดาษ</label>
            <select id="paperWeight" required>
                <option value="70">70 แกรม</option>
                <option value="100">100 แกรม</option>
            </select>

            <label for="color">สี/ขาวดำ</label>
            <select id="color" required>
                <option value="black">ขาวดำ</option>
                <option value="color">สี</option>
            </select>

            <label for="printType">รูปแบบการปริ้น</label>
            <select id="printType" required>
                <option value="single">หน้าเดียว</option>
                <option value="double">หน้าหลัง</option>
            </select>

            <label for="bindingType">รูปแบบการเข้าเล่ม</label>
            <select id="bindingType" required>
                <option value="spiral">สันเกลียว</option>
                <option value="laminate">แลคซีน</option>
                <option value="corner">เย็บมุม</option>
                <option value="none">ปริ้นเฉยๆ</option>
            </select>

            <div class="output" id="result"></div>
            <div class="warning" id="warning"></div>

            <button type="button" onclick="calculatePrice()">คำนวณ</button>
        </form>
    </div>

    <script>
        function calculatePrice() {
            const numPages = parseInt(document.getElementById('numPages').value);
            const paperWeight = document.getElementById('paperWeight').value;
            const color = document.getElementById('color').value;
            const printType = document.getElementById('printType').value;
            const bindingType = document.getElementById('bindingType').value;

            let numSheets = printType === 'double' ? Math.ceil(numPages / 2) : numPages;
            let printPrice = 0;

            // Calculate print price
            if (paperWeight === '70' && color === 'black' && printType === 'single') {
                printPrice = numPages * 0.5;
            } else if (paperWeight === '70' && color === 'black' && printType === 'double') {
                printPrice = numSheets * 0.5;
            } else if (paperWeight === '70' && color === 'color' && printType === 'single') {
                printPrice = numPages * 0.75;
            } else if (paperWeight === '70' && color === 'color' && printType === 'double') {
                printPrice = numSheets * 1.4;
            } else if (paperWeight === '100' && color === 'black' && printType === 'single') {
                printPrice = numPages * 0.7;
            } else if (paperWeight === '100' && color === 'black' && printType === 'double') {
                printPrice = numSheets * 1.3;
            } else if (paperWeight === '100' && color === 'color') {
                printPrice = numPages; // Fixed for 100 แกรม สี
            }

            // Calculate binding price
            let bindingPrice = 0;
            let warningMessage = "";

            if (bindingType === 'spiral') {
                if (numSheets <= 75) bindingPrice = 25;
                else if (numSheets <= 130) bindingPrice = 30;
                else if (numSheets <= 190) bindingPrice = 40;
                else if (numSheets <= 230) bindingPrice = 50;
                else if (numSheets <= 270) bindingPrice = 60;
                else if (numSheets <= 300) bindingPrice = 70;
                else if (numSheets <= 350) bindingPrice = 80;
                else if (numSheets <= 400) bindingPrice = 90;
                else if (numSheets <= 500) bindingPrice = 115;
                else warningMessage = "จำนวนเกิน 500 แผ่น ไม่สามารถเข้าเล่มได้";
            } else if (bindingType === 'laminate' || bindingType === 'corner') {
                if (numSheets > 80) {
                    warningMessage = "จำนวนเกิน 80 แผ่นไม่สามรถเข้าเล่มแลคซีนหรือเย็บมุมได้";
                } else {
                    bindingPrice = 8;
                }
            }

            // Calculate total price
            const totalPrice = printPrice + bindingPrice;

            // Display results
            document.getElementById('result').textContent = `ราคาสุทธิ: ${totalPrice.toFixed(2)} บาท`;
            document.getElementById('warning').textContent = warningMessage;
        }
    </script>
</body>
</html>
