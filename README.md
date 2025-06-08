html>
<html lang="he" dir="rtl">
<head>
<meta charset="UTF-8" />
<title>קביעת תור בקופת חולים</title>
<style>
  body { font-family: Arial, sans-serif; max-width: 600px; margin: 20px auto; }
  label, select, input, button { display: block; margin: 10px 0; width: 100%; }
  button { padding: 10px; }
  #slots { margin-top: 20px; }
  .slot { padding: 8px; border: 1px solid #ccc; margin-bottom: 5px; cursor: pointer; }
  .slot.selected { background-color: #4caf50; color: white; }
</style>
</head>
<body>

<h1>קביעת תור בקופת חולים</h1>

<label for="kupotSelect">בחר קופת חולים:</label>
<select id="kupotSelect">
  <option value="">--בחר--</option>
  <option value="clalit">כללית</option>
  <option value="maccabi">מכבי</option>
  <option value="leumit">לאומית</option>
  <option value=" Meuhedet">מאוחדת</option>
</select>

<div id="slots" style="display:none;">
  <h2>שעות פנויות</h2>
  <div id="slotsList"></div>

  <label for="idNumber">מספר זהות:</label>
  <input type="text" id="idNumber" placeholder="הכנס מספר זהות" />

  <button id="bookBtn" disabled>קבע תור</button>
</div>

<div id="confirmation" style="margin-top:20px; font-weight:bold; color:green;"></div>

<script>
  const kupotSlots = {
    clalit: ["09:00", "09:30", "10:00", "10:30"],
    maccabi: ["11:00", "11:30", "12:00"],
    leumit: ["08:00", "08:30", "09:00"],
    Meuhedet: ["13:00", "13:30", "14:00"]
  };

  const kupotSelect = document.getElementById('kupotSelect');
  const slotsDiv = document.getElementById('slots');
  const slotsList = document.getElementById('slotsList');
  const bookBtn = document.getElementById('bookBtn');
  const idNumberInput = document.getElementById('idNumber');
  const confirmationDiv = document.getElementById('confirmation');

  let selectedSlot = null;

  kupotSelect.addEventListener('change', () => {
    confirmationDiv.textContent = '';
    selectedSlot = null;
    bookBtn.disabled = true;
    idNumberInput.value = '';
    slotsList.innerHTML = '';
    if (kupotSelect.value) {
      slotsDiv.style.display = 'block';
      kupotSlots[kupotSelect.value].forEach(slot => {
        const div = document.createElement('div');
        div.textContent = slot;
        div.className = 'slot';
        div.addEventListener('click', () => {
          document.querySelectorAll('.slot').forEach(s => s.classList.remove('selected'));
          div.classList.add('selected');
          selectedSlot = slot;
          checkEnableBook();
        });
        slotsList.appendChild(div);
      });
    } else {
      slotsDiv.style.display = 'none';
    }
  });

  idNumberInput.addEventListener('input', checkEnableBook);

  function checkEnableBook() {
    const id = idNumberInput.value.trim();
    bookBtn.disabled = !(selectedSlot && id.length >= 5); // בדיקה פשוטה למספר זהות
  }

  bookBtn.addEventListener('click', () => {
    const id = idNumberInput.value.trim();
    if (!selectedSlot || !id) return;
    // כאן אפשר להוסיף קריאת API אמיתית
    confirmationDiv.textContent = `תורך נקבע בהצלחה בקופת חולים ${kupotSelect.options[kupotSelect.selectedIndex].text} בשעה ${selectedSlot} עבור ת.ז. ${id}`;
    slotsDiv.style.display = 'none';
    kupotSelect.value = '';
  });
</script>

</body>
</html>
