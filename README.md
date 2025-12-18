

<!DOCTYPE html>
ROOMS.forEach(room => {
const btn = document.createElement('button');
btn.textContent = room;
btn.className = 'button' + (room === selectedRoom ? ' selected' : '');
btn.onclick = () => { selectedRoom = room; renderCalendar(); renderSummary(); renderRoomsButtons(); };
roomsButtons.appendChild(btn);
});
}


function generateMonthDays(month) {
const today = new Date();
const lastDayCurrent = new Date(today.getFullYear(), month + 1, 0).getDate();
const days = [];
for (let day = 15; day <= lastDayCurrent; day++) days.push({day, month});
const nextMonth = (month + 1) % 12;
for (let day = 1; day <= 15; day++) days.push({day, month: nextMonth});
return days;
}


function toggleDay(dayObj) {
const key = `${selectedRoom}-${dayObj.month}-${dayObj.day}`;
records[key] = !records[key];
// Save to localStorage so changes persist
localStorage.setItem('calendarRecords', JSON.stringify(records));
renderCalendar();
renderSummary();
}


function totalOutForRoom(room) {
const days = generateMonthDays(monthIndex);
return days.reduce((count, dayObj) => {
const key = `${room}-${dayObj.month}-${dayObj.day}`;
return records[key] ? count + 1 : count;
}, 0);
}


function renderCalendar() {
monthLabel.textContent = `${MONTHS[monthIndex]} - ${MONTHS[(monthIndex+1)%12]}`;
calendarDiv.innerHTML = '';
const days = generateMonthDays(monthIndex);
days.forEach(dayObj => {
const key = `${selectedRoom}-${dayObj.month}-${dayObj.day}`;
const dayDiv = document.createElement('div');
dayDiv.textContent = dayObj.day;
dayDiv.className = 'day' + (records[key] ? ' out' : '');
dayDiv.onclick = () => toggleDay(dayObj);
calendarDiv.appendChild(dayDiv);
});
}


function renderSummary() {
allRoomsSummary.innerHTML = '';
ROOMS.forEach(room => {
const div = document.createElement('div');
div.className = 'room-summary';
div.textContent = `${room}: ${totalOutForRoom(room)}`;
allRoomsSummary.appendChild(div);
});
}


document.getElementById('prev-month').onclick = () => { monthIndex = (monthIndex + 11) % 12; renderCalendar(); renderSummary(); };
document.getElementById('next-month').onclick = () => { monthIndex = (monthIndex + 1) % 12; renderCalendar(); renderSummary(); };


renderRoomsButtons();
renderCalendar();
renderSummary();
</script>
</body>
</html>
