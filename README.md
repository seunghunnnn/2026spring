# 2026spring
2021130896 최승훈


<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vibe-Match Pro</title>
    <style>
        :root {
            --primary: #0984e3; --student: #00b894; --instructor: #6c5ce7;
            --bg: #f4f6f9; --dark: #2d3436; --white: #ffffff; --border: #dfe6e9;
        }

        body { font-family: 'Pretendard', sans-serif; background: var(--bg); margin: 0; color: var(--dark); line-height: 1.6; }
        
        /* 네비게이션 */
        .navbar { background: var(--dark); color: white; padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; }
        .nav-menus { display: flex; gap: 10px; }
        .nav-btn { background: rgba(255,255,255,0.1); border: none; color: white; padding: 8px 16px; border-radius: 8px; cursor: pointer; font-weight: bold; }
        .nav-btn.active { background: var(--primary); }
        .nav-btn[data-mode="student"].active { background: var(--student); }
        .nav-btn[data-mode="instructor"].active { background: var(--instructor); }

        .container { max-width: 900px; margin: 20px auto; padding: 0 20px; }
        .view-section { display: none; animation: fadeIn 0.4s ease; }
        .view-section.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* 공통 카드 */
        .card { background: var(--white); padding: 25px; border-radius: 12px; box-shadow: 0 4px 15px rgba(0,0,0,0.03); border: 1px solid var(--border); margin-bottom: 20px; }
        .card h2 { margin-top: 0; font-size: 1.2rem; border-bottom: 2px solid var(--bg); padding-bottom: 10px; margin-bottom: 20px; }
        
        /* 통계 그리드 (추가된 부분의 스타일 지원용) */
        .stats-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; }
        .stat-box { background: var(--bg); padding: 20px; border-radius: 8px; text-align: center; }
        .stat-box .num { font-size: 1.5rem; font-weight: bold; margin: 5px 0 0 0; }
        .stat-box .num.student-color { color: var(--student); }

        /* 달력 스타일 */
        .calendar-grid { display: grid; grid-template-columns: repeat(7, 1fr); gap: 5px; margin-bottom: 20px; }
        .calendar-day { padding: 10px; text-align: center; background: #f8f9fa; border-radius: 6px; cursor: pointer; font-size: 0.9rem; border: 1px solid transparent; }
        .calendar-day:hover { background: #e9ecef; }
        .calendar-day.active { border-color: var(--primary); background: #eef2ff; font-weight: bold; }
        .calendar-day.has-booking { color: var(--primary); font-weight: bold; }
        .day-header { font-weight: bold; color: #636e72; padding-bottom: 5px; }

        /* 시간표 및 리스트 */
        .timetable { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        .list-item { display: flex; justify-content: space-between; align-items: center; padding: 12px; border: 1px solid var(--border); border-radius: 8px; background: var(--white); font-size: 0.9rem; }
        .btn { border: none; padding: 8px 15px; border-radius: 6px; cursor: pointer; font-weight: bold; color: white; }
        
        /* 피드백 타임라인 */
        .timeline-item { padding: 15px; background: #f8f9fa; border-radius: 10px; margin-bottom: 15px; border-left: 5px solid var(--student); position: relative; }
        .round-badge { background: var(--student); color: white; padding: 2px 8px; border-radius: 4px; font-size: 0.8rem; margin-right: 10px; }
    </style>
</head>
<body>

    <div class="navbar">
        <h1>Vibe-Match</h1>
        <div class="nav-menus">
            <button class="nav-btn active" data-mode="general" onclick="switchMode('general')">🏠 홈</button>
            <button class="nav-btn" data-mode="student" onclick="switchMode('student')">🎓 수강생</button>
            <button class="nav-btn" data-mode="instructor" onclick="switchMode('instructor')">🛠️ 강사</button>
        </div>
    </div>

    <div class="container">
        
        <div id="viewGeneral" class="view-section active">
            <div class="card" style="text-align:center; padding: 50px;">
                <h2 style="border:none; font-size: 2rem;">Vibe-Match Pro</h2>
                <p>강사와 수강생이 실시간으로 소통하는 스마트 레슨 플랫폼</p>
                <div style="margin-top: 30px;">
                    <button class="btn" style="background:var(--student); margin-right:10px;" onclick="switchMode('student')">수강생 시작하기</button>
                    <button class="btn" style="background:var(--instructor);" onclick="switchMode('instructor')">강사 관리 시작</button>
                </div>
            </div>
        </div>

        <div id="viewStudent" class="view-section">
            <div class="card" style="background: var(--student); color: white; border: none;">
                <h2 style="border: none; margin: 0 0 10px 0; color: white;">최승훈 님, 오늘 실력을 얼마나 키워볼까요?</h2>
                <p style="margin: 0; opacity: 0.9;">📍 지정 장소: <span id="stuLocation" style="font-weight: bold;">로딩중...</span></p>
            </div>

            <div class="card">
                <h2>📊 이번 달 수강 및 결제 정보</h2>
                <div class="stats-grid">
                    <div class="stat-box">
                        <div style="font-size: 0.9rem; color: #636e72;">총 예약 횟수</div>
                        <p class="num student-color" id="stuTotalLessons">0회</p>
                    </div>
                    <div class="stat-box">
                        <div style="font-size: 0.9rem; color: #636e72;">결제 예정 금액</div>
                        <p class="num student-color" id="stuExpectedCost">0원</p>
                    </div>
                </div>
                <p style="text-align: right; font-size: 0.8rem; color: #b2bec3; margin-top: 10px;">* 금액은 강사님이 설정한 단가에 따라 실시간 반영됩니다.</p>
            </div>

            <div class="card">
                <h2>📅 레슨 예약 (날짜를 먼저 선택하세요)</h2>
                <div class="calendar-grid" id="stuCalendar"></div>
                <div id="stuSelectedDateLabel" style="font-weight:bold; margin-bottom:10px; color:var(--student);"></div>
                <div class="timetable" id="stuTimetable"></div>
            </div>

            <div class="card">
                <h2>📝 나의 회차별 성장 기록 (Feedback)</h2>
                <div id="stuTimeline"></div>
            </div>
        </div>

        <div id="viewInstructor" class="view-section">
            <div class="card">
                <h2>⚙️ 수업료 및 장소 관리</h2>
                <div style="display:flex; gap:10px;">
                    <input type="number" id="instPrice" value="60000" style="flex:1; padding:10px; border-radius:8px; border:1px solid var(--border);">
                    <input type="text" id="instLocation" value="강남 골프 스튜디오" style="flex:2; padding:10px; border-radius:8px; border:1px solid var(--border);">
                    <button class="btn" style="background:var(--instructor);" onclick="saveSettings()">저장</button>
                </div>
            </div>

            <div class="card">
                <h2>📅 전체 스케줄 관리</h2>
                <div class="calendar-grid" id="instCalendar"></div>
                <div id="instSelectedDateLabel" style="font-weight:bold; margin-bottom:10px; color:var(--instructor);"></div>
                <div class="timetable" id="instTimetable"></div>
            </div>

            <div class="card">
                <h2>📝 피드백 전송 (수강생 기록 반영)</h2>
                <select id="instRoundSelect" style="width:100%; padding:10px; margin-bottom:10px; border-radius:8px;">
                    <option value="1">1회차 레슨</option>
                    <option value="2">2회차 레슨</option>
                    <option value="3">3회차 레슨</option>
                    <option value="4">4회차 레슨</option>
                </select>
                <textarea id="instFeedbackText" style="width:100%; height:80px; padding:10px; border-radius:8px; border:1px solid var(--border);" placeholder="오늘의 피드백을 입력하세요."></textarea>
                <button class="btn" style="background:var(--instructor); width:100%; margin-top:10px;" onclick="sendFeedback()">피드백 전송하기</button>
            </div>
        </div>
    </div>

<script>
    // --- 데이터 상태 ---
    let appState = {
        currentViewDate: '2026-04-23',
        hourlyRate: 60000,
        location: "강남 골프 스튜디오",
        feedbacks: [
            { round: 1, date: '2026-04-10', text: '그립과 어드레스의 기초를 다졌습니다.' }
        ],
        // 날짜별 스케줄 데이터 저장
        schedules: {
            '2026-04-23': [
                { time: '10:00', status: 'available' }, { time: '10:30', status: 'booked', student: '최승훈' },
                { time: '14:00', status: 'available' }, { time: '14:30', status: 'available' }
            ],
            '2026-04-24': [
                { time: '11:00', status: 'available' }, { time: '15:00', status: 'available' }
            ]
        }
    };

    // --- 화면 전환 ---
    function switchMode(mode) {
        document.querySelectorAll('.view-section').forEach(el => el.classList.remove('active'));
        document.querySelectorAll('.nav-btn').forEach(el => el.classList.remove('active'));
        document.getElementById(`view${mode.charAt(0).toUpperCase() + mode.slice(1)}`).classList.add('active');
        document.querySelector(`[data-mode="${mode}"]`).classList.add('active');
        renderAll();
    }

    // --- 날짜 변경 ---
    function selectDate(date) {
        appState.currentViewDate = date;
        if(!appState.schedules[date]) {
            appState.schedules[date] = [
                { time: '10:00', status: 'available' }, { time: '11:00', status: 'available' },
                { time: '14:00', status: 'available' }, { time: '15:00', status: 'available' }
            ];
        }
        renderAll();
    }

    // --- 강사 기능 ---
    function saveSettings() {
        appState.hourlyRate = Number(document.getElementById('instPrice').value);
        appState.location = document.getElementById('instLocation').value;
        alert('단가와 장소가 업데이트되었습니다.');
        renderAll();
    }

    function sendFeedback() {
        const round = document.getElementById('instRoundSelect').value;
        const text = document.getElementById('instFeedbackText').value;
        if(!text) return alert('피드백을 입력하세요.');
        
        appState.feedbacks.unshift({
            round: round,
            date: new Date().toLocaleDateString(),
            text: text
        });
        document.getElementById('instFeedbackText').value = '';
        alert(`${round}회차 피드백이 전송되었습니다.`);
        renderAll();
    }

    // --- 수강생 예약 ---
    function book(idx) {
        appState.schedules[appState.currentViewDate][idx].status = 'booked';
        appState.schedules[appState.currentViewDate][idx].student = '최승훈';
        renderAll();
    }

    function cancel(idx) {
        appState.schedules[appState.currentViewDate][idx].status = 'available';
        appState.schedules[appState.currentViewDate][idx].student = '';
        renderAll();
    }

    // --- 렌더링 엔진 ---
    function renderAll() {
        // 🎯 복구된 부분 3: 전체 예약 횟수 및 금액 실시간 계산 로직
        let totalBookings = 0;
        Object.values(appState.schedules).forEach(daySlots => {
            totalBookings += daySlots.filter(s => s.status === 'booked' && s.student === '최승훈').length;
        });
        const expectedCost = totalBookings * appState.hourlyRate;

        // 수강생 화면에 데이터 동기화
        const stuLocationEl = document.getElementById('stuLocation');
        if(stuLocationEl) stuLocationEl.innerText = appState.location;
        
        const stuTotalLessonsEl = document.getElementById('stuTotalLessons');
        if(stuTotalLessonsEl) stuTotalLessonsEl.innerText = `${totalBookings}회`;
        
        const stuExpectedCostEl = document.getElementById('stuExpectedCost');
        if(stuExpectedCostEl) stuExpectedCostEl.innerText = `${expectedCost.toLocaleString()}원`;

        // 나머지 기존 렌더링 유지
        renderCalendar('stuCalendar');
        renderCalendar('instCalendar');
        renderTimeline();
        renderTimetable('stuTimetable', 'student');
        renderTimetable('instTimetable', 'instructor');
        
        document.getElementById('stuSelectedDateLabel').innerText = `📅 선택된 날짜: ${appState.currentViewDate}`;
        document.getElementById('instSelectedDateLabel').innerText = `📅 선택된 날짜: ${appState.currentViewDate}`;
    }

    function renderCalendar(id) {
        const cal = document.getElementById(id);
        cal.innerHTML = '';
        const days = ['일','월','화','수','목','금','토'];
        days.forEach(d => cal.innerHTML += `<div class="day-header">${d}</div>`);
        
        for(let i=20; i<=30; i++) {
            const dateStr = `2026-04-${i}`;
            const isActive = appState.currentViewDate === dateStr ? 'active' : '';
            cal.innerHTML += `<div class="calendar-day ${isActive}" onclick="selectDate('${dateStr}')">${i}</div>`;
        }
    }

    function renderTimetable(id, mode) {
        const target = document.getElementById(id);
        target.innerHTML = '';
        const slots = appState.schedules[appState.currentViewDate] || [];
        
        slots.forEach((s, idx) => {
            let html = `<div class="list-item"><span>${s.time}</span>`;
            if(s.status === 'booked') {
                html += `<span>${s.student} 예약됨</span>`;
                if(mode === 'instructor' || s.student === '최승훈') {
                    html += `<button class="btn" style="background:#ff7675;" onclick="cancel(${idx})">취소</button>`;
                }
            } else {
                html += `<span>비어있음</span>`;
                if(mode === 'student') html += `<button class="btn" style="background:var(--student);" onclick="book(${idx})">예약</button>`;
            }
            html += `</div>`;
            target.innerHTML += html;
        });
    }

    function renderTimeline() {
        const tl = document.getElementById('stuTimeline');
        tl.innerHTML = '';
        appState.feedbacks.forEach(f => {
            tl.innerHTML += `
                <div class="timeline-item">
                    <div><span class="round-badge">${f.round}회차</span> <strong>${f.date}</strong></div>
                    <p style="margin-top:10px;">${f.text}</p>
                </div>`;
        });
    }

    window.onload = renderAll;
</script>
</body>
</html>
