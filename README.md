<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Installments</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f3f4f6;
            direction: rtl;
        }
        .calculator {
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            width: 450px;
        }
        .calculator h2 {
            margin-bottom: 20px;
            font-size: 24px;
            color: #333;
            text-align: center;
        }
        .form-group {
            margin-bottom: 15px;
        }
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-size: 14px;
            color: #333;
        }
        .form-group input {
            width: 100%;
            padding: 8px;
            font-size: 16px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        .result {
            margin-top: 20px;
            font-size: 18px;
            font-weight: bold;
            text-align: center;
            color: #333;
        }
        table {
            width: 100%;
            margin-top: 20px;
            border-collapse: collapse;
        }
        table, th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: center;
        }
        th {
            background-color: #f0f0f0;
        }
    </style>
</head>
<body>

<div class="calculator">
    <h2>B Auto </h2>
    <h3 style="text-align: center ;">Installments Calculator </h3>
    <div class="form-group">
        <label for="totalAmount">المبلغ الكلي (جنيه)</label>
        <input type="number" id="totalAmount" oninput="calculateAll()">
    </div>
    <div class="form-group">
        <label for="downPaymentRate">نسبة المقدم (%)</label>
        <input type="number" id="downPaymentRate" oninput="calculateAll()">
    </div>
    <div class="result" id="downPaymentResult">المقدم: -</div>
    <div class="form-group">
        <label for="financeAmount">مبلغ التمويل بعد المقدم (جنيه)</label>
        <input type="number" id="financeAmount" readonly>
    </div>
    <div class="form-group">
        <label for="interestRate">الفائدة (%)</label>
        <input type="number" id="interestRate" oninput="calculateAll()">
    </div>
    <div class="result" id="totalInterestResult">مجموع الفوائد على مدى السنوات: -</div>
    <div class="result" id="adminFeeResult">المصاريف الإدارية: -</div>
    <div class="result" id="insuranceResult">التأمين: -</div>

    <h3>جدول الأقساط حسب السنوات</h3>
    <table>
        <thead>
            <tr>
                <th>السنوات</th>
                <th>القسط الشهري (جنيه)</th>
                <th>مبلغ الفائدة الإضافية (جنيه)</th>
            </tr>
        </thead>
        <tbody id="installmentTable">
            <!-- سيتم ملء الجدول هنا -->
        </tbody>
    </table>

    <h4 style="text-align: center; margin-top: 20px; color: darkblue;text-decoration: underline">Designed by Ahmed Shaaban</h4>
</div>

<script>
    function calculateMonthlyInstallment(financeAmount, interestRate, years) {
        const totalInterest = financeAmount * (interestRate / 100) * years;
        const totalAmount = financeAmount + totalInterest;
        const monthlyInstallment = totalAmount / (years * 12);
        return { monthlyInstallment, totalInterest };
    }

    function calculateAll() {
        const totalAmount = parseFloat(document.getElementById("totalAmount").value) || 0;
        const downPaymentRate = parseFloat(document.getElementById("downPaymentRate").value) || 0;
        const interestRate = parseFloat(document.getElementById("interestRate").value) || 0;

        // حساب المقدم وتحديث مبلغ التمويل بعد المقدم
        const downPayment = totalAmount * (downPaymentRate / 100);
        const financeAmount = totalAmount - downPayment;
        document.getElementById("downPaymentResult").innerText = "المقدم: " + downPayment.toFixed(2) + " جنيه";
        document.getElementById("financeAmount").value = financeAmount.toFixed(2);

        // حساب مجموع الفوائد على مدى السنوات
        const totalInterestOverYears = financeAmount * (interestRate / 100) * 5;  // يفترض أن عدد السنوات 5 للعرض
        document.getElementById("totalInterestResult").innerText = "مجموع الفوائد على مدى السنوات: " + totalInterestOverYears.toFixed(2) + " جنيه";

        // حساب المصاريف الإدارية 2.5% من مبلغ التمويل
        const adminFee = financeAmount * 0.025;
        document.getElementById("adminFeeResult").innerText = "المصاريف الإدارية: " + adminFee.toFixed(2) + " جنيه";

        // حساب التأمين 2% من المبلغ الكلي
        const insurance = totalAmount * 0.02;
        document.getElementById("insuranceResult").innerText = "التأمين: " + insurance.toFixed(2) + " جنيه";

        // حساب الأقساط الشهرية والفائدة الإضافية لكل سنة من 1 إلى 5 سنوات
        let tableContent = '';
        for (let years = 1; years <= 5; years++) {
            const { monthlyInstallment, totalInterest } = calculateMonthlyInstallment(financeAmount, interestRate, years);
            tableContent += `
                <tr>
                    <td>${years}</td>
                    <td>${monthlyInstallment.toFixed(2)}</td>
                    <td>${totalInterest.toFixed(2)}</td>
                </tr>
            `;
        }
        document.getElementById("installmentTable").innerHTML = tableContent;
    }
</script>

</body>
</html>
