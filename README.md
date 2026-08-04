<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pink Panther - Gestione Turni Personale</title>
    <style>
        :root {
            --bg-color: #050505;
            --card-bg: #121212;
            --card-border: #222222;
            --accent-pink: #e65c9c;
            --text-main: #ffffff;
            --text-secondary: #999999;
            --green-wa: #25d366;
            --red-riposo: #ff4d4d;
            --table-header-bg: #1f1418;
            --table-border: #2a2a2a;
        }

        * {
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            margin: 0;
            padding: 16px;
            max-width: 900px;
            margin-left: auto;
            margin-right: auto;
        }

        /* HEADER E LOGO PINK PANTHER */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0 20px 0;
            border-bottom: 1px solid #1a1a1a;
            margin-bottom: 20px;
        }

        .logo-box {
            background-color: #000000;
            padding: 12px 16px;
            border-radius: 8px;
            display: flex;
            flex-direction: column;
            line-height: 0.85;
            font-family: 'Arial Black', Impact, sans-serif;
            text-transform: uppercase;
            user-select: none;
            box-shadow: 0 4px 10px rgba(0,0,0,0.5);
        }

        .logo-row {
            font-size: 24px;
            font-weight: 900;
            letter-spacing: 2px;
            color: #ffffff;
            display: flex;
        }

        .pink-p {
            background: linear-gradient(90deg, var(--accent-pink) 45%, #ffffff 45%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .pink-k {
            background: linear-gradient(135deg, #ffffff 55%, var(--accent-pink) 55%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .pink-a, .pink-h {
            background: linear-gradient(180deg, #ffffff 40%, var(--accent-pink) 40%, var(--accent-pink) 65%, #ffffff 65%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .pink-r {
            background: linear-gradient(135deg, #ffffff 60%, var(--accent-pink) 60%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .toggle-btn {
            background: rgba(230, 92, 156, 0.15);
            border: 1px solid var(--accent-pink);
            color: var(--accent-pink);
            padding: 10px 14px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 800;
            cursor: pointer;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        /* BARRA FILTRI */
        .card {
            background-color: var(--card-bg);
            border: 1px solid var(--card-border);
            border-radius: 16px;
            padding: 16px;
            margin-bottom: 20px;
        }

        .filter-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
            gap: 12px;
            align-items: center;
        }

        .form-group {
            display: flex;
            flex-direction: column;
        }

        .form-group label {
            font-size: 11px;
            color: var(--text-secondary);
            margin-bottom: 5px;
            text-transform: uppercase;
            font-weight: bold;
        }

        .form-group select, .form-group input {
            padding: 10px;
            background-color: #1a1a1a;
            border: 1px solid #2a2a2a;
            border-radius: 10px;
            color: var(--text-main);
            font-size: 13px;
            outline: none;
        }

        .form-group select:focus, .form-group input:focus {
            border-color: var(--accent-pink);
        }

        /* TABELLA TURNI */
        .table-container {
            overflow-x: auto;
            border-radius: 12px;
            border: 1px solid var(--table-border);
            background: var(--card-bg);
            margin-bottom: 20px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            text-align: center;
            font-size: 13px;
            min-width: 650px;
        }

        th, td {
            border: 1px solid var(--table-border);
            padding: 8px 4px;
        }

        th {
            background-color: var(--table-header-bg);
            color: var(--accent-pink);
            font-weight: 800;
            text-transform: uppercase;
            font-size: 12px;
            letter-spacing: 0.5px;
        }

        th small {
            display: block;
            color: var(--text-main);
            font-size: 11px;
            font-weight: normal;
            margin-top: 2px;
        }

        td.staff-name {
            font-weight: bold;
            text-align: left;
            padding-left: 12px;
            background-color: #161616;
            color: #ffffff;
            width: 130px;
        }

        /* MENU A TENDINA CELLE */
        .cell-select {
            width: 100%;
            background: transparent;
            border: none;
            color: var(--text-main);
            text-align: center;
            text-align-last: center;
            font-weight: 600;
            font-size: 13px;
            outline: none;
            padding: 6px 2px;
            cursor: pointer;
            border-radius: 6px;
        }

        .cell-select option {
            background-color: #1a1a1a;
            color: #ffffff;
        }

        .cell-select:disabled {
            opacity: 1;
            color: var(--text-main);
            -webkit-text-fill-color: var(--text-main);
            cursor: default;
        }

        .cell-select.is-riposo {
            color: var(--red-riposo) !important;
            -webkit-text-fill-color: var(--red-riposo) !important;
            font-weight: bold;
        }

        /* MODALE PIN */
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.9); display: flex;
            justify-content: center; align-items: center; z-index: 1000;
        }

        .modal-card {
            background: var(--card-bg); padding: 24px; border-radius: 20px;
            width: 85%; max-width: 320px; text-align: center; border: 1px solid var(--accent-pink);
        }

        .pin-input {
            width: 100%; padding: 12px; font-size: 24px; letter-spacing: 8px;
            text-align: center; background: #1a1a1a; border: 1px solid #333;
            color: white; border-radius: 10px; margin: 16px 0; outline: none;
        }

        .modal-btns { display: flex; gap: 10px; }
        .btn-confirm { background: var(--accent-pink); color: white; border: none; padding: 12px; border-radius: 10px; flex: 1; font-weight: bold; cursor: pointer; }
        .btn-cancel { background: #2a2a2a; color: white; border: none; padding: 12px; border-radius: 10px; flex: 1; font-weight: bold; cursor: pointer; }

        .hidden { display: none !important; }

        /* PULSANTI GESTORE */
        .action-bar {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            margin-bottom: 20px;
        }

        .btn-action {
            background: #1a1a1a;
            border: 1px solid #333;
            color: white;
            padding: 10px 14px;
            border-radius: 10px;
            font-size: 12px;
            font-weight: bold;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .btn-action.primary {
            background: var(--accent-pink);
            border-color: var(--accent-pink);
        }

        .btn-action.whatsapp {
            background: #1f2c24;
            color: var(--green-wa);
            border-color: var(--green-wa);
        }

        @media print {
            .header button, .action-bar, .card { display: none !important; }
            body { background: #fff; color: #000; }
            th { background: #eee; color: #000; }
            td.staff-name { background: #f5f5f5; color: #000; }
            .cell-select { color: #000 !important; }
        }
    </style>
</head>
<body>

    <!-- HEADER LOGO PINK PANTHER -->
    <div class="header">
        <div class="logo-box">
            <div class="logo-row">
                <span class="pink-p">P</span>IN<span class="pink-k">K</span>
            </div>
            <div class="logo-row">
                P<span class="pink-a">A</span>N<span class="pink-h">T</span>H<span class="pink-h">E</span><span class="pink-r">R</span>
            </div>
        </div>
        <button class="toggle-btn" id="toggleViewBtn" onclick="handleViewToggle()">Modifica (PIN)</button>
    </div>

    <!-- SELETTORE DATA E PERIODO -->
    <div class="card">
        <div class="filter-grid">
            <div class="form-group">
                <label>Anno</label>
                <select id="selectYear" onchange="onYearMonthChange()"></select>
            </div>
            <div class="form-group">
                <label>Mese</label>
                <select id="selectMonth" onchange="onYearMonthChange()">
                    <option value="0">Gennaio</option>
                    <option value="1">Febbraio</option>
                    <option value="2">Marzo</option>
                    <option value="3">Aprile</option>
                    <option value="4">Maggio</option>
                    <option value="5">Giugno</option>
                    <option value="6">Luglio</option>
                    <option value="7">Agosto</option>
                    <option value="8">Settembre</option>
                    <option value="9">Ottobre</option>
                    <option value="10">Novembre</option>
                    <option value="11">Dicembre</option>
                </select>
            </div>
            <div class="form-group">
                <label>Settimana Reale</label>
                <select id="selectWeek" onchange="renderTable()"></select>
            </div>
            <div class="form-group">
                <label>Cerca Dipendente</label>
                <input type="text" id="searchStaff" placeholder="Es. Salvatore..." oninput="filterStaff()">
            </div>
        </div>
    </div>

    <!-- AZIONI GESTORE -->
    <div id="adminActions" class="action-bar hidden">
        <button class="btn-action primary" onclick="addStaffPrompt()">+ Aggiungi Dipendente</button>
        <button class="btn-action" onclick="saveShifts()">Salva Modifiche</button>
        <button class="btn-action whatsapp" onclick="shareWhatsApp()">Invia su WhatsApp</button>
        <button class="btn-action" onclick="window.print()">Stampa / Salva PDF</button>
    </div>

    <!-- TABELLA TURNI -->
    <div class="table-container">
        <table id="shiftsTable">
            <thead>
                <tr id="tableHeaderRow">
                    <th style="width:130px;">Dipendente</th>
                    <th>Lunedi</th>
                    <th>Martedi</th>
                    <th>Mercoledi</th>
                    <th>Giovedi</th>
                    <th>Venerdi</th>
                    <th>Sabato</th>
                    <th>Domenica</th>
                </tr>
            </thead>
            <tbody id="tableBody">
            </tbody>
        </table>
    </div>

    <!-- MODALE PIN -->
    <div id="pinModal" class="modal-overlay hidden">
        <div class="modal-card">
            <h3 style="margin:0; color:var(--accent-pink); font-size:16px;">AREA GESTORE TURNI</h3>
            <p style="font-size: 12px; color: var(--text-secondary); margin-top: 6px;">Inserisci il PIN per modificare i turni</p>
            <input type="password" id="pinCode" class="pin-input" maxlength="4" placeholder="****">
            <div class="modal-btns">
                <button class="btn-cancel" onclick="closePinModal()">Annulla</button>
                <button class="btn-confirm" onclick="verifyPin()">Entra</button>
            </div>
        </div>
    </div>

    <script>
        var MASTER_PIN = "1818";
        var isEditMode = false;
        var defaultStaff = ["Salvatore", "Marco", "Gennaro", "Titti", "Raffaella", "Maria", "Mirko"];

        function getStaffList() {
            var stored = localStorage.getItem('pk_staff_list_v2');
            return stored ? JSON.parse(stored) : defaultStaff;
        }

        function saveStaffList(list) {
            localStorage.setItem('pk_staff_list_v2', JSON.stringify(list));
        }

        function getShiftsData() {
            var stored = localStorage.getItem('pk_shifts_data');
            return stored ? JSON.parse(stored) : {};
        }

        function saveShiftsData(data) {
            localStorage.setItem('pk_shifts_data', JSON.stringify(data));
        }

        function initFilters() {
            var yearSelect = document.getElementById('selectYear');
            var now = new Date();
            var currentYear = now.getFullYear();
            var currentMonth = now.getMonth();

            for (var y = currentYear - 1; y <= currentYear + 2; y++) {
                var opt = document.createElement('option');
                opt.value = y;
                opt.innerText = y;
                if (y === currentYear) opt.selected = true;
                yearSelect.appendChild(opt);
            }

            document.getElementById('selectMonth').value = currentMonth;
            populateWeeks();
        }

        function onYearMonthChange() {
            populateWeeks();
            renderTable();
        }

        // Calcola i Lunedì e le Domeniche reali del mese
        function populateWeeks() {
            var year = parseInt(document.getElementById('selectYear').value);
            var month = parseInt(document.getElementById('selectMonth').value);
            var weekSelect = document.getElementById('selectWeek');
            weekSelect.innerHTML = "";

            // Trova la data iniziale del mese
            var firstDayOfMonth = new Date(year, month, 1);
            var dayOfWeek = firstDayOfMonth.getDay(); // 0 è Domenica, 1 è Lunedì...
            
            // Trova il primo lunedì del calendario associato a questo mese
            var diffToMonday = (dayOfWeek === 0 ? -6 : 1 - dayOfWeek);
            var currentMonday = new Date(year, month, 1 + diffToMonday);

            var lastDayOfMonth = new Date(year, month + 1, 0);

            var weekCount = 1;
            while (currentMonday <= lastDayOfMonth || weekCount === 1) {
                var sunday = new Date(currentMonday);
                sunday.setDate(sunday.getDate() + 6);

                var opt = document.createElement('option');
                // Salva la data esatta ISO YYYY-MM-DD del lunedì come valore
                var isoString = currentMonday.getFullYear() + "-" + String(currentMonday.getMonth() + 1).padStart(2, '0') + "-" + String(currentMonday.getDate()).padStart(2, '0');
                opt.value = isoString;

                var labelMon = currentMonday.getDate() + " " + getMonthNameShort(currentMonday.getMonth());
                var labelSun = sunday.getDate() + " " + getMonthNameShort(sunday.getMonth());
                opt.innerText = "Dal " + labelMon + " al " + labelSun;

                weekSelect.appendChild(opt);

                // Passa al Lunedì successivo
                currentMonday.setDate(currentMonday.getDate() + 7);
                weekCount++;
            }
        }

        function getMonthNameShort(m) {
            var names = ["Gen", "Feb", "Mar", "Apr", "Mag", "Giu", "Lug", "Ago", "Set", "Ott", "Nov", "Dic"];
            return names[m];
        }

        function renderTable() {
            var weekStartIso = document.getElementById('selectWeek').value;
            if (!weekStartIso) return;

            var parts = weekStartIso.split('-');
            var startDate = new Date(parseInt(parts[0]), parseInt(parts[1]) - 1, parseInt(parts[2]));

            var daysNames = ["Lunedì", "Martedì", "Mercoledì", "Giovedì", "Venerdì", "Sabato", "Domenica"];

            var headerRow = document.getElementById('tableHeaderRow');
            headerRow.innerHTML = '<th style="width:130px;">Dipendente</th>';

            var weekDates = [];
            for (var i = 0; i < 7; i++) {
                var d = new Date(startDate);
                d.setDate(d.getDate() + i);
                weekDates.push(d);

                headerRow.innerHTML += '<th>' + daysNames[i] + '<small>' + d.getDate() + ' ' + getMonthNameShort(d.getMonth()) + '</small></th>';
            }

            var staffList = getStaffList();
            var shiftsData = getShiftsData();
            var currentWeekData = shiftsData[weekStartIso] || {};

            var tbody = document.getElementById('tableBody');
            tbody.innerHTML = "";

            var optionsList = ["", "Mattina", "Pomeriggio", "Sera", "Riposo"];

            staffList.forEach(function(staff) {
                var rowHTML = '<tr><td class="staff-name">' + staff;
                if (isEditMode) {
                    rowHTML += ' <span onclick="removeStaff(\'' + staff + '\')" style="color:var(--red-riposo); cursor:pointer; font-size:10px; margin-left:4px;">(X)</span>';
                }
                rowHTML += '</td>';

                for (var dayIdx = 0; dayIdx < 7; dayIdx++) {
                    var cellVal = (currentWeekData[staff] && currentWeekData[staff][dayIdx]) ? currentWeekData[staff][dayIdx] : "";
                    var isRiposo = cellVal === "Riposo";

                    rowHTML += '<td>';
                    rowHTML += '<select class="cell-select ' + (isRiposo ? 'is-riposo' : '') + '" ' + (!isEditMode ? 'disabled' : '') + ' onchange="updateShiftMemory(\'' + staff + '\', ' + dayIdx + ', this.value)">';
                    
                    optionsList.forEach(function(opt) {
                        var selected = (cellVal === opt) ? 'selected' : '';
                        var label = opt === "" ? "-" : opt;
                        rowHTML += '<option value="' + opt + '" ' + selected + '>' + label + '</option>';
                    });

                    rowHTML += '</select>';
                    rowHTML += '</td>';
                }
                rowHTML += '</tr>';
                tbody.innerHTML += rowHTML;
            });
        }

        function updateShiftMemory(staff, dayIdx, value) {
            var weekStartIso = document.getElementById('selectWeek').value;
            var shiftsData = getShiftsData();
            if (!shiftsData[weekStartIso]) shiftsData[weekStartIso] = {};
            if (!shiftsData[weekStartIso][staff]) shiftsData[weekStartIso][staff] = ["", "", "", "", "", "", ""];

            shiftsData[weekStartIso][staff][dayIdx] = value;
            saveShiftsData(shiftsData);
            renderTable();
        }

        function saveShifts() {
            alert("Modifiche salvate con successo!");
        }

        function handleViewToggle() {
            if (isEditMode) {
                isEditMode = false;
                document.getElementById('adminActions').classList.add('hidden');
                document.getElementById('toggleViewBtn').innerText = "Modifica (PIN)";
                renderTable();
            } else {
                document.getElementById('pinModal').classList.remove('hidden');
                document.getElementById('pinCode').value = '';
                document.getElementById('pinCode').focus();
            }
        }

        function closePinModal() {
            document.getElementById('pinModal').classList.add('hidden');
        }

        function verifyPin() {
            var input = document.getElementById('pinCode').value.trim();
            if (input === MASTER_PIN) {
                isEditMode = true;
                closePinModal();
                document.getElementById('adminActions').classList.remove('hidden');
                document.getElementById('toggleViewBtn').innerText = "Esci da Modifica";
                renderTable();
            } else {
                alert("PIN Errato! Riprova con 1818.");
                document.getElementById('pinCode').value = '';
            }
        }

        function addStaffPrompt() {
            var name = prompt("Inserisci il nome del nuovo dipendente:");
            if (name && name.trim() !== "") {
                var staffList = getStaffList();
                if (staffList.indexOf(name.trim()) === -1) {
                    staffList.push(name.trim());
                    saveStaffList(staffList);
                    renderTable();
                } else {
                    alert("Questo dipendente esiste gia!");
                }
            }
        }

        function removeStaff(name) {
            if (confirm("Vuoi rimuovere " + name + " dalla lista turni?")) {
                var staffList = getStaffList().filter(function(s) { return s !== name; });
                saveStaffList(staffList);
                renderTable();
            }
        }

        function filterStaff() {
            var query = document.getElementById('searchStaff').value.toLowerCase();
            var rows = document.querySelectorAll('#tableBody tr');
            rows.forEach(function(r) {
                var name = r.querySelector('.staff-name').innerText.toLowerCase();
                r.style.display = name.indexOf(query) !== -1 ? "" : "none";
            });
        }

        function shareWhatsApp() {
            var weekSelect = document.getElementById('selectWeek');
            var weekLabel = weekSelect.options[weekSelect.selectedIndex].text;

            var msg = "*TURNI PINK PANTHER*\n(" + weekLabel + ")\n\n";
            
            var staffList = getStaffList();
            var shiftsData = getShiftsData();
            var weekStartIso = weekSelect.value;
            var currentWeekData = shiftsData[weekStartIso] || {};

            staffList.forEach(function(staff) {
                msg += "*" + staff + ":*\n";
                var days = ["Lun", "Mar", "Mer", "Gio", "Ven", "Sab", "Dom"];
                for (var i = 0; i < 7; i++) {
                    var val = (currentWeekData[staff] && currentWeekData[staff][i]) ? currentWeekData[staff][i] : "-";
                    msg += "   " + days[i] + ": " + val + "\n";
                }
                msg += "\n";
            });

            window.open("https://wa.me/?text=" + encodeURIComponent(msg), '_blank');
        }

        initFilters();
        renderTable();
    </script>
</body>
</html>
