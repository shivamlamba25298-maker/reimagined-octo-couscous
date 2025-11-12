// Deterministic daily number generator (same for everyone for a given date).
// Works without server: seed = date string "YYYY-MM-DD" -> hashed -> number 0-99.

function dateToYYYYMMDD(d) {
  const y = d.getFullYear();
  const m = String(d.getMonth() + 1).padStart(2, '0');
  const day = String(d.getDate()).padStart(2, '0');
  return `${y}-${m}-${day}`;
}

// simple hash function producing integer from string (deterministic)
function simpleHash(str) {
  // djb2-like but safe small-int
  let hash = 5381;
  for (let i = 0; i < str.length; i++) {
    hash = ((hash << 5) + hash) + str.charCodeAt(i); /* hash * 33 + c */
    hash = hash & 0xffffffff; // keep 32-bit
  }
  return Math.abs(hash);
}

// get result number 0-99 (you can change range if you like)
function getResultForDateString(dateStr) {
  const h = simpleHash(dateStr);
  return (h % 100); // 0-99
}

// format number to 2 digits (like 05)
function twoDigits(n) {
  return String(n).padStart(2, '0');
}

// UI bindings
const todayDateEl = document.getElementById('todayDate');
const todayResultEl = document.getElementById('todayResult');
const refreshBtn = document.getElementById('refreshBtn');
const pickDateEl = document.getElementById('pickDate');
const checkBtn = document.getElementById('checkBtn');
const pickedResultEl = document.getElementById('pickedResult');
const last7El = document.getElementById('last7');

function showToday() {
  const today = new Date();
  const dateStr = dateToYYYYMMDD(today);
  todayDateEl.textContent = dateStr;
  const num = getResultForDateString(dateStr);
  todayResultEl.textContent = twoDigits(num);
}

// check for picked date
checkBtn.addEventListener('click', () => {
  const val = pickDateEl.value;
  if (!val) {
    pickedResultEl.textContent = 'Please choose a date.';
    return;
  }
  const num = getResultForDateString(val);
  pickedResultEl.textContent = `${val} → ${twoDigits(num)}`;
});

// refresh (recompute)
refreshBtn.addEventListener('click', showToday);

// populate last 7 days
function showLast7() {
  last7El.innerHTML = '';
  const today = new Date();
  for (let i = 0; i < 7; i++) {
    const d = new Date(today);
    d.setDate(today.getDate() - i);
    const ds = dateToYYYYMMDD(d);
    const n = getResultForDateString(ds);
    const li = document.createElement('li');
    li.textContent = `${ds} → ${twoDigits(n)}`;
    last7El.appendChild(li);
  }
}

// initial
showToday();
showLast7();

// optional: set date input default to today
pickDateEl.value = dateToYYYYMMDD(new Date());
