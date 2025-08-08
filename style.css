const taskInput = document.getElementById("taskInput");
const taskList = document.getElementById("taskList");
const calendarSection = document.getElementById("calendarSection");
const monthSelect = document.getElementById("monthSelect");
const yearSelect = document.getElementById("yearSelect");
const calendar = document.getElementById("calendar");
const selectedTaskTitle = document.getElementById("selectedTaskTitle");
const summary = document.getElementById("summary");

let currentTask = null;
let allTasks = JSON.parse(localStorage.getItem("tasks")) || [];
let attendanceData = JSON.parse(localStorage.getItem("attendance")) || {};

function saveData() {
  localStorage.setItem("tasks", JSON.stringify(allTasks));
  localStorage.setItem("attendance", JSON.stringify(attendanceData));
}

function addTask() {
  const task = taskInput.value.trim();
  if (!task || allTasks.includes(task)) return;
  allTasks.push(task);
  attendanceData[task] = {};
  taskInput.value = "";
  saveData();
  renderTasks();
}

function deleteTask(task) {
  const confirmDelete = confirm(`Are you sure you want to delete the task "${task}"?`);
  if (!confirmDelete) return;

  allTasks = allTasks.filter(t => t !== task);
  delete attendanceData[task];
  if (currentTask === task) calendarSection.classList.add("hidden");
  saveData();
  renderTasks();
}

function renderTasks() {
  taskList.innerHTML = "";
  allTasks.forEach(task => {
    const div = document.createElement("div");
    div.className = "task";
    div.innerHTML = `
      <span>${task}</span>
      <div>
        <button onclick="selectTask('${task}')">📅</button>
        <button onclick="deleteTask('${task}')">🗑</button>
      </div>
    `;
    taskList.appendChild(div);
  });
}

function selectTask(task) {
  currentTask = task;
  selectedTaskTitle.textContent = `📌 ${task}`;
  calendarSection.classList.remove("hidden");
  renderCalendar();
}

function renderCalendar() {
  const selectedMonth = parseInt(monthSelect.value);
  const selectedYear = parseInt(yearSelect.value);
  const daysInMonth = new Date(selectedYear, selectedMonth + 1, 0).getDate();
  calendar.innerHTML = "";

  // Weekday headers
  const weekdays = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
  weekdays.forEach(w => {
    const hd = document.createElement("div");
    hd.className = "weekday";
    hd.textContent = w;
    calendar.appendChild(hd);
  });

  // Compute which weekday the 1st of month falls on
  const firstDay = new Date(selectedYear, selectedMonth, 1).getDay();

  // Add leading empty slots so dates fall under correct weekday
  for (let i = 0; i < firstDay; i++) {
    const blank = document.createElement("div");
    blank.className = "empty";
    calendar.appendChild(blank);
  }

  const taskData = attendanceData[currentTask] || {};
  let present = 0, absent = 0;

  for (let day = 1; day <= daysInMonth; day++) {
    const dateKey = `${selectedYear}-${selectedMonth}-${day}`;
    if (!(dateKey in taskData)) {
      taskData[dateKey] = true; // default to present
    }

    const isPresent = taskData[dateKey];
    const div = document.createElement("div");
    div.className = "dateCell";
    div.textContent = day;
    if (!isPresent) div.classList.add("absent");

    div.onclick = () => {
      taskData[dateKey] = !taskData[dateKey];
      attendanceData[currentTask] = taskData;
      saveData();
      renderCalendar();
    };

    calendar.appendChild(div);
    isPresent ? present++ : absent++;
  }

  summary.textContent = `✅ Present: ${present} | ❌ Absent: ${absent}`;
  attendanceData[currentTask] = taskData;
  saveData();
}

function populateMonthSelect() {
  const months = [
    "January", "February", "March", "April", "May", "June",
    "July", "August", "September", "October", "November", "December"
  ];
  months.forEach((m, i) => {
    const option = document.createElement("option");
    option.value = i;
    option.textContent = m;
    monthSelect.appendChild(option);
  });
  monthSelect.value = new Date().getMonth();
}

function populateYearSelect() {
  const currentYear = new Date().getFullYear();
  for (let y = currentYear - 5; y <= currentYear + 5; y++) {
    const option = document.createElement("option");
    option.value = y;
    option.textContent = y;
    if (y === currentYear) option.selected = true;
    yearSelect.appendChild(option);
  }
}

monthSelect.onchange = renderCalendar;
yearSelect.onchange = renderCalendar;

document.addEventListener("DOMContentLoaded", () => {
  populateMonthSelect();
  populateYearSelect();
  renderTasks();
});
