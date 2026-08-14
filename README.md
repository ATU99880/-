<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Realistic Prank Calculator</title>

<style>
* {
    box-sizing: border-box;
}

body {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    margin: 0;

    background:
        radial-gradient(circle at top, #eef2f7, #b8c2cc 70%);

    font-family: "Segoe UI", Tahoma, sans-serif;
    user-select: none;
}

/* جسم الآلة */
.calculator {
    width: min(350px, 92vw);
    padding: 22px;

    border-radius: 25px;

    background:
        linear-gradient(145deg, #3b424b, #252a30);

    box-shadow:
        0 25px 45px rgba(0,0,0,.45),
        inset 0 2px 2px rgba(255,255,255,.12),
        inset 0 -4px 8px rgba(0,0,0,.35);
}

/* الشاشة */
.display {
    width: 100%;
    height: 78px;

    display: flex;
    align-items: center;
    justify-content: flex-end;

    padding: 10px 14px;

    background:
        linear-gradient(180deg, #8b969e, #68737c);

    color: #172027;

    border-radius: 12px;

    /*
      خط 7-Segment
      نستخدم system monospace كخيار احتياطي
      إذا لم يتوفر الخط
    */
    font-family:
        "DS-Digital",
        "Seven Segment",
        "Digital-7",
        "Courier New",
        monospace;

    font-size: 2.35em;
    font-weight: bold;

    letter-spacing: 2px;

    overflow: hidden;
    white-space: nowrap;

    box-shadow:
        inset 0 6px 12px rgba(0,0,0,.45),
        inset 0 -2px 3px rgba(255,255,255,.12);

    text-shadow:
        0 1px 1px rgba(255,255,255,.18);

    margin-bottom: 22px;
}

/* شبكة الأزرار */
.buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 12px;
}

/* الأزرار */
button {
    position: relative;

    height: 65px;

    border: none;
    border-radius: 12px;

    font-size: 1.35em;
    font-weight: 700;

    cursor: pointer;

    transition:
        transform .08s ease,
        box-shadow .08s ease,
        filter .08s ease;

    box-shadow:
        0 6px 0 #171b1f,
        0 9px 12px rgba(0,0,0,.28);

    overflow: hidden;
}

/* لمعة */
button::before {
    content: "";

    position: absolute;

    top: 0;
    left: 0;
    right: 0;

    height: 45%;

    border-radius: 12px 12px 50% 50%;

    background:
        linear-gradient(
            to bottom,
            rgba(255,255,255,.28),
            rgba(255,255,255,0)
        );

    pointer-events: none;
}

/* الضغط */
button:active {
    transform: translateY(5px) scale(.97);

    box-shadow:
        0 1px 0 #171b1f,
        0 3px 6px rgba(0,0,0,.25);

    filter: brightness(.9);
}

/* الأرقام */
.number {
    background:
        linear-gradient(145deg, #ffffff, #dfe3e8);

    color: #252b31;
}

/* العمليات */
.operator {
    background:
        linear-gradient(145deg, #f2c94c, #d49b12);

    color: white;

    text-shadow:
        0 2px 2px rgba(0,0,0,.3);
}

/* زر C */
.clear {
    background:
        linear-gradient(145deg, #ff5b38, #c92d0b);

    color: white;

    text-shadow:
        0 2px 2px rgba(0,0,0,.3);
}

/* زر الحذف */
.delete {
    background:
        linear-gradient(145deg, #ff914d, #d85b1c);

    color: white;

    text-shadow:
        0 2px 2px rgba(0,0,0,.3);
}

/* زر = */
.equal {
    grid-column: span 2;

    background:
        linear-gradient(145deg, #61e653, #32a928);

    color: white;

    text-shadow:
        0 2px 2px rgba(0,0,0,.3);
}

/* حركة العطل */
@keyframes shake {
    0%   { transform: translateX(0); }
    15%  { transform: translateX(-5px) rotate(-1deg); }
    30%  { transform: translateX(5px) rotate(1deg); }
    45%  { transform: translateX(-4px) rotate(-1deg); }
    60%  { transform: translateX(4px) rotate(1deg); }
    75%  { transform: translateX(-2px); }
    100% { transform: translateX(0); }
}

.malfunction {
    animation: shake .45s ease;
}

/* للجوال */
@media (max-width: 380px) {

    .calculator {
        padding: 17px;
    }

    .buttons {
        gap: 9px;
    }

    button {
        height: 60px;
        font-size: 1.25em;
    }

    .display {
        height: 70px;
        font-size: 2em;
    }
}
</style>
</head>

<body>

<div class="calculator">

    <div class="display" id="display">0</div>

    <div class="buttons">

        <!-- C -->
        <button class="clear"
                onclick="buttonSound(); clearDisplay()">
            C
        </button>

        <!-- حذف رقم -->
        <button class="delete"
                onclick="buttonSound(); deleteNumber()">
            ⌫
        </button>

        <!-- قسمة -->
        <button class="operator"
                onclick="buttonSound(); appendOperator('/')">
            ÷
        </button>

        <!-- ضرب -->
        <button class="operator"
                onclick="buttonSound(); appendOperator('*')">
            ×
        </button>


        <!-- 7 8 9 - -->
        <button class="number"
                onclick="buttonSound(); appendNumber('7')">
            7
        </button>

        <button class="number"
                onclick="buttonSound(); appendNumber('8')">
            8
        </button>

        <button class="number"
                onclick="buttonSound(); appendNumber('9')">
            9
        </button>

        <button class="operator"
                onclick="buttonSound(); appendOperator('-')">
            −
        </button>


        <!-- 4 5 6 + -->
        <button class="number"
                onclick="buttonSound(); appendNumber('4')">
            4
        </button>

        <button class="number"
                onclick="buttonSound(); appendNumber('5')">
            5
        </button>

        <button class="number"
                onclick="buttonSound(); appendNumber('6')">
            6
        </button>

        <button class="operator"
                onclick="buttonSound(); appendOperator('+')">
            +
        </button>


        <!-- 1 2 3 . -->
        <button class="number"
                onclick="buttonSound(); appendNumber('1')">
            1
        </button>

        <button class="number"
                onclick="buttonSound(); appendNumber('2')">
            2
        </button>

        <button class="number"
                onclick="buttonSound(); appendNumber('3')">
            3
        </button>

        <button class="number"
                onclick="buttonSound(); appendNumber('.')">
            .
        </button>


        <!-- 0 و = -->
        <button class="number"
                onclick="buttonSound(); appendNumber('0')">
            0
        </button>

        <button class="equal"
                onclick="equalSound(); calculate()">
            =
        </button>

    </div>
</div>


<script>

let screenText = '0';
let realEquation = '';
let isResultDisplayed = false;

const displayElement =
    document.getElementById('display');


/* =========================
   نظام الصوت
========================= */

let audioContext;

function createAudio() {

    if (!audioContext) {

        audioContext =
            new (window.AudioContext ||
                 window.webkitAudioContext)();
    }

    if (audioContext.state === "suspended") {
        audioContext.resume();
    }
}


/* صوت الأزرار */
function buttonSound() {

    createAudio();

    const oscillator =
        audioContext.createOscillator();

    const gain =
        audioContext.createGain();

    oscillator.type = "square";

    oscillator.frequency.setValueAtTime(
        180,
        audioContext.currentTime
    );

    oscillator.frequency.exponentialRampToValueAtTime(
        90,
        audioContext.currentTime + 0.045
    );

    gain.gain.setValueAtTime(
        0.08,
        audioContext.currentTime
    );

    gain.gain.exponentialRampToValueAtTime(
        0.001,
        audioContext.currentTime + 0.06
    );

    oscillator.connect(gain);
    gain.connect(audioContext.destination);

    oscillator.start();

    oscillator.stop(
        audioContext.currentTime + 0.06
    );
}


/* صوت = */
function equalSound() {

    createAudio();

    const oscillator =
        audioContext.createOscillator();

    const gain =
        audioContext.createGain();

    oscillator.type = "sine";

    oscillator.frequency.setValueAtTime(
        400,
        audioContext.currentTime
    );

    oscillator.frequency.exponentialRampToValueAtTime(
        650,
        audioContext.currentTime + 0.12
    );

    gain.gain.setValueAtTime(
        0.12,
        audioContext.currentTime
    );

    gain.gain.exponentialRampToValueAtTime(
        0.001,
        audioContext.currentTime + 0.16
    );

    oscillator.connect(gain);
    gain.connect(audioContext.destination);

    oscillator.start();

    oscillator.stop(
        audioContext.currentTime + 0.16
    );
}


/* صوت العطل 😈 */
function malfunctionSound() {

    createAudio();

    const oscillator =
        audioContext.createOscillator();

    const gain =
        audioContext.createGain();

    oscillator.type = "sawtooth";

    oscillator.frequency.setValueAtTime(
        180,
        audioContext.currentTime
    );

    oscillator.frequency.exponentialRampToValueAtTime(
        45,
        audioContext.currentTime + 0.5
    );

    gain.gain.setValueAtTime(
        0.16,
        audioContext.currentTime
    );

    gain.gain.exponentialRampToValueAtTime(
        0.001,
        audioContext.currentTime + 0.55
    );

    oscillator.connect(gain);
    gain.connect(audioContext.destination);

    oscillator.start();

    oscillator.stop(
        audioContext.currentTime + 0.55
    );
}


/* =========================
   الشاشة
========================= */

function updateDisplay() {

    displayElement.innerText =
        screenText;
}


/* =========================
   إدخال الأرقام
========================= */

function appendNumber(number) {

    if (
        screenText === '0' ||
        screenText === 'Calculator Malfunction' ||
        screenText === 'Error' ||
        isResultDisplayed
    ) {

        screenText = number;

        realEquation = number;

        isResultDisplayed = false;

        displayElement.style.fontSize =
            '2.35em';

    } else {

        screenText += number;

        realEquation += number;
    }

    updateDisplay();
}


/* =========================
   العمليات
========================= */

function appendOperator(operator) {

    if (
        screenText === 'Calculator Malfunction' ||
        screenText === 'Error'
    ) {
        return;
    }

    if (isResultDisplayed) {

        realEquation =
            '67' + operator;

        screenText =
            '67' +
            getDisplayOperator(operator);

        isResultDisplayed = false;

    } else {

        /*
         * منع وضع عمليتين وراء بعض
         */
        if (
            realEquation.endsWith('+') ||
            realEquation.endsWith('-') ||
            realEquation.endsWith('*') ||
            realEquation.endsWith('/')
        ) {
            return;
        }

        realEquation += operator;

        screenText +=
            getDisplayOperator(operator);
    }

    updateDisplay();
}


/* شكل العمليات */
function getDisplayOperator(operator) {

    if (operator === '*')
        return '×';

    if (operator === '/')
        return '÷';

    if (operator === '-')
        return '−';

    return operator;
}


/* =========================
   حذف آخر رقم / علامة
========================= */

function deleteNumber() {

    if (
        screenText === 'Calculator Malfunction' ||
        screenText === 'Error' ||
        isResultDisplayed
    ) {
        return;
    }

    if (
        screenText.length <= 1 ||
        realEquation.length <= 1
    ) {

        screenText = '0';
        realEquation = '';

    } else {

        /*
         * نحذف من المعادلة الحقيقية
         */
        realEquation =
            realEquation.slice(0, -1);

        /*
         * نحذف من الشاشة
         */
        screenText =
            screenText.slice(0, -1);
    }

    updateDisplay();
}


/* =========================
   مسح الكل
========================= */

function clearDisplay() {

    screenText = '0';

    realEquation = '';

    isResultDisplayed = false;

    displayElement.style.fontSize =
        '2.35em';

    displayElement.classList.remove(
        'malfunction'
    );

    updateDisplay();
}


/* =========================
   الحساب والمقلب
========================= */

function calculate() {

    if (
        screenText === 'Calculator Malfunction' ||
        screenText === 'Error' ||
        realEquation === ''
    ) {
        return;
    }


    /*
     * لا نحسب إذا انتهت المعادلة
     * بعلامة عملية
     */
    if (
        realEquation.endsWith('+') ||
        realEquation.endsWith('-') ||
        realEquation.endsWith('*') ||
        realEquation.endsWith('/')
    ) {
        return;
    }


    let realResult;


    try {

        realResult =
            eval(realEquation);

    } catch (error) {

        screenText = 'Error';

        updateDisplay();

        return;
    }


    /*
     * إذا كانت النتيجة الحقيقية 67
     */
    if (realResult === 67) {

        screenText =
            'Calculator Malfunction';

        displayElement.style.fontSize =
            '1.05em';

        displayElement.classList.remove(
            'malfunction'
        );

        void displayElement.offsetWidth;

        displayElement.classList.add(
            'malfunction'
        );

        malfunctionSound();

    }

    /*
     * أي نتيجة ثانية تظهر 67
     */
    else {

        screenText = '67';

        displayElement.style.fontSize =
            '2.35em';

        isResultDisplayed = true;
    }


    updateDisplay();
}

</script>

</body>
</html>
