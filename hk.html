<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nihongo GO - Hoàn Thiện</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            overflow: hidden;
        }
        .screen {
            display: none;
        }
        .active-screen {
            display: flex; 
            flex-direction: column;
            height: 100%;
        }
        .app-container {
            width: 100%;
            max-width: 420px;
            height: 100vh;
            max-height: 800px;
            margin: auto;
            background: white;
            border-radius: 24px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }
        .screen-content {
             padding: 24px;
             flex-grow: 1;
             display: flex;
             flex-direction: column;
             overflow: hidden;
        }
        .fixed-header {
            flex-shrink: 0;
        }
        .scrollable-area {
            flex-grow: 1;
            overflow-y: auto;
            margin-right: -16px; 
            padding-right: 16px;
        }
        .center-content {
             justify-content: center;
        }
        #login-screen-container {
            background-image: url('https://images.unsplash.com/photo-1528164344705-47542687000d?q=80&w=2094&auto=format&fit=crop');
            background-size: cover;
            background-position: center;
        }
        #login-screen-container .overlay {
            background: rgba(0,0,0,0.3);
            backdrop-filter: blur(2px);
            width:100%;
            height:100%;
            display:flex;
            flex-direction:column;
            justify-content:center;
        }
        .milestone-unlocked { background-color: #34d399; color: white; }
        .milestone-locked { background-color: #f87171; color: white; cursor: not-allowed; opacity: 0.8; }
        .milestone-current { background-color: #60a5fa; color: white; box-shadow: 0 0 15px rgba(96, 165, 250, 0.7); }
        .option-button.correct { background-color: #22c55e !important; color: white; transform: scale(1.05); }
        .option-button.incorrect { background-color: #ef4444 !important; color: white; }
        .option-button.disabled { opacity: 0.7; pointer-events: none; }
        @keyframes fall {
            0% { transform: translateY(-10vh) rotate(0deg); opacity: 1; }
            100% { transform: translateY(100vh) rotate(720deg); opacity: 0; }
        }
        .sakura-petal {
            position: absolute; width: 15px; height: 15px; background: #ffb7c5;
            border-radius: 15px 0; animation-name: fall; animation-timing-function: linear;
            pointer-events: none; z-index: 1001;
        }
        .modal-overlay {
            position: fixed; top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0, 0, 0, 0.5); display: flex; align-items: center; justify-content: center;
            z-index: 2000; opacity: 0; pointer-events: none; transition: opacity 0.3s ease;
        }
        .modal-overlay.visible { opacity: 1; pointer-events: auto; }
        .modal-content {
            background: white; padding: 24px; border-radius: 12px;
            max-width: 400px; width: 90%; text-align: center;
            transform: scale(0.9); transition: transform 0.3s ease;
        }
        .modal-overlay.visible .modal-content { transform: scale(1); }
    </style>
</head>
<body class="bg-gray-200">

    <div class="app-container">
        <div id="loading-screen" class="active-screen bg-indigo-500 text-white center-content">
            <div class="text-center">
                <h1 class="text-5xl font-bold mb-8">Nihongo GO</h1>
                <svg class="animate-spin h-8 w-8 text-white mx-auto mt-4" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                    <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                </svg>
                 <p id="loading-status" class="mt-4 text-sm opacity-80">Đang khởi tạo...</p>
            </div>
        </div>
        
        <div id="login-screen-container" class="screen center-content">
            <div class="overlay">
                 <div class="screen-content text-center text-white">
                    <h1 class="text-5xl font-bold mb-3" style="text-shadow: 2px 2px 4px rgba(0,0,0,0.5);">Nihongo GO</h1>
                    <p class="text-lg mb-12" style="text-shadow: 1px 1px 2px rgba(0,0,0,0.5);">Chinh phục tiếng Nhật, khám phá văn hoá.</p>
                    <button id="login-button" class="w-full bg-white/30 backdrop-blur-sm border border-white/50 text-white font-bold py-3 px-4 rounded-lg hover:bg-white/40 transition duration-300 transform hover:scale-105">
                        Bắt đầu hành trình
                    </button>
                    <p class="text-xs text-center text-white/80 mt-6">User ID của bạn: <span id="user-id-display" class="font-mono">...</span></p>
                </div>
            </div>
        </div>

        <div id="main-app-screen" class="screen">
            <div class="screen-content">
            </div>
        </div>
    </div>
    
    <div id="custom-alert-modal" class="modal-overlay">
        <div class="modal-content">
            <h3 id="modal-title" class="text-xl font-bold text-red-600 mb-3">Thông báo</h3>
            <p id="modal-message" class="text-gray-700 mb-5">Đã có lỗi xảy ra.</p>
            <button id="modal-close-button" class="bg-indigo-500 text-white font-bold py-2 px-6 rounded-lg hover:bg-indigo-600 transition">Đã hiểu</button>
        </div>
    </div>

    <div id="social-share-modal" class="modal-overlay">
        <div class="modal-content">
            <h3 class="text-xl font-bold text-gray-800 mb-4">Chia sẻ thành tích!</h3>
            <p class="text-gray-600 mb-6">Hãy khoe kết quả của bạn với bạn bè nhé!</p>
            <div class="flex justify-center gap-4">
                 <button id="share-facebook" class="w-12 h-12 bg-blue-600 text-white rounded-full flex items-center justify-center text-2xl font-bold">f</button>
                 <button id="share-twitter" class="w-12 h-12 bg-black text-white rounded-full flex items-center justify-center text-2xl font-bold">X</button>
                 <button id="share-zalo" class="w-12 h-12 bg-blue-500 text-white rounded-full flex items-center justify-center text-2xl font-bold">Z</button>
            </div>
             <button id="social-modal-close-button" class="mt-6 text-sm text-gray-500 hover:underline">Đóng</button>
        </div>
    </div>
    
    <div id="sakura-container" class="absolute top-0 left-0 w-full h-full pointer-events-none overflow-hidden"></div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged, signOut, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, getDoc, setDoc, updateDoc, collection, writeBatch, getDocs, query, where, increment, arrayUnion, orderBy, limit, Timestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const appId = typeof __app_id !== 'undefined' ? __app_id : 'nihongo-go-mvp-default';
        let firebaseConfig;
        try {
            firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
        } catch(e) { firebaseConfig = {}; }
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
        
        let app, auth, db;
        let currentUser = null, userProfile = null;
        let currentQuiz = [], currentQuestionIndex = 0, score = 0, timerInterval;
        let activeEvents = [];

        const screens = {
            loading: document.getElementById('loading-screen'),
            login: document.getElementById('login-screen-container'),
            mainApp: document.getElementById('main-app-screen'),
        };
        const mainAppContent = document.getElementById('main-app-screen').firstElementChild;
        const userIdDisplay = document.getElementById('user-id-display');
        const modal = document.getElementById('custom-alert-modal');
        const modalMessage = document.getElementById('modal-message');
        const modalCloseButton = document.getElementById('modal-close-button');
        const socialShareModal = document.getElementById('social-share-modal');
        const loadingStatus = document.getElementById('loading-status');

        function showScreen(screenId) {
            Object.values(screens).forEach(s => s.classList.remove('active-screen'));
            if(screens[screenId]) screens[screenId].classList.add('active-screen');
        }

        function showAlert(message, title = "Lỗi") {
            document.getElementById('modal-title').textContent = title;
            modalMessage.textContent = message;
            modal.classList.add('visible');
        }
        modalCloseButton.addEventListener('click', () => modal.classList.remove('visible'));
        document.getElementById('social-modal-close-button').addEventListener('click', () => socialShareModal.classList.remove('visible'));
        
        function initializeServices() {
            loadingStatus.textContent = "Kiểm tra cấu hình...";
            if (!firebaseConfig || !firebaseConfig.apiKey) {
                showAlert("Lỗi cấu hình ứng dụng. Không thể kết nối đến máy chủ.");
                showScreen('login');
                return false;
            }
            loadingStatus.textContent = "Kết nối đến máy chủ...";
            app = initializeApp(firebaseConfig);
            auth = getAuth(app);
            db = getFirestore(app);
            return true;
        }

        function renderUI(uiName, props = {}) {
            let html = '';
            switch(uiName) {
                case 'mainMenu':
                    let eventsHtml = '';
                    if (activeEvents.length > 0) {
                        activeEvents.forEach(event => {
                            eventsHtml += `<div class="p-4 mb-4 text-center bg-red-100 border-l-4 border-red-500 text-red-700 rounded-lg">
                                <h3 class="font-bold">${event.icon} ${event.name}</h3>
                                <p class="text-sm">${event.description}</p>
                            </div>`;
                        });
                    }
                    html = `
                        <div class="fixed-header">
                            <h1 class="text-2xl font-bold text-center mb-6">Menu Chính</h1>
                            <div class="flex justify-between items-center mb-4 p-4 bg-gray-100 rounded-lg">
                                <div>
                                    <p class="text-sm text-gray-500">Kinh nghiệm</p>
                                    <p class="text-lg font-bold text-indigo-600">XP: <span id="xp-display">${userProfile.xp}</span></p>
                                </div>
                                <div>
                                    <p class="text-sm text-gray-500">Điểm thưởng</p>
                                    <p class="text-lg font-bold text-pink-500">🌸 Sakura: <span id="sakura-display">${userProfile.sakura}</span></p>
                                </div>
                                <button id="logout-button" class="text-sm text-gray-500 hover:text-indigo-600 self-start">Đăng xuất</button>
                            </div>
                            ${eventsHtml}
                        </div>
                        <div class="scrollable-area">
                            <div class="grid grid-cols-1 gap-4">
                                <button id="roadmap-button" class="bg-blue-500 text-white font-bold py-4 rounded-lg hover:bg-blue-600 transition">Lộ Trình Học Tập</button>
                                <button id="store-button" class="bg-red-500 text-white font-bold py-4 rounded-lg hover:bg-red-600 transition">Cửa Hàng</button>
                                <button id="leaderboard-button" class="bg-green-500 text-white font-bold py-4 rounded-lg hover:bg-green-600 transition">Bảng Xếp Hạng</button>
                                <button id="community-button" class="bg-purple-500 text-white font-bold py-4 rounded-lg hover:bg-purple-600 transition">Cộng Đồng</button>
                            </div>
                        </div>
                    `;
                    break;
                case 'communityHome':
                     html = `
                        <div class="fixed-header">
                            <button id="back-to-main-menu-button" class="mb-4 text-indigo-600 hover:underline">&lt; Quay lại Menu</button>
                            <h1 class="text-2xl font-bold text-center mb-6">Cộng Đồng</h1>
                        </div>
                        <div class="scrollable-area">
                             <div class="grid grid-cols-1 gap-4">
                                <button id="hall-of-fame-button" class="bg-amber-500 text-white font-bold py-4 rounded-lg hover:bg-amber-600 transition">🏆 Đại Sảnh Danh Vọng</button>
                                <button id="achievements-button" class="bg-yellow-500 text-white font-bold py-4 rounded-lg hover:bg-yellow-600 transition">🎖️ Bộ Sưu Tập Thành Tựu</button>
                                <button id="forums-button" class="bg-purple-500 text-white font-bold py-4 rounded-lg hover:bg-purple-600 transition">💬 Diễn đàn Thảo luận</button>
                            </div>
                        </div>
                    `;
                    break;
                case 'store':
                     html = `
                        <div class="fixed-header">
                            <button id="back-to-main-menu-button" class="mb-4 text-indigo-600 hover:underline">&lt; Quay lại Menu</button>
                            <h1 class="text-2xl font-bold text-center mb-6">Cửa Hàng</h1>
                        </div>
                        <div class="scrollable-area">
                             <div class="grid grid-cols-1 gap-4">
                                <button id="bookstore-button" class="bg-orange-500 text-white font-bold py-4 rounded-lg hover:bg-orange-600 transition">📚 Nhà sách Nihongo</button>
                                <button id="reward-center-button" class="bg-teal-500 text-white font-bold py-4 rounded-lg hover:bg-teal-600 transition">🎁 Trung tâm Phần thưởng</button>
                            </div>
                        </div>
                    `;
                    break;
                case 'bookstore':
                    let booksHtml = '';
                    props.products.forEach(book => {
                        booksHtml += `
                            <div class="bg-gray-50 rounded-lg p-4 flex items-start gap-4">
                                <img src="${book.image_url}" class="w-20 h-28 object-cover rounded-md shadow-md" onerror="this.src='https://placehold.co/80x112/e2e8f0/334155?text=Sách'"/>
                                <div class="flex-grow">
                                    <h3 class="font-bold">${book.name}</h3>
                                    <p class="text-sm text-gray-600 mt-1">${book.description}</p>
                                    <button data-url="${book.url}" class="buy-book-button mt-3 bg-indigo-500 text-white text-sm font-bold py-2 px-4 rounded-lg hover:bg-indigo-600 transition">Xem trên đối tác</button>
                                </div>
                            </div>
                        `;
                    });
                    html = `
                         <div class="fixed-header">
                            <button id="back-to-store-button" class="mb-4 text-indigo-600 hover:underline">&lt; Quay lại Cửa hàng</button>
                            <h1 class="text-2xl font-bold text-center mb-6">📚 Nhà sách Nihongo</h1>
                        </div>
                        <div class="scrollable-area">
                            <div class="space-y-4">${booksHtml}</div>
                        </div>
                    `;
                    break;
                case 'rewardCenter':
                    let rewardsHtml = '';
                    props.rewards.forEach(reward => {
                        const canAfford = userProfile.sakura >= reward.koban_cost;
                        rewardsHtml += `
                            <div class="bg-gray-50 rounded-lg p-4 flex items-center gap-4">
                                <div class="flex-grow">
                                    <h3 class="font-bold">${reward.name}</h3>
                                    <p class="text-sm text-gray-600 mt-1">${reward.description}</p>
                                    <p class="text-lg font-bold text-pink-500 mt-2">🌸 ${reward.koban_cost}</p>
                                </div>
                                <button data-reward-id="${reward.id}" data-cost="${reward.koban_cost}" class="redeem-button ${canAfford ? 'bg-green-500 hover:bg-green-600' : 'bg-gray-400 cursor-not-allowed'} text-white font-bold py-2 px-4 rounded-lg transition" ${!canAfford ? 'disabled' : ''}>Đổi</button>
                            </div>
                        `;
                    });
                    html = `
                         <div class="fixed-header">
                            <button id="back-to-store-button" class="mb-4 text-indigo-600 hover:underline">&lt; Quay lại Cửa hàng</button>
                            <h1 class="text-2xl font-bold text-center mb-6">🎁 Trung tâm Phần thưởng</h1>
                        </div>
                        <div class="scrollable-area">
                            <div class="space-y-4">${rewardsHtml}</div>
                        </div>
                    `;
                    break;
                case 'n5Milestones':
                    let milestonesHtml = '';
                    props.milestones.forEach(milestone => {
                         const isUnlocked = userProfile.unlocked_milestones.includes(milestone.id);
                         const nextMilestoneId = `n5_milestone_${milestone.order + 1}`;
                         const isCurrent = isUnlocked && !userProfile.unlocked_milestones.includes(nextMilestoneId);
                         const isLastAndUnlocked = props.milestones.length === milestone.order && isUnlocked;

                         let buttonClass = 'milestone-locked';
                         if (isUnlocked) {
                             buttonClass = (isCurrent || isLastAndUnlocked) ? 'milestone-current' : 'milestone-unlocked';
                         }
                        
                         milestonesHtml += `
                            <button data-milestone-id="${milestone.id}" class="milestone-button w-full font-bold py-3 px-4 rounded-lg flex items-center justify-center gap-4 transition ${buttonClass}" ${!isUnlocked ? 'disabled' : ''}>
                                <span class="text-2xl">${isUnlocked ? '🔓' : '🔒'}</span>
                                <span>${milestone.name}</span>
                            </button>
                         `;
                    });
                    html = `
                        <div class="fixed-header">
                            <button id="back-to-main-menu-button" class="mb-4 text-indigo-600 hover:underline">&lt; Quay lại Menu</button>
                            <h1 class="text-2xl font-bold text-center mb-6">Chinh Phục N5</h1>
                        </div>
                        <div class="scrollable-area">
                            <div class="space-y-3">${milestonesHtml}</div>
                        </div>
                    `;
                    break;
                case 'quiz':
                    let optionsHtml = '';
                    const shuffledOptions = [...props.question.options].sort(() => 0.5 - Math.random());
                    shuffledOptions.forEach(option => {
                        optionsHtml += `<button data-option="${option}" class="option-button bg-blue-400 text-white p-3 rounded-lg text-left transition hover:bg-blue-500 transform hover:-translate-y-1">${option}</button>`;
                    });
                    html = `
                        <div class="fixed-header">
                            <div class="flex justify-between items-center mb-4">
                                <h2 class="text-xl font-bold">Câu <span id="question-number">${props.index + 1}</span>/10</h2>
                                <div class="text-lg font-bold text-red-500"><span id="timer">15</span>s</div>
                            </div>
                            <div class="bg-gray-100 p-6 rounded-lg mb-4 min-h-[120px] flex items-center justify-center">
                                 <p class="text-center text-lg font-semibold">${props.question.content}</p>
                                 ${props.question.item_type === 'listening' ? '<button id="play-audio-btn" class="ml-4 p-2 bg-indigo-500 rounded-full text-white">🔊</button>' : ''}
                            </div>
                        </div>
                        <div class="scrollable-area">
                            <div class="grid grid-cols-1 gap-3">${optionsHtml}</div>
                        </div>
                    `;
                    break;
                case 'result':
                     html = `
                        <div class="text-center relative flex flex-col justify-center items-center h-full">
                            <h1 id="result-title" class="text-4xl font-bold mb-4">${props.isWin ? '🎉 Chúc mừng! 🎉' : '🤔 Cố gắng hơn nhé! 🤔'}</h1>
                            <p class="text-lg mb-6">${props.isWin ? 'Bạn đã xuất sắc vượt qua cột mốc này!' : 'Đừng nản lòng, hãy ôn tập và thử lại nào!'}</p>
                            <p class="text-2xl font-bold mb-8">Điểm số: <span id="result-score">${props.score}</span>/10</p>
                            <div class="flex gap-4">
                                <button id="back-to-milestones-button" class="bg-indigo-600 text-white font-bold py-3 px-6 rounded-lg hover:bg-indigo-700 transition">
                                    Tiếp tục hành trình
                                </button>
                                <button id="share-community-button" class="bg-green-500 text-white font-bold py-3 px-6 rounded-lg hover:bg-green-600 transition">
                                    Khoe lên diễn đàn
                                </button>
                            </div>
                             <button id="share-social-button" class="mt-4 text-sm text-blue-500 hover:underline">Chia sẻ lên mạng xã hội khác</button>
                        </div>
                     `;
                     break;
                case 'leaderboard':
                    let leaderboardHtml = '';
                    props.scores.forEach((entry, index) => {
                        leaderboardHtml += `
                            <div class="flex items-center p-3 rounded-lg ${index < 3 ? 'bg-yellow-100' : 'bg-gray-50'}">
                                <span class="font-bold text-lg w-8">${index + 1}</span>
                                <span class="flex-grow font-semibold">${entry.name || 'Người dùng ẩn danh'}</span>
                                <span class="font-bold text-indigo-600">${entry.xp} XP</span>
                            </div>
                        `;
                    });
                     html = `
                        <div class="fixed-header">
                            <button id="back-to-main-menu-button" class="mb-4 text-indigo-600 hover:underline">&lt; Quay lại Menu</button>
                            <h1 class="text-2xl font-bold text-center mb-6">Bảng Xếp Hạng Tuần</h1>
                        </div>
                        <div class="scrollable-area">
                            <div class="space-y-2">${leaderboardHtml}</div>
                        </div>
                     `;
                    break;
                case 'hallOfFame':
                     let topUsersHtml = '';
                     props.topUsers.forEach((user, index) => {
                         let medal = '';
                         if (index === 0) medal = '🥇';
                         if (index === 1) medal = '🥈';
                         if (index === 2) medal = '🥉';
                         topUsersHtml += `
                            <div class="flex items-center p-4 rounded-lg bg-gradient-to-r from-yellow-50 to-amber-100 border-l-4 border-amber-400">
                                <span class="text-4xl w-10">${medal || index + 1}</span>
                                <div class="ml-4">
                                    <h3 class="font-bold text-amber-800">${user.name}</h3>
                                    <p class="text-sm text-amber-600">${user.xp} XP</p>
                                </div>
                            </div>
                         `;
                     });
                     html = `
                        <div class="fixed-header">
                            <button id="back-to-community-button" class="mb-4 text-indigo-600 hover:underline">&lt; Quay lại Cộng đồng</button>
                            <h1 class="text-2xl font-bold text-center mb-6">🏆 Đại Sảnh Danh Vọng</h1>
                        </div>
                        <div class="scrollable-area">
                            <div class="space-y-3">${topUsersHtml}</div>
                        </div>
                     `;
                    break;
                case 'achievements':
                     let achievementsHtml = '';
                     props.achievements.forEach(ach => {
                         const isUnlocked = (userProfile.unlocked_achievements || []).includes(ach.id);
                         achievementsHtml += `
                            <div class="flex items-center p-4 rounded-lg ${isUnlocked ? 'bg-green-100' : 'bg-gray-100'}">
                                <span class="text-4xl ${isUnlocked ? '' : 'opacity-30'}">${ach.icon}</span>
                                <div class="ml-4">
                                    <h3 class="font-bold ${isUnlocked ? 'text-green-800' : 'text-gray-800'}">${ach.name}</h3>
                                    <p class="text-sm ${isUnlocked ? 'text-green-600' : 'text-gray-500'}">${ach.description}</p>
                                </div>
                            </div>
                         `;
                     });
                     html = `
                        <div class="fixed-header">
                            <button id="back-to-community-button" class="mb-4 text-indigo-600 hover:underline">&lt; Quay lại Cộng đồng</button>
                            <h1 class="text-2xl font-bold text-center mb-6">Bộ Sưu Tập Thành Tựu</h1>
                        </div>
                        <div class="scrollable-area">
                            <div class="space-y-3">${achievementsHtml}</div>
                        </div>
                     `;
                    break;
                case 'communityForums':
                    let forumsHtml = '';
                    props.forums.forEach(forum => {
                        forumsHtml += `<button data-forum-id="${forum.id}" data-forum-title="${forum.title}" class="forum-button text-left w-full p-4 bg-gray-100 rounded-lg hover:bg-gray-200 transition">
                            <h3 class="font-bold text-gray-800">${forum.title}</h3>
                        </button>`;
                    });
                    html = `
                        <div class="fixed-header">
                            <button id="back-to-community-button" class="mb-4 text-indigo-600 hover:underline">&lt; Quay lại Cộng đồng</button>
                            <h1 class="text-2xl font-bold text-center mb-6">Diễn đàn</h1>
                        </div>
                        <div class="scrollable-area">
                            <div class="space-y-3">${forumsHtml}</div>
                        </div>
                    `;
                    break;
                case 'communityThreads':
                    let threadsHtml = '<p class="text-center text-gray-500 mb-4">Chưa có bài đăng nào.</p>';
                    if (props.threads.length > 0) {
                        threadsHtml = '';
                        props.threads.forEach(thread => {
                             threadsHtml += `<div data-thread-id="${thread.id}" class="thread-item p-3 bg-gray-50 rounded-lg hover:bg-gray-100 cursor-pointer">
                                <p class="font-semibold">${thread.title}</p>
                                <p class="text-xs text-gray-500">bởi ${thread.userName || 'một người dùng'}</p>
                             </div>`;
                        });
                    }
                    html = `
                        <div class="fixed-header">
                            <button id="back-to-forums-button" class="mb-4 text-indigo-600 hover:underline">&lt; Quay lại Diễn đàn</button>
                            <h1 class="text-2xl font-bold text-center mb-6">${props.forumTitle}</h1>
                        </div>
                        <div class="scrollable-area">
                            <div class="space-y-2">${threadsHtml}</div>
                        </div>
                        <div class="flex-shrink-0 pt-4">
                            <button id="create-thread-button" class="w-full bg-purple-600 text-white font-bold py-3 rounded-lg hover:bg-purple-700 transition">Tạo bài đăng mới</button>
                        </div>
                    `;
                    break;
                case 'communityThreadDetail':
                    let postsHtml = '<p class="text-center text-gray-500 mt-4">Chưa có bình luận nào.</p>';
                    if (props.posts.length > 0) {
                        postsHtml = '';
                        props.posts.forEach(post => {
                            postsHtml += `<div class="bg-gray-100 p-3 rounded-lg">
                                <p class="text-sm">${post.content}</p>
                                <p class="text-xs text-right text-gray-500 mt-2">- ${post.userName || 'Người dùng ẩn danh'}</p>
                            </div>`;
                        });
                    }
                     html = `
                        <div class="fixed-header">
                            <button id="back-to-threads-button" data-forum-id="${props.thread.forum_id}" data-forum-title="${props.forumTitle}" class="mb-4 text-indigo-600 hover:underline">&lt; Quay lại Bài đăng</button>
                            <div class="p-4 bg-blue-50 rounded-lg">
                                <h1 class="text-xl font-bold">${props.thread.title}</h1>
                                <p class="text-sm text-gray-600">bởi ${props.thread.userName}</p>
                            </div>
                        </div>
                        <div class="scrollable-area py-4 space-y-3">${postsHtml}</div>
                        <div class="flex-shrink-0 pt-4 border-t">
                             <textarea id="comment-input" class="w-full border rounded-lg p-2" rows="2" placeholder="Viết bình luận..."></textarea>
                             <button id="post-comment-button" class="w-full mt-2 bg-purple-600 text-white font-bold py-3 rounded-lg hover:bg-purple-700 transition">Gửi bình luận</button>
                        </div>
                     `;
                    break;
            }
            mainAppContent.innerHTML = html;
            addEventListeners(uiName, props);
        }
        
        function addEventListeners(uiName, props = {}) {
            const backToMenuButton = document.getElementById('back-to-main-menu-button');
            if (backToMenuButton) backToMenuButton.addEventListener('click', () => renderUI('mainMenu'));

            switch(uiName) {
                case 'mainMenu':
                    document.getElementById('logout-button').addEventListener('click', () => signOut(auth));
                    document.getElementById('roadmap-button').addEventListener('click', handleShowMilestones);
                    document.getElementById('leaderboard-button').addEventListener('click', handleShowLeaderboard);
                    document.getElementById('store-button').addEventListener('click', () => renderUI('store'));
                    document.getElementById('community-button').addEventListener('click', () => renderUI('communityHome'));
                    break;
                case 'communityHome':
                     document.getElementById('hall-of-fame-button').addEventListener('click', handleShowHallOfFame);
                     document.getElementById('achievements-button').addEventListener('click', handleShowAchievements);
                     document.getElementById('forums-button').addEventListener('click', handleShowForums);
                     break;
                case 'store':
                case 'bookstore':
                case 'rewardCenter':
                     const backToStoreBtn = document.getElementById('back-to-store-button');
                     if(backToStoreBtn) backToStoreBtn.addEventListener('click', () => renderUI('store'));
                     if (uiName === 'store') {
                        document.getElementById('bookstore-button').addEventListener('click', handleShowBookstore);
                        document.getElementById('reward-center-button').addEventListener('click', handleShowRewardCenter);
                     }
                     if (uiName === 'bookstore') {
                         document.querySelectorAll('.buy-book-button').forEach(btn => btn.addEventListener('click', (e) => window.open(e.target.dataset.url, '_blank')));
                     }
                     if (uiName === 'rewardCenter') {
                         document.querySelectorAll('.redeem-button').forEach(btn => btn.addEventListener('click', (e) => redeemReward(e.target.dataset.rewardId, e.target.dataset.cost)));
                     }
                    break;
                case 'n5Milestones':
                case 'leaderboard':
                case 'achievements':
                case 'hallOfFame':
                    const backToCommunityButton = document.getElementById('back-to-community-button');
                    if (backToCommunityButton) backToCommunityButton.addEventListener('click', () => renderUI('communityHome'));
                    if (uiName === 'n5Milestones'){
                        document.querySelectorAll('.milestone-button').forEach(btn => {
                            if (!btn.disabled) btn.addEventListener('click', () => startQuiz(btn.dataset.milestoneId));
                        });
                    }
                    break;
                case 'quiz':
                    document.querySelectorAll('.option-button').forEach(btn => btn.addEventListener('click', (e) => handleAnswer(e.target.dataset.option)));
                    if (props.question.item_type === 'listening') {
                        document.getElementById('play-audio-btn').addEventListener('click', () => speak(props.question.answer, 'ja-JP'));
                    }
                    break;
                case 'result':
                    document.getElementById('back-to-milestones-button').addEventListener('click', handleShowMilestones);
                    document.getElementById('share-community-button').addEventListener('click', () => shareToCommunity(props.score));
                    document.getElementById('share-social-button').addEventListener('click', () => showSocialShareModal(props.score));
                    break;
                case 'communityForums':
                    document.querySelectorAll('.forum-button').forEach(btn => {
                        btn.addEventListener('click', (e) => handleShowThreads(e.currentTarget.dataset.forumId, e.currentTarget.dataset.forumTitle));
                    });
                    break;
                case 'communityThreads':
                    document.getElementById('back-to-forums-button').addEventListener('click', handleShowForums);
                    document.getElementById('create-thread-button').addEventListener('click', () => showAlert("Tính năng tạo bài đăng mới sẽ sớm được ra mắt!", "Thông báo"));
                    document.querySelectorAll('.thread-item').forEach(item => {
                        item.addEventListener('click', (e) => handleShowThreadDetail(e.currentTarget.dataset.threadId, props.forumTitle));
                    });
                    break;
                case 'communityThreadDetail':
                     document.getElementById('back-to-threads-button').addEventListener('click', (e) => handleShowThreads(e.currentTarget.dataset.forumId, e.currentTarget.dataset.forumTitle));
                     document.getElementById('post-comment-button').addEventListener('click', () => showAlert("Bạn đã bình luận thành công! (Tính năng đang được mô phỏng)", "Thành công!"));
                    break;
            }
        }
        
        document.getElementById('login-button').addEventListener('click', async () => {
            showScreen('loading');
            loadingStatus.textContent = "Đang xác thực...";
            try {
                if (initialAuthToken) {
                     try {
                        await signInWithCustomToken(auth, initialAuthToken);
                    } catch (error) {
                        if (error.code === 'auth/invalid-custom-token' || error.code === 'auth/invalid-claims') {
                            await signInAnonymously(auth);
                        } else { throw error; }
                    }
                } else {
                    await signInAnonymously(auth);
                }
            } catch (error) {
                showAlert(`Đăng nhập thất bại: ${error.message}.`);
                showScreen('login');
            }
        });

        function setupAuthListener() {
            onAuthStateChanged(auth, async (user) => {
                if (user) {
                    currentUser = user;
                    userIdDisplay.textContent = user.uid.substring(0, 12) + '...';
                    
                    try {
                        await setupMockDataIfNeeded(); 
                        await loadUserProfile();
                        await checkAndDisplayEvents();
                        showScreen('mainApp');
                        renderUI('mainMenu');
                    } catch (error) {
                        showAlert(`Đã xảy ra lỗi khi tải dữ liệu: ${error.message}. Vui lòng thử lại.`);
                        signOut(auth);
                    }
                } else {
                    currentUser = null; userProfile = null;
                    showScreen('login');
                }
            });
        }
        
        async function loadUserProfile() {
            const userDocRef = doc(db, `artifacts/${appId}/users`, currentUser.uid);
            const userDocSnap = await getDoc(userDocRef);
            if (userDocSnap.exists()) {
                userProfile = userDocSnap.data();
            } else {
                userProfile = {
                    xp: 0, 
                    sakura: 500, 
                    unlocked_milestones: ['n5_milestone_1'],
                    unlocked_achievements: [],
                    name: 'Tân Binh',
                    createdAt: new Date()
                };
                await setDoc(userDocRef, userProfile);
            }
        }

        async function handleShowMilestones() {
            mainAppContent.innerHTML = '<div class="text-center p-10"><p>Đang tải lộ trình...</p></div>';
            const lessonsRef = collection(db, `artifacts/${appId}/public/data/lessons`);
            const querySnapshot = await getDocs(query(lessonsRef, orderBy("order")));
            let milestones = [];
            querySnapshot.forEach(doc => milestones.push({ id: doc.id, ...doc.data() }));
            await loadUserProfile();
            renderUI('n5Milestones', { milestones });
        }
        
        async function startQuiz(lessonId) {
             mainAppContent.innerHTML = '<div class="text-center p-10"><p>Đang chuẩn bị câu hỏi...</p></div>';
             const allItems = await fetchItemsForLesson(lessonId);
            
            const vocabItems = allItems.filter(q => q.item_type === 'vocab').sort(() => 0.5 - Math.random()).slice(0, 3);
            const kanjiItems = allItems.filter(q => q.item_type === 'kanji').sort(() => 0.5 - Math.random()).slice(0, 1);
            const grammarItems = allItems.filter(q => q.item_type === 'grammar').sort(() => 0.5 - Math.random()).slice(0, 3);
            const listeningItems = allItems.filter(q => q.item_type === 'listening').sort(() => 0.5 - Math.random()).slice(0, 2);
            const readingItems = allItems.filter(q => q.item_type === 'reading').sort(() => 0.5 - Math.random()).slice(0, 1);
            
            currentQuiz = [...vocabItems, ...kanjiItems, ...grammarItems, ...listeningItems, ...readingItems].sort(() => 0.5 - Math.random());
            
            if (currentQuiz.length < 10) {
                 showAlert("Cột mốc này chưa có đủ câu hỏi để tạo bài thi."); handleShowMilestones(); return;
            }

            currentQuestionIndex = 0;
            score = 0;
            renderNextQuestion();
        }

        async function fetchItemsForLesson(lessonId) {
            const q = query(collection(db, `artifacts/${appId}/public/data/items`), where("lesson_id", "==", lessonId));
            const querySnapshot = await getDocs(q);
            return querySnapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
        }

        function renderNextQuestion() {
            if (currentQuestionIndex >= currentQuiz.length) {
                endQuiz(); return;
            }
            const question = currentQuiz[currentQuestionIndex];
            renderUI('quiz', { question, index: currentQuestionIndex });
            
            let timeLeft = 15;
            const timerEl = document.getElementById('timer');
            timerInterval = setInterval(() => {
                timeLeft--;
                if(timerEl) timerEl.textContent = timeLeft;
                if (timeLeft <= 0) handleAnswer(null); 
            }, 1000);
        }
        
        function handleAnswer(selectedOption) {
            clearInterval(timerInterval);
            const question = currentQuiz[currentQuestionIndex];
            const isCorrect = selectedOption === question.answer;

            document.querySelectorAll('.option-button').forEach(btn => {
                btn.classList.add('disabled');
                if (btn.dataset.option === question.answer) btn.classList.add('correct');
                else if (btn.dataset.option === selectedOption) btn.classList.add('incorrect');
            });

            if(isCorrect) {
                score++;
                createSakuraPetals(5);
            }
            
            currentQuestionIndex++;
            setTimeout(renderNextQuestion, 2000);
        }

        async function endQuiz() {
             mainAppContent.innerHTML = '<div class="text-center p-10"><p>Đang tính điểm...</p></div>';
             const userDocRef = doc(db, `artifacts/${appId}/users`, currentUser.uid);
             const lessonId = currentQuiz[0].lesson_id;
             const lessonOrder = parseInt(lessonId.split('_').pop());
             const isWin = score >= 7;
             
             const xpGained = isWin ? score * 10 : -10;
             const sakuraGained = isWin ? score * 5 : -10;

             const updates = {
                 xp: increment(xpGained),
                 sakura: increment(sakuraGained),
             };

             if (isWin) { 
                const nextMilestoneId = `n5_milestone_${lessonOrder + 1}`;
                updates.unlocked_milestones = arrayUnion(nextMilestoneId);
             }
             
             await updateDoc(userDocRef, updates);
             await updateLeaderboard(currentUser.uid, userProfile.name, userProfile.xp + xpGained);
             await checkAndGrantAchievement('first_quiz');
             if (isWin) await checkAndGrantAchievement('first_win');
             
             await loadUserProfile();
             renderUI('result', { isWin, score });
             if (isWin) createSakuraPetals(30);
        }
        
        function speak(text, lang) {
            if ('speechSynthesis' in window) {
                speechSynthesis.cancel();
                const utterance = new SpeechSynthesisUtterance(text);
                utterance.lang = lang;
                utterance.rate = 0.9;
                speechSynthesis.speak(utterance);
            } else {
                showAlert("Trình duyệt của bạn không hỗ trợ phát âm thanh.");
            }
        }

        function createSakuraPetals(count) {
            const container = document.getElementById('sakura-container');
            for(let i=0; i < count; i++) {
                const petal = document.createElement('div');
                petal.className = 'sakura-petal';
                petal.style.left = `${Math.random() * 100}vw`;
                petal.style.animationDuration = `${Math.random() * 3 + 4}s`;
                petal.style.animationDelay = `${Math.random() * 2}s`;
                container.appendChild(petal);
                setTimeout(() => petal.remove(), 7000);
            }
        }
        
        function getWeekNumber(d) {
            d = new Date(Date.UTC(d.getFullYear(), d.getMonth(), d.getDate()));
            d.setUTCDate(d.getUTCDate() + 4 - (d.getUTCDay()||7));
            var yearStart = new Date(Date.UTC(d.getUTCFullYear(),0,1));
            var weekNo = Math.ceil((((d - yearStart) / 86400000) + 1)/7);
            return [d.getUTCFullYear(), weekNo];
        }

        function getCurrentLeaderboardId() {
            const [year, week] = getWeekNumber(new Date());
            return `leaderboard_${year}_W${week}`;
        }

        async function updateLeaderboard(userId, userName, newXp) {
            const leaderboardId = getCurrentLeaderboardId();
            const leaderboardDocRef = doc(db, `artifacts/${appId}/public/data/leaderboards/${leaderboardId}/scores`, userId);
            await setDoc(leaderboardDocRef, { xp: newXp, name: userName }, { merge: true });
        }

        async function handleShowLeaderboard() {
            mainAppContent.innerHTML = '<div class="text-center p-10"><p>Đang tải bảng xếp hạng...</p></div>';
            const leaderboardId = getCurrentLeaderboardId();
            const scoresRef = collection(db, `artifacts/${appId}/public/data/leaderboards/${leaderboardId}/scores`);
            const q = query(scoresRef, orderBy("xp", "desc"), limit(50));
            const querySnapshot = await getDocs(q);
            const scores = [];
            querySnapshot.forEach(doc => scores.push(doc.data()));
            renderUI('leaderboard', { scores });
        }

        async function handleShowHallOfFame() {
            mainAppContent.innerHTML = '<div class="text-center p-10"><p>Đang tải đại sảnh...</p></div>';
            const hallOfFameRef = collection(db, `artifacts/${appId}/public/data/hall_of_fame/monthly/scores`);
            const q = query(hallOfFameRef, orderBy("xp", "desc"), limit(10));
            const querySnapshot = await getDocs(q);
            const topUsers = [];
            querySnapshot.forEach(doc => topUsers.push(doc.data()));
            renderUI('hallOfFame', { topUsers });
        }

        async function checkAndGrantAchievement(achievementId) {
            if (userProfile && !(userProfile.unlocked_achievements || []).includes(achievementId)) {
                const userDocRef = doc(db, `artifacts/${appId}/users`, currentUser.uid);
                await updateDoc(userDocRef, {
                    unlocked_achievements: arrayUnion(achievementId)
                });
                await loadUserProfile();
                const achRef = doc(db, `artifacts/${appId}/public/data/achievements`, achievementId);
                const achSnap = await getDoc(achRef);
                if (achSnap.exists()) {
                    showAlert(`Bạn đã mở khóa thành tựu mới: ${achSnap.data().name}!`, "Chúc mừng!");
                }
            }
        }

        async function handleShowAchievements() {
            mainAppContent.innerHTML = '<div class="text-center p-10"><p>Đang tải thành tựu...</p></div>';
            const achievementsRef = collection(db, `artifacts/${appId}/public/data/achievements`);
            const querySnapshot = await getDocs(achievementsRef);
            const achievements = [];
            querySnapshot.forEach(doc => achievements.push({ id: doc.id, ...doc.data() }));
            renderUI('achievements', { achievements });
        }

        async function handleShowBookstore() {
            mainAppContent.innerHTML = '<div class="text-center p-10"><p>Đang tải nhà sách...</p></div>';
            const productsRef = collection(db, `artifacts/${appId}/public/data/products`);
            const querySnapshot = await getDocs(productsRef);
            const products = [];
            querySnapshot.forEach(doc => products.push(doc.data()));
            renderUI('bookstore', { products });
        }
        
        async function handleShowRewardCenter() {
            mainAppContent.innerHTML = '<div class="text-center p-10"><p>Đang tải phần thưởng...</p></div>';
            const rewardsRef = collection(db, `artifacts/${appId}/public/data/rewards`);
            const querySnapshot = await getDocs(rewardsRef);
            const rewards = [];
            querySnapshot.forEach(doc => rewards.push({ id: doc.id, ...doc.data() }));
            renderUI('rewardCenter', { rewards });
        }

        async function redeemReward(rewardId, cost) {
            const costNum = parseInt(cost);
            if (userProfile.sakura >= costNum) {
                const userDocRef = doc(db, `artifacts/${appId}/users`, currentUser.uid);
                await updateDoc(userDocRef, { sakura: increment(-costNum) });
                await loadUserProfile();
                showAlert(`Bạn đã đổi thành công phần thưởng! Mã voucher của bạn là: NIHONGO-GO-${Math.random().toString(36).substring(2, 8).toUpperCase()}`, "Thành công!");
                handleShowRewardCenter();
            } else {
                showAlert("Bạn không đủ 🌸 Sakura để đổi phần thưởng này.");
            }
        }
        
        async function checkAndDisplayEvents() {
            const now = new Date("2025-07-01T16:15:00Z"); 
            const eventsRef = collection(db, `artifacts/${appId}/public/data/events`);
            const querySnapshot = await getDocs(eventsRef);
            activeEvents = [];
            querySnapshot.forEach(doc => {
                const event = doc.data();
                if (now >= event.start_date.toDate() && now <= event.end_date.toDate()) {
                    activeEvents.push(event);
                }
            });
        }

        async function handleShowForums() {
            mainAppContent.innerHTML = '<div class="text-center p-10"><p>Đang tải diễn đàn...</p></div>';
            const forumsRef = collection(db, `artifacts/${appId}/public/data/forums`);
            const querySnapshot = await getDocs(forumsRef);
            const forums = [];
            querySnapshot.forEach(doc => forums.push({ id: doc.id, ...doc.data() }));
            renderUI('communityForums', { forums });
        }

        async function handleShowThreads(forumId, forumTitle) {
            mainAppContent.innerHTML = '<div class="text-center p-10"><p>Đang tải bài đăng...</p></div>';
            const threadsRef = collection(db, `artifacts/${appId}/public/data/threads`);
            const q = query(threadsRef, where("forum_id", "==", forumId), orderBy("createdAt", "desc"));
            const querySnapshot = await getDocs(q);
            const threads = [];
            querySnapshot.forEach(doc => threads.push({id: doc.id, ...doc.data()}));
            renderUI('communityThreads', { threads, forumId, forumTitle });
        }

        async function handleShowThreadDetail(threadId, forumTitle) {
             mainAppContent.innerHTML = '<div class="text-center p-10"><p>Đang tải chi tiết...</p></div>';
             const threadRef = doc(db, `artifacts/${appId}/public/data/threads`, threadId);
             const threadSnap = await getDoc(threadRef);
             if(!threadSnap.exists()) {
                 showAlert("Không tìm thấy bài đăng này.");
                 handleShowForums();
                 return;
             }
             const thread = threadSnap.data();

             const postsRef = collection(db, `artifacts/${appId}/public/data/posts`);
             const q = query(postsRef, where("thread_id", "==", threadId), orderBy("createdAt", "asc"));
             const postsSnapshot = await getDocs(q);
             const posts = [];
             postsSnapshot.forEach(doc => posts.push(doc.data()));

             renderUI('communityThreadDetail', { thread, posts, forumTitle });
        }

        async function shareToCommunity(score) {
            const title = `Vừa hoàn thành một cột mốc với ${score}/10 điểm!`;
            const threadRef = doc(collection(db, `artifacts/${appId}/public/data/threads`));
            await setDoc(threadRef, {
                forum_id: 'general_discussion',
                user_id: currentUser.uid,
                userName: userProfile.name,
                title: title,
                content: `Hãy cùng chúc mừng ${userProfile.name} nhé!`,
                createdAt: Timestamp.now()
            });
            showAlert("Đã chia sẻ thành tích lên diễn đàn!", "Thành công!");
        }

        function showSocialShareModal(score) {
            const text = encodeURIComponent(`Tôi vừa đạt ${score}/10 điểm trong Nihongo GO! Hãy tham gia cùng tôi để chinh phục tiếng Nhật! #NihongoGO #hoctiengnhat`);
            const url = encodeURIComponent("https://nihongo-go.example.com");

            document.getElementById('share-facebook').onclick = () => window.open(`https://www.facebook.com/sharer/sharer.php?u=${url}&quote=${text}`, '_blank');
            document.getElementById('share-twitter').onclick = () => window.open(`https://twitter.com/intent/tweet?url=${url}&text=${text}`, '_blank');
            document.getElementById('share-zalo').onclick = () => showAlert("Tính năng chia sẻ Zalo cần tích hợp SDK chuyên sâu.", "Thông báo");
            
            socialShareModal.classList.add('visible');
        }

        async function setupMockDataIfNeeded() {
            loadingStatus.textContent = "Kiểm tra dữ liệu...";
            const flagRef = doc(db, `artifacts/${appId}/public/data/setup_flags`, "phase3_v6_final_community");
            if ((await getDoc(flagRef)).exists()) return;

            loadingStatus.textContent = "Đang khởi tạo dữ liệu lần đầu...";
            
            const batch = writeBatch(db);
            const lessons = [];
            const lessonTopics = [
                "Chào hỏi & Bảng chữ cái", "Giới thiệu bản thân", "Mua sắm cơ bản", "Ngày & Giờ", "Đi lại & Địa điểm", "Ăn uống", "Gia đình & Bạn bè", "Tính từ & Sở thích", "Vị trí & Sự tồn tại", "Đếm & Tần suất",
                "Động từ & Thể Te", "Xin phép & Cấm đoán", "Liên kết hành động", "Diễn tả mong muốn", "Tặng và nhận (Nâng cao)", "Cách nói lý do", "Thể Phủ định (V-nai)", "Sự cần thiết", "Thể Quá khứ (V-ta)", "So sánh",
                "Thể từ điển (V-ru) & Sở thích", "Bổ nghĩa danh từ", "Cho/Nhận (Kureru/Ageru)", "Thể điều kiện (Tara)", "Ôn tập tổng hợp (Bài 1-25)",
            ];
            for (let i = 1; i <= 50; i++) {
                let name = `Cột mốc ${i}: ${lessonTopics[i-1] || '(Chủ đề N5 mở rộng)'}`;
                if (i > 25 && i < 49) name = `Cột mốc ${i}: (Chủ đề N5 mở rộng ${i-25})`;
                if (i === 49) name = "Cột mốc 49: Ôn tập tổng hợp N5";
                if (i === 50) name = "Cột mốc 50: Thi thử cuối khóa N5";
                lessons.push({ id: `n5_milestone_${i}`, name: name, order: i });
            }
            lessons.forEach(l => batch.set(doc(db,`artifacts/${appId}/public/data/lessons`,l.id),{name: l.name, order: l.order, course_id: 'N5'}));
            
            const forums = [
                { id: 'general_discussion', title: 'Thảo luận chung' },
                { id: 'grammar_questions', title: 'Hỏi đáp Ngữ pháp' },
                { id: 'kanji_help', title: 'Trợ giúp Kanji' },
            ];
            forums.forEach(f => batch.set(doc(db, `artifacts/${appId}/public/data/forums`, f.id), f));

            const threads = [
                { forum_id: 'general_discussion', user_id: 'system', userName: 'Admin', title: 'Chào mừng đến với cộng đồng Nihongo GO!', content: 'Hãy cùng nhau chia sẻ và học hỏi nhé!', createdAt: Timestamp.now() },
                { forum_id: 'grammar_questions', user_id: 'system', userName: 'Admin', title: 'Phân biệt は và が như thế nào?', content: 'Đây là câu hỏi nhiều bạn thắc mắc, hãy cùng thảo luận!', createdAt: Timestamp.now() }
            ];
            threads.forEach(t => batch.set(doc(collection(db, `artifacts/${appId}/public/data/threads`)), t));
            
            await batch.commit();

            let itemsBatch = writeBatch(db);
            let writeCount = 0;
            for(let i = 1; i <= 50; i++) {
                const lessonId = `n5_milestone_${i}`;
                const items = [
                    { content: `Từ vựng mốc ${i} - A`, options: ["A", "B", "C", "D"], answer: "A", item_type: "vocab" }, { content: `Từ vựng mốc ${i} - B`, options: ["A", "B", "C", "D"], answer: "B", item_type: "vocab" },
                    { content: `Từ vựng mốc ${i} - C`, options: ["A", "B", "C", "D"], answer: "C", item_type: "vocab" }, { content: `Kanji mốc ${i}`, options: ["漢", "字", "A", "B"], answer: "漢", item_type: "kanji" },
                    { content: `Ngữ pháp mốc ${i} - 1`, options: ["1", "2", "3", "4"], answer: "1", item_type: "grammar" }, { content: `Ngữ pháp mốc ${i} - 2`, options: ["1", "2", "3", "4"], answer: "2", item_type: "grammar" },
                    { content: `Ngữ pháp mốc ${i} - 3`, options: ["1", "2", "3", "4"], answer: "3", item_type: "grammar" }, { content: `Nghe hiểu mốc ${i} - 1`, options: ["Nghe A", "Nghe B", "Nghe C", "Nghe D"], answer: "Nghe A", item_type: "listening" },
                    { content: `Nghe hiểu mốc ${i} - 2`, options: ["Nghe A", "Nghe B", "Nghe C", "Nghe D"], answer: "Nghe B", item_type: "listening" }, { content: `Đọc hiểu mốc ${i}`, options: ["Đọc A", "Đọc B", "Đọc C", "Đọc D"], answer: "Đọc A", item_type: "reading" },
                ];
                items.forEach(q => {
                    const itemRef = doc(collection(db, `artifacts/${appId}/public/data/items`));
                    itemsBatch.set(itemRef, { ...q, lesson_id: lessonId });
                    writeCount++;
                    if (writeCount >= 490) { 
                        itemsBatch.commit();
                        itemsBatch = writeBatch(db);
                        writeCount = 0;
                    }
                });
            }
            if(writeCount > 0) await itemsBatch.commit();
            
            const otherDataBatch = writeBatch(db);
            const achievements = [
                { id: 'first_quiz', name: 'Bước chân đầu tiên', description: 'Hoàn thành bài kiểm tra đầu tiên.', icon: '🔰' },
                { id: 'first_win', name: 'Chiến thắng đầu tay', description: 'Vượt qua một cột mốc lần đầu tiên.', icon: '🏆' },
                { id: 'ten_wins', name: 'Chinh phục gia', description: 'Vượt qua 10 cột mốc.', icon: '🎖️' }
            ];
            achievements.forEach(ach => otherDataBatch.set(doc(db, `artifacts/${appId}/public/data/achievements`, ach.id), ach));
            
            const products = [
                { name: 'Giáo trình Minna no Nihongo 1', description: 'Sách giáo khoa N5 phổ biến nhất.', url: 'https://tiki.vn/', image_url: 'https://placehold.co/80x112/e0f2fe/0c4a6e?text=Minna+1' },
                { name: 'Giáo trình Genki I', description: 'Giáo trình được yêu thích tại các trường đại học.', url: 'https://tiki.vn/', image_url: 'https://placehold.co/80x112/dcfce7/14532d?text=Genki+1' },
            ];
            products.forEach(p => otherDataBatch.set(doc(collection(db, `artifacts/${appId}/public/data/products`)), p));

            const rewards = [
                { name: 'Giảm giá 10% gói Premium', description: 'Giảm 10% cho tháng Premium đầu tiên.', koban_cost: 5000, type: 'discount' },
                { name: 'Voucher 50.000đ Nhà sách', description: 'Đổi lấy voucher mua sắm tại Nihongo Shoten.', koban_cost: 10000, type: 'voucher' },
            ];
            rewards.forEach(r => otherDataBatch.set(doc(collection(db, `artifacts/${appId}/public/data/rewards`)), r));

            const events = [
                { name: 'Lễ hội Gion Matsuri', description: 'Khám phá lễ hội mùa hè lớn nhất Kyoto!', icon: '🏮', start_date: Timestamp.fromDate(new Date('2025-07-01')), end_date: Timestamp.fromDate(new Date('2025-07-31')) }
            ];
            events.forEach(e => otherDataBatch.set(doc(collection(db, `artifacts/${appId}/public/data/events`)), e));
            
            const topUsers = [
                { name: 'Yamada Kenji', xp: 9850 }, { name: 'Sato Yui', xp: 9500 }, { name: 'Tanaka Hiroshi', xp: 9200 }
            ];
            topUsers.forEach(u => otherDataBatch.set(doc(collection(db, `artifacts/${appId}/public/data/hall_of_fame/monthly/scores`)), u));

            otherDataBatch.set(flagRef, { completed: true, timestamp: new Date() });
            await otherDataBatch.commit();
        }
        
        async function main() {
            showScreen('loading');
            try {
                if (initializeServices()) {
                    setupAuthListener();
                }
            } catch (error) {
                console.error("Lỗi nghiêm trọng khi khởi tạo dịch vụ:", error);
                showAlert(`Không thể khởi tạo ứng dụng: ${error.message}`);
                showScreen('login');
            }
        }

        main();
    </script>
</body>
</html>


