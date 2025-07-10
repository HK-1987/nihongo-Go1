<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Velo-Tomo: Dự án Hoàn thiện</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <!-- Chart.js for Performance Analysis -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <style>
        body { font-family: 'Inter', sans-serif; }
        .nav-button.active { color: #4f46e5; /* indigo-600 */ }
        .footer-icon { width: 24px; height: 24px; margin-bottom: 2px; }
        #login-screen::before {
            content: '';
            position: absolute;
            top: 0; left: 0; right: 0; bottom: 0;
            background-image: url('https://images.unsplash.com/photo-1541625602330-22773444f428?q=80&w=2070&auto=format&fit=crop');
            background-size: cover;
            background-position: center;
            filter: blur(2px) brightness(0.8);
            z-index: 1;
        }
        .route-option-btn.active {
            background-color: #f97316; /* orange-500 */
            color: white;
            border-color: #f97316;
        }
        .progress-bar-fill { transition: width 0.5s ease-in-out; }
    </style>
</head>
<body class="bg-gray-100">

    <div id="app" class="h-screen overflow-hidden">
        <!-- MÀN HÌNH XÁC THỰC -->
        <div id="auth-container">
            <div id="login-screen" class="relative min-h-screen flex items-center justify-center bg-gray-900 p-4">
                <div class="relative z-10 max-w-md w-full space-y-8"><div class="text-center text-white"><h2 class="text-5xl font-extrabold tracking-tight">Velo-Tomo</h2><p class="mt-2 text-lg text-gray-200">Người bạn đồng hành trên mọi nẻo đường Nhật Bản</p></div><div class="bg-gradient-to-br from-cyan-500 to-orange-400 rounded-2xl p-8 shadow-2xl space-y-6"><p class="text-center text-white font-medium">Đăng nhập để bắt đầu hành trình của bạn</p><button type="button" id="google-login-button" class="group relative w-full flex items-center justify-center py-3 px-4 border border-transparent font-semibold rounded-md text-gray-800 bg-white hover:bg-gray-200 transition-all duration-300 transform hover:scale-105"><svg class="h-6 w-6 mr-3" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 48 48"><path fill="#FFC107" d="M43.611,20.083H42V20H24v8h11.303c-1.649,4.657-6.08,8-11.303,8c-6.627,0-12-5.373-12-12c0-6.627,5.373-12,12-12c3.059,0,5.842,1.154,7.961,3.039l5.657-5.657C34.046,6.053,29.268,4,24,4C12.955,4,4,12.955,4,24c0,11.045,8.955,20,20,20c11.045,0,20-8.955,20-20C44,22.659,43.862,21.35,43.611,20.083z"/><path fill="#FF3D00" d="M6.306,14.691l6.571,4.819C14.655,15.108,18.961,12,24,12c3.059,0,5.842,1.154,7.961,3.039l5.657-5.657C34.046,6.053,29.268,4,24,4C16.318,4,9.656,8.337,6.306,14.691z"/><path fill="#4CAF50" d="M24,44c5.166,0,9.86-1.977,13.409-5.192l-6.19-5.238C29.211,35.091,26.715,36,24,36c-5.202,0-9.619-3.317-11.283-7.946l-6.522,5.025C9.505,39.556,16.227,44,24,44z"/><path fill="#1976D2" d="M43.611,20.083H42V20H24v8h11.303c-0.792,2.237-2.231,4.166-4.087,5.571l6.19,5.238C41.38,36.148,44,30.638,44,24C44,22.659,43.862,21.35,43.611,20.083z"/></svg>Đăng nhập với Google</button><div class="text-center"><a href="#" id="email-login-toggle" class="text-sm text-white hover:text-gray-200">Hoặc đăng nhập bằng email</a></div><div id="email-form" class="hidden space-y-4 pt-4"><input id="login-email" type="email" required class="appearance-none rounded-md relative block w-full px-4 py-3 border border-gray-300 placeholder-gray-500 bg-white" placeholder="Địa chỉ email" value="demo@velo-tomo.jp"><input id="login-password" type="password" required class="appearance-none rounded-md relative block w-full px-4 py-3 border border-gray-300 placeholder-gray-500 bg-white" placeholder="Mật khẩu" value="password123"><button type="button" id="login-button" class="group relative w-full flex justify-center py-3 px-4 border border-transparent font-semibold rounded-md text-white bg-orange-500 hover:bg-orange-600">Đăng nhập</button></div><p id="login-error" class="text-yellow-300 text-sm mt-2 text-center font-semibold"></p></div>
                </div>
            </div>
        </div>

        <!-- MÀN HÌNH CHÍNH (SAU KHI ĐĂNG NHẬP) -->
        <div id="main-container" class="hidden h-full flex flex-col">
            <header class="p-4 bg-white shadow-md flex justify-between items-center z-20 shrink-0"><span class="text-xl font-bold text-gray-800">Velo-Tomo</span><button id="logout-button" class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-md text-sm">Đăng xuất</button></header>
            
            <main class="flex-grow overflow-y-auto bg-gray-50">
                <!-- CÁC VIEW CHÍNH -->
                <div id="main-view" class="h-full flex flex-col"><div class="p-4 bg-white border-b shrink-0"><p id="user-greeting" class="text-gray-700 font-medium text-lg"></p><div id="weekly-stats" class="mt-2 grid grid-cols-2 gap-4"></div></div><div id="main-map" class="flex-grow z-10"></div><div class="p-4 bg-white shrink-0"><button id="start-ride-button" class="w-full bg-green-500 hover:bg-green-600 text-white font-bold py-4 px-4 rounded-lg text-xl">Bắt đầu Chuyến đi</button></div></div>
                <div id="planner-view" class="hidden h-full flex flex-col bg-emerald-600 text-white"><div class="p-4 border-b border-emerald-500 shrink-0"><h2 class="text-xl font-bold">Lập kế hoạch</h2></div><div class="p-4 space-y-4 shrink-0"><div class="bg-emerald-700/50 p-4 rounded-lg shadow"><input id="planner-start" type="text" class="w-full p-2 border rounded-md mb-2 bg-emerald-50 text-gray-800" placeholder="Điểm bắt đầu" value="Bệnh viện Otokoyama, Yawata"><input id="planner-end" type="text" class="w-full p-2 border rounded-md" placeholder="Điểm kết thúc" value="Ga Kuzuha"><button id="plan-route-btn" class="mt-4 w-full bg-orange-500 text-white font-bold py-2 px-4 rounded-md hover:bg-orange-600">Lập kế hoạch</button><div id="premium-options" class="mt-4"><label class="font-semibold">Tùy chọn (Premium):</label><div class="flex space-x-2 mt-2"><button data-route-type="fastest" class="route-option-btn flex-1 p-2 border border-emerald-400 text-emerald-100 rounded-md bg-transparent">Nhanh nhất</button><button data-route-type="scenic" class="route-option-btn flex-1 p-2 border border-emerald-400 text-emerald-100 rounded-md bg-transparent">Cảnh đẹp</button><button data-route-type="flat" class="route-option-btn flex-1 p-2 border border-emerald-400 text-emerald-100 rounded-md bg-transparent">Ít dốc</button></div></div><div id="route-info" class="mt-4 text-sm font-medium"></div><div id="upgrade-prompt-planner" class="hidden mt-4 text-center p-4 bg-yellow-100 rounded-lg text-yellow-800"><p class="text-sm">Tính năng này chỉ dành cho thành viên Premium.</p><button class="upgrade-btn mt-2 bg-indigo-600 text-white font-bold py-2 px-4 rounded-md">Nâng cấp</button></div></div></div><div id="planner-map" class="flex-grow z-10 bg-gray-200"></div></div>
                <div id="challenges-view" class="hidden h-full flex flex-col bg-red-700 text-white"><div class="p-4 border-b border-red-600"><h2 class="text-xl font-bold">Thử thách</h2></div><div id="challenges-list" class="flex-grow p-4 space-y-4"></div></div>
                <div id="store-view" class="hidden h-full flex flex-col"><div class="p-4 bg-white border-b"><h2 class="text-xl font-bold">Cửa hàng</h2></div><div class="p-4"><div class="bg-gradient-to-r from-indigo-500 to-purple-500 text-white p-6 rounded-xl shadow-lg flex justify-between items-center"><div class="flex items-center space-x-4"> <img id="store-avatar" src="" alt="avatar" class="w-12 h-12 rounded-full border-2 border-white"> <div> <p class="font-medium">Điểm của bạn</p><p id="store-points" class="font-bold text-3xl">0</p></div></div><button id="view-profile-btn" class="bg-white/30 hover:bg-white/40 text-white font-bold py-2 px-4 rounded-lg text-sm">Hồ sơ & Thành tích</button></div></div><div id="store-list" class="flex-grow p-4 grid grid-cols-2 gap-4"></div></div>
                <div id="community-view" class="hidden h-full flex flex-col"><div class="p-4 bg-white border-b"><h2 class="text-xl font-bold">Bảng tin Cộng đồng</h2></div><div id="community-feed" class="flex-grow p-4 space-y-4"></div></div>
                <div id="history-view" class="hidden h-full flex flex-col"><div class="p-4 bg-white border-b"><h2 class="text-xl font-bold">Lịch sử Chuyến đi</h2></div><div id="history-list" class="flex-grow p-4 space-y-4"></div></div>
                <div id="profile-view" class="hidden h-full flex flex-col bg-emerald-600 text-white"><div class="p-4 border-b border-emerald-500 flex items-center"><button class="back-to-store-btn mr-4 text-2xl font-bold">←</button><h2 class="text-xl font-bold">Hồ sơ & Tiện ích</h2></div><div class="p-4 space-y-6"><div id="profile-status" class="bg-emerald-700/50 p-6 rounded-lg shadow text-center"></div><div class="bg-emerald-700/50 p-6 rounded-lg shadow"><h3 class="font-semibold text-lg mb-4">Thành tích</h3><div id="badge-grid" class="grid grid-cols-4 gap-4 text-center"></div></div>
                    <div class="bg-emerald-700/50 p-4 rounded-lg shadow divide-y divide-emerald-500/50">
                        <button id="analysis-btn" class="w-full text-left p-3 hover:bg-emerald-700 rounded-t-md">Phân tích Hiệu suất</button>
                        <button id="offline-maps-btn" class="w-full text-left p-3 hover:bg-emerald-700">Bản đồ Ngoại tuyến</button>
                        <button id="wallet-btn" class="w-full text-left p-3 hover:bg-emerald-700">Ví Đăng ký Xe</button>
                        <button id="rules-btn" class="w-full text-left p-3 hover:bg-emerald-700 rounded-b-md">Luật Giao thông</button>
                    </div>
                </div></div>
                <!-- CÁC VIEW PHỤ -->
                <div id="analysis-view" class="hidden h-full flex flex-col"><div class="p-4 bg-white border-b flex items-center shrink-0"><button class="back-to-profile-btn mr-4 text-2xl font-bold">←</button><h2 class="text-xl font-bold">Phân tích Hiệu suất</h2></div><div id="analysis-content" class="p-4 space-y-4 flex-grow overflow-y-auto"><div class="bg-white p-4 rounded-lg shadow"><h3 class="font-semibold mb-2">Tổng quãng đường (km)</h3><canvas id="distanceChart"></canvas></div></div><div id="upgrade-prompt-analysis" class="hidden m-4 text-center p-4 bg-yellow-100 rounded-lg"><p class="text-sm">Tính năng Premium.</p><button class="upgrade-btn mt-2 bg-indigo-600 text-white font-bold py-2 px-4 rounded-md">Nâng cấp</button></div></div>
                <div id="offline-maps-view" class="hidden h-full flex flex-col"><div class="p-4 bg-white border-b flex items-center shrink-0"><button class="back-to-profile-btn mr-4 text-2xl font-bold">←</button><h2 class="text-xl font-bold">Bản đồ Ngoại tuyến</h2></div><div id="offline-maps-content" class="p-4 space-y-4 flex-grow overflow-y-auto"><div id="offline-maps-list" class="bg-white divide-y rounded-lg shadow"></div></div><div id="upgrade-prompt-offline" class="hidden m-4 text-center p-4 bg-yellow-100 rounded-lg"><p class="text-sm">Tính năng Premium.</p><button class="upgrade-btn mt-2 bg-indigo-600 text-white font-bold py-2 px-4 rounded-md">Nâng cấp</button></div></div>
                <div id="wallet-view" class="hidden h-full flex flex-col"><div class="p-4 bg-white border-b flex items-center shrink-0"><button class="back-to-profile-btn mr-4 text-2xl font-bold">←</button><h2 class="text-xl font-bold">Ví Đăng ký Xe</h2></div><div class="p-4"><div class="bg-white p-6 rounded-lg shadow text-center"><h3 class="font-bold text-lg mb-4">Thẻ Đăng ký Phòng chống Trộm cắp</h3><img src="https://placehold.co/600x400/E2E8F0/4A5568?text=Ảnh+Giấy+Tờ" alt="Giấy tờ xe" class="w-full rounded-lg border-2 border-dashed"><div class="mt-4 text-left space-y-2"><p><strong>Chủ sở hữu:</strong> <span id="wallet-owner"></span></p><p><strong>Số đăng ký:</strong> <span class="font-mono bg-gray-200 px-2 py-1 rounded">Osaka-B 456789</span></p></div></div></div></div>
                <div id="rules-view" class="hidden h-full flex flex-col"><div class="p-4 bg-white border-b flex items-center shrink-0"><button class="back-to-profile-btn mr-4 text-2xl font-bold">←</button><h2 class="text-xl font-bold">Luật Giao thông</h2></div><div class="p-4 space-y-4"><div class="rule-item bg-white p-4 rounded-lg shadow"><h3 class="font-bold text-lg">1. Đi bên trái đường</h3><p class="mt-2 text-gray-700">Tại Nhật, xe đạp phải đi bên trái đường.</p></div><div class="rule-item bg-white p-4 rounded-lg shadow"><h3 class="font-bold text-lg">2. Đi trên vỉa hè</h3><p class="mt-2 text-gray-700">Chỉ được đi trên vỉa hè khi có biển báo cho phép.</p></div></div></div>
            </main>

            <footer class="grid grid-cols-5 p-2 bg-white border-t z-20 text-center shrink-0">
                 <button class="nav-button p-2 rounded-lg text-gray-500" data-target="main-view"><svg class="footer-icon mx-auto" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"/></svg><span class="text-xs">Chính</span></button>
                 <button class="nav-button p-2 rounded-lg text-gray-500" data-target="planner-view"><svg class="footer-icon mx-auto" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 20l-5.447-2.724A1 1 0 013 16.382V5.618a1 1 0 011.447-.894L9 7m0 13l6-3m-6 3V7m6 10l5.447 2.724A1 1 0 0021 16.382V5.618a1 1 0 00-1.447-.894L15 7m-6 13v-7m6 10V7"/></svg><span class="text-xs">Kế hoạch</span></button>
                 <button class="nav-button p-2 rounded-lg text-gray-500" data-target="challenges-view"><svg class="footer-icon mx-auto" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"/></svg><span class="text-xs">Thử thách</span></button>
                 <button class="nav-button p-2 rounded-lg text-gray-500" data-target="community-view"><svg class="footer-icon mx-auto" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"/></svg><span class="text-xs">Cộng đồng</span></button>
                 <button class="nav-button p-2 rounded-lg text-gray-500" data-target="store-view"><svg class="footer-icon mx-auto" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z"/></svg><span class="text-xs">Cửa hàng</span></button>
            </footer>
        </div>
        
        <!-- MÀN HÌNH ĐỘC LẬP -->
        <div id="live-tracking-screen" class="hidden h-full flex flex-col"><header class="p-4 bg-white shadow-md z-20"><h2 class="text-xl font-bold">Đang theo dõi...</h2></header><div id="live-map" class="flex-grow z-10"></div><footer class="p-4 bg-white shadow-top z-20"><div class="grid grid-cols-2 gap-4 mb-4 text-center"><div><p class="text-sm text-gray-500">Thời gian</p><p id="live-time" class="text-2xl font-bold">00:00:00</p></div><div><p class="text-sm text-gray-500">Quãng đường</p><p id="live-distance" class="text-2xl font-bold">0.00 km</p></div></div><button id="finish-ride-button" class="w-full bg-red-500 hover:bg-red-600 text-white font-bold py-4 px-4 rounded-lg text-lg">Kết thúc</button></footer></div>
        <div id="detail-screen" class="hidden h-full flex flex-col"><header class="p-4 bg-white shadow-md flex justify-between items-center z-20"><h2 class="text-xl font-bold">Chi tiết Chuyến đi</h2><button class="back-to-history-button bg-gray-200 text-gray-800 font-bold py-2 px-4 rounded-md">Quay lại</button></header><main class="flex-grow overflow-y-auto p-4"><div id="detail-map" class="h-64 rounded-lg z-10"></div><div class="mt-4 bg-white p-4 rounded-lg shadow"><h3 id="detail-date" class="text-lg font-semibold mb-2"></h3><div class="grid grid-cols-2 gap-4 text-center"><div><p class="text-sm text-gray-500">Thời gian</p><p id="detail-time" class="text-2xl font-bold"></p></div><div><p class="text-sm text-gray-500">Quãng đường</p><p id="detail-distance" class="text-2xl font-bold"></p></div></div></div><div id="ride-photos-container" class="mt-4 bg-white p-4 rounded-lg shadow"></div><div id="comment-container" class="mt-4 bg-white p-4 rounded-lg shadow"></div><div class="mt-4 bg-white p-4 rounded-lg shadow"><h3 class="font-semibold text-lg mb-2">Chia sẻ Thành tích</h3><div id="share-buttons" class="flex justify-center space-x-4"></div></div></main></div>

        <!-- MODALS -->
        <div id="premium-modal" class="hidden fixed inset-0 bg-gray-600 bg-opacity-50 h-full w-full z-50 flex items-center justify-center"><div class="relative mx-auto p-5 border w-full max-w-md shadow-lg rounded-md bg-white"><div class="text-center"><h3 class="text-lg font-medium text-gray-900">Nâng cấp lên Velo-Tomo Premium</h3><div class="mt-2 px-7 py-3"><p class="text-sm text-gray-500">Mở khóa các tính năng mạnh mẽ nhất!</p><p class="text-2xl font-bold my-4">500 JPY / tháng</p></div><div class="space-y-2"><button id="pay-button" class="px-4 py-2 bg-indigo-600 text-white w-full rounded-md">Thanh toán (Giả lập)</button><button id="cancel-upgrade-button" class="px-4 py-2 bg-gray-200 text-gray-800 w-full rounded-md">Để sau</button></div></div></div></div>
        <div id="alert-modal" class="hidden fixed inset-0 bg-gray-600 bg-opacity-50 h-full w-full z-50 flex items-center justify-center"><div class="relative mx-auto p-5 border w-full max-w-sm shadow-lg rounded-md bg-white"><div class="text-center"><div id="alert-modal-icon" class="mx-auto flex items-center justify-center h-12 w-12 rounded-full bg-green-100 mb-4"><svg class="h-6 w-6 text-green-600" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" /></svg></div><p id="alert-modal-text" class="text-sm text-gray-600"></p><button id="alert-modal-close" class="mt-4 px-4 py-2 bg-indigo-600 text-white w-full rounded-md">OK</button></div></div></div>
    </div>

    <script>
        // === STATE & MOCK DATA ===
        const state = { currentUser: null, mapInstances: {}, ridesHistory: [], currentRide: null, timers: {}, plannerLayers: [], currentPlanKey: 'otokoyama_kuzuha' };
        const users = { 'demo@velo-tomo.jp': { name: 'Hoàng Khoa', password: 'password123', points: 250, badges: [], isPremium: true, offlineMaps: [], challengesProgress: {}, avatar: 'https://placehold.co/40x40/a5b4fc/1e1b4b?text=Sếp' } };
        const otherUsers = { 'user2': { name: 'An', avatar: 'https://placehold.co/40x40/fca5a5/450a0a?text=A'} };
        const badges = { 'b01': { name: 'Người Mới Bắt Đầu', icon: 'https://placehold.co/80x80/ecfccb/84cc16?text=🚴'} };
        const challenges = [
            { id: 'c01', title: 'Chuyến đi đầu tiên', description: 'Hoàn thành chuyến đi đầu tiên của bạn.', type: 'ride_count', goal: 1, rewardPoints: 50, badgeId: 'b01'},
            { id: 'c02', title: '5km Khởi Động', description: 'Đạp xe tổng cộng 5km.', type: 'total_distance', goal: 5000, rewardPoints: 100, badgeId: null },
        ];
        const offlineMapRegions = [ { id: 'kanto', name: 'Kanto', size: '150MB' }, { id: 'kansai', name: 'Kansai', size: '120MB' } ];
        const storeItems = [ 
            { id: 'item01', name: 'Voucher Giảm 10% Y\'s Road', cost: 200 }, 
            { id: 'item02', name: 'Bình nước Velo-Tomo', cost: 500 },
            { id: 'item03', name: 'Mũ bảo hiểm tiêu chuẩn', cost: 1500 },
            { id: 'item04', name: 'Găng tay chuyên nghiệp', cost: 800 }
        ];
        const simulatedUserLocation = { lat: 34.8720, lng: 135.7100 }; // Otokoyama Hospital, Yawata
        const mockRouteSets = {
            'otokoyama_kuzuha': {
                center: [34.868, 135.701],
                startPopup: "Bệnh viện Otokoyama",
                endPopup: "Ga Kuzuha",
                fastest: { path: [[34.8720, 135.7100], [34.868, 135.700], [34.8645, 135.6925]], info: 'Nhanh nhất (4.2 km)' },
                scenic: { path: [[34.8720, 135.7100], [34.875, 135.705], [34.869, 135.695], [34.8645, 135.6925]], info: 'Cảnh đẹp (5.1 km, đi qua công viên)'},
                flat: { path: [[34.8720, 135.7100], [34.871, 135.712], [34.863, 135.698], [34.8645, 135.6925]], info: 'Ít dốc (4.5 km)'}
            },
            'shinjuku_skytree': {
                center: [35.695, 139.75],
                startPopup: "Shinjuku Gyoen",
                endPopup: "Tokyo Skytree",
                fastest: { path: [[35.685, 139.71], [35.69, 139.75], [35.71, 139.81]], info: 'Nhanh nhất (8.5 km)'},
                scenic: { path: [[35.685, 139.71], [35.705, 139.73], [35.712, 139.78], [35.71, 139.81]], info: 'Cảnh đẹp (9.2 km, ven sông)'},
                flat: { path: [[35.685, 139.71], [35.686, 139.74], [35.70, 139.79], [35.71, 139.81]], info: 'Ít dốc (8.8 km)'}
            }
        };
        const scenicSpot = { lat: 34.6879, lng: 135.5262, name: 'Lâu đài Osaka', photo: 'https://placehold.co/600x400/86efac/14532d?text=Osaka+Castle' };
        
        // === DOM ELEMENTS ===
        const topLevelScreens = { login: document.getElementById('auth-container'), main: document.getElementById('main-container'), liveTracking: document.getElementById('live-tracking-screen'), detail: document.getElementById('detail-screen') };
        const mainViews = { main: document.getElementById('main-view'), community: document.getElementById('community-view'), planner: document.getElementById('planner-view'), store: document.getElementById('store-view'), challenges: document.getElementById('challenges-view'), history: document.getElementById('history-view'), profile: document.getElementById('profile-view'), analysis: document.getElementById('analysis-view'), offlineMaps: document.getElementById('offline-maps-view'), wallet: document.getElementById('wallet-view'), rules: document.getElementById('rules-view') };

        // === NAVIGATION & UI ===
        function showTopLevelScreen(screenName) { Object.values(topLevelScreens).forEach(s => s.classList.add('hidden')); if (topLevelScreens[screenName]) topLevelScreens[screenName].classList.remove('hidden'); }
        function navigate(targetViewId) {
            Object.values(mainViews).forEach(v => v.classList.add('hidden'));
            const viewKey = targetViewId.replace('-view', '');
            if (mainViews[viewKey]) { mainViews[viewKey].classList.remove('hidden'); } else { mainViews['main'].classList.remove('hidden'); targetViewId = 'main-view'; } 
            
            if (viewKey === 'main') requestAnimationFrame(() => initMap('main-map', { center: [simulatedUserLocation.lat, simulatedUserLocation.lng], zoom: 13 }));
            if (viewKey === 'planner') initPlanner();
            if (viewKey === 'profile') renderProfile();
            if (viewKey === 'analysis') initAnalysis();
            if (viewKey === 'offlineMaps') initOfflineMaps();
            if (viewKey === 'wallet') renderWallet();
            if (viewKey === 'history') renderHistory();
            if (viewKey === 'challenges') renderChallenges();
            if (viewKey === 'store') renderStore();
            if (viewKey === 'community') renderCommunityFeed();
            
            document.querySelectorAll('.nav-button').forEach(b => { b.classList.toggle('active', b.dataset.target === targetViewId); b.classList.toggle('text-gray-500', b.dataset.target !== targetViewId); });
        }
        function showAlert(message) { document.getElementById('alert-modal-text').textContent = message; document.getElementById('alert-modal').classList.remove('hidden'); }

        // === MAP & CHART ===
        function initMap(mapId, view) { 
            if (state.mapInstances[mapId] && !document.body.contains(state.mapInstances[mapId].getContainer())) { state.mapInstances[mapId].remove(); state.mapInstances[mapId] = null; }
            if (state.mapInstances[mapId] === null || !state.mapInstances[mapId]) { const mapEl = document.getElementById(mapId); if (!mapEl) return; state.mapInstances[mapId] = L.map(mapEl).setView(view.center, view.zoom); L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { maxZoom: 19 }).addTo(state.mapInstances[mapId]); } 
        }
        function initChart() {
            const ctx = document.getElementById('distanceChart')?.getContext('2d');
            if(!ctx) return;
            if(state.distanceChart) state.distanceChart.destroy();
            state.distanceChart = new Chart(ctx, { type: 'bar', data: { labels: ['Tháng 3', 'Tháng 4', 'Tháng 5'], datasets: [{ label: 'km', data: [55, 78, 92], backgroundColor: 'rgba(79, 70, 229, 0.8)' }] }, options: { scales: { y: { beginAtZero: true } } } });
        }

        // === RIDE LOGIC ===
        function startRide() {
            state.currentRide = { id: Date.now(), startTime: new Date(), path: [], distance: 0, media: [], comments: [] };
            showTopLevelScreen('liveTracking');
            requestAnimationFrame(() => {
                initMap('live-map', { center: [simulatedUserLocation.lat, simulatedUserLocation.lng], zoom: 16 });
                if (state.mapInstances['live-map']) {
                    state.currentRide.polyline = L.polyline([], { color: 'red' }).addTo(state.mapInstances['live-map']);
                    let seconds = 0;
                    state.timers.rideTimer = setInterval(() => { seconds++; document.getElementById('live-time').textContent = new Date(seconds * 1000).toISOString().substr(11, 8); }, 1000);
                    const ridePath = [[34.8720, 135.7100], [scenicSpot.lat, scenicSpot.lng], [34.685, 135.505]];
                    let pathIndex = 0;
                    state.timers.gpsSimulator = setInterval(() => {
                        if (pathIndex >= ridePath.length) { finishRide(); return; }
                        const newPoint = ridePath[pathIndex];
                        if (state.currentRide) {
                           state.currentRide.path.push(newPoint);
                           if(state.currentRide.polyline) state.currentRide.polyline.addLatLng(newPoint);
                           if(state.mapInstances['live-map']) state.mapInstances['live-map'].panTo(newPoint);
                           if (state.currentRide.path.length > 1) { const lastPoint = state.currentRide.path[state.currentRide.path.length - 2]; state.currentRide.distance += L.latLng(lastPoint).distanceTo(L.latLng(newPoint)); }
                           document.getElementById('live-distance').textContent = `${(state.currentRide.distance / 1000).toFixed(2)} km`;
                           if (L.latLng(newPoint).distanceTo(L.latLng(scenicSpot.lat, scenicSpot.lng)) < 50) {
                               if(!state.currentRide.media.includes(scenicSpot.photo)) { state.currentRide.media.push(scenicSpot.photo); }
                           }
                        }
                        pathIndex++;
                    }, 2000);
                }
            });
        }
        function finishRide() {
            if(state.timers.rideTimer) clearInterval(state.timers.rideTimer);
            if(state.timers.gpsSimulator) clearInterval(state.timers.gpsSimulator);
            if (!state.currentRide) return;
            state.currentRide.endTime = new Date();
            state.ridesHistory.unshift({ ...state.currentRide });
            checkAndUpdateChallenges(state.currentUser, state.currentRide);
            state.currentRide = null;
            if (state.mapInstances['live-map']) { state.mapInstances['live-map'].remove(); state.mapInstances['live-map'] = null; }
            showTopLevelScreen('main');
            navigate('history-view');
        }

        // === PREMIUM & FEATURE INIT ===
        function upgradeToPremium() { state.currentUser.isPremium = true; document.getElementById('premium-modal').classList.add('hidden'); const currentView = document.querySelector('.nav-button.active').dataset.target; navigate(currentView); }
        function initPlanner() {
            requestAnimationFrame(() => {
                initMap('planner-map', { center: [34.868, 135.701], zoom: 15 });
                if (state.currentUser.isPremium) {
                    document.getElementById('premium-options').style.display = 'block';
                    document.getElementById('upgrade-prompt-planner').style.display = 'none';
                    const fastestBtn = document.querySelector('.route-option-btn[data-route-type="fastest"]');
                    if (!fastestBtn.classList.contains('active')) { document.querySelectorAll('.route-option-btn').forEach(b => b.classList.remove('active')); fastestBtn.classList.add('active'); }
                    drawPlannerRoute(state.currentPlanKey, 'fastest');
                } else {
                     document.getElementById('premium-options').style.display = 'none';
                    document.getElementById('upgrade-prompt-planner').style.display = 'block';
                }
            });
        }
        function drawPlannerRoute(planKey, type) {
            const map = state.mapInstances['planner-map'];
            if (!map) { setTimeout(() => drawPlannerRoute(planKey, type), 100); return; };
            if(state.plannerLayers) { state.plannerLayers.forEach(layer => map.removeLayer(layer)); state.plannerLayers = []; }
            const routeSet = mockRouteSets[planKey];
            const routeData = routeSet[type];
            if (!routeData) return;
            const routeCoords = routeData.path;
            const colors = { fastest: '#3b82f6', scenic: '#16a34a', flat: '#6b7280' };
            document.getElementById('route-info').textContent = `Đang hiển thị: ${routeData.info}`;
            const routeLine = L.polyline(routeCoords, { color: colors[type], weight: 6, opacity: 0.8 }).addTo(map);
            const startMarker = L.marker(routeCoords[0]).addTo(map).bindPopup(routeSet.startPopup);
            const endMarker = L.marker(routeCoords[routeCoords.length - 1]).addTo(map).bindPopup(routeSet.endPopup);
            state.plannerLayers.push(routeLine, startMarker, endMarker);
            map.fitBounds(routeLine.getBounds().pad(0.1));
        }
        function planNewRoute() {
            const startInput = document.getElementById('planner-start').value;
            const endInput = document.getElementById('planner-end').value;
            if (startInput.toLowerCase().includes('shinjuku') && endInput.toLowerCase().includes('skytree')) {
                state.currentPlanKey = 'shinjuku_skytree';
            } else {
                state.currentPlanKey = 'otokoyama_kuzuha';
            }
            initPlanner();
        }
        function initAnalysis() { const isPremium = state.currentUser.isPremium; document.getElementById('analysis-content').style.display = isPremium ? 'block' : 'none'; document.getElementById('upgrade-prompt-analysis').style.display = isPremium ? 'none' : 'block'; if(isPremium) setTimeout(initChart, 100); }
        function initOfflineMaps() { const isPremium = state.currentUser.isPremium; document.getElementById('offline-maps-content').style.display = isPremium ? 'block' : 'none'; document.getElementById('upgrade-prompt-offline').style.display = isPremium ? 'none' : 'block'; if(isPremium) renderOfflineMapsList(); }
        
        // === UI RENDERING ===
        function renderProfile() { if (!state.currentUser) return; const statusDiv = document.getElementById('profile-status'); if (state.currentUser.isPremium) { statusDiv.innerHTML = `<p class="font-semibold text-lg text-yellow-500">⭐ Thành viên Premium</p>`; } else { statusDiv.innerHTML = `<p class="font-semibold text-lg">Thành viên Thường</p><button class="upgrade-btn mt-2 text-sm text-indigo-600 hover:underline">Nâng cấp</button>`; } const grid = document.getElementById('badge-grid'); grid.innerHTML = ''; state.currentUser.badges.forEach(badgeId => { const badge = badges[badgeId]; if (!badge) return; const el = document.createElement('div'); el.innerHTML = `<img src="${badge.icon}" alt="${badge.name}" class="w-16 h-16 rounded-full mx-auto"><p class="text-xs mt-2">${badge.name}</p>`; grid.appendChild(el); }); }
        function renderOfflineMapsList() { const listEl = document.getElementById('offline-maps-list'); listEl.innerHTML = ''; offlineMapRegions.forEach(region => { const isDownloaded = state.currentUser.offlineMaps.includes(region.id); const el = document.createElement('div'); el.className = 'flex justify-between items-center p-4'; el.innerHTML = `<div><p class="font-semibold">${region.name}</p><p class="text-sm text-gray-500">${region.size}</p></div><button data-region-id="${region.id}" class="offline-dl-btn px-4 py-2 text-sm font-medium rounded-md ${isDownloaded ? 'bg-gray-200 text-gray-500' : 'bg-blue-500 text-white'}">${isDownloaded ? 'Đã tải' : 'Tải về'}</button>`; listEl.appendChild(el); }); }
        function renderWallet() { document.getElementById('wallet-owner').textContent = state.currentUser.name; }
        function renderHistory() { const listEl = document.getElementById('history-list'); listEl.innerHTML = state.ridesHistory.length === 0 ? `<p class="text-center text-gray-500 p-4">Chưa có chuyến đi nào.</p>` : ''; state.ridesHistory.forEach(ride => { const duration = Math.round((ride.endTime - ride.startTime) / 60000); const el = document.createElement('div'); el.className = 'bg-white p-4 rounded-lg shadow cursor-pointer hover:bg-gray-50'; el.innerHTML = `<p class="font-bold">${ride.startTime.toLocaleString('vi-VN')}</p><p class="text-sm text-gray-600">Quãng đường: ${(ride.distance / 1000).toFixed(2)} km | Thời gian: ${duration} phút</p>`; el.addEventListener('click', () => showRideDetail(ride)); listEl.appendChild(el); }); }
        function showRideDetail(ride) { showTopLevelScreen('detail'); document.getElementById('detail-date').textContent = ride.startTime.toLocaleString('vi-VN'); const duration = Math.round((ride.endTime - ride.startTime) / 60000); document.getElementById('detail-time').textContent = `${duration} phút`; document.getElementById('detail-distance').textContent = `${(ride.distance / 1000).toFixed(2)} km`; renderRideMedia(ride); renderComments(ride); renderShareButtons(); requestAnimationFrame(() => { initMap('detail-map', { center: ride.path[0] || [simulatedUserLocation.lat, simulatedUserLocation.lng], zoom: 15 }); if(state.mapInstances['detail-map'] && ride.path.length > 0) { const ridePolyline = L.polyline(ride.path, { color: 'blue' }).addTo(state.mapInstances['detail-map']); state.mapInstances['detail-map'].fitBounds(ridePolyline.getBounds()); } }); }
        function renderChallenges() { const listEl = document.getElementById('challenges-list'); listEl.innerHTML = ''; challenges.forEach(c => { const progress = state.currentUser.challengesProgress[c.id] || {current: 0, completed: false}; const isCompleted = progress.completed; let currentVal = progress.current; if (c.type === 'total_distance') {currentVal = state.ridesHistory.reduce((acc, r) => acc + r.distance, 0);} else if(c.type === 'ride_count') { currentVal = state.ridesHistory.length;} const percent = isCompleted ? 100 : Math.min((currentVal / c.goal) * 100, 100); const el = document.createElement('div'); el.className = `bg-red-800/50 p-4 rounded-lg shadow`; el.innerHTML = `<div class="flex justify-between items-center mb-2"><h3 class="font-bold text-lg">${c.title}</h3><span class="text-sm font-semibold text-orange-400">+${c.rewardPoints} Điểm</span></div><p class="text-sm text-red-200 mb-3">${c.description}</p><div class="w-full bg-green-900 rounded-full h-4"><div class="bg-orange-500 h-4 rounded-full progress-bar-fill" style="width: ${percent}%"></div></div><p class="text-right text-xs mt-1">${isCompleted ? 'Đã hoàn thành' : `${c.type === 'total_distance' ? (currentVal/1000).toFixed(1) : currentVal} / ${c.type === 'total_distance' ? c.goal/1000 : c.goal}`}</p>`; listEl.appendChild(el); });}
        function renderStore() { document.getElementById('store-points').textContent = state.currentUser.points; const listEl = document.getElementById('store-list'); listEl.innerHTML = ''; storeItems.forEach(item => { const el = document.createElement('div'); el.className = 'bg-white p-4 rounded-lg shadow flex justify-between items-center'; el.innerHTML = `<div><p class="font-bold">${item.name}</p><p class="text-sm text-indigo-600">${item.cost} điểm</p></div><button data-item-id="${item.id}" data-item-cost="${item.cost}" class="redeem-btn px-4 py-2 text-sm font-medium rounded-md bg-green-500 text-white">Đổi</button>`; listEl.appendChild(el); });}
        function renderShareButtons() { const container = document.getElementById('share-buttons'); container.innerHTML = `<button class="share-btn p-2 bg-blue-600 text-white rounded-full">F</button><button class="share-btn p-2 bg-black text-white rounded-full">X</button><button class="share-btn p-2 bg-green-500 text-white rounded-full">L</button>`;}
        function renderCommunityFeed() { const feedEl = document.getElementById('community-feed'); feedEl.innerHTML = state.ridesHistory.length === 0 ? `<p class="text-center text-gray-500 p-4">Chưa có hoạt động nào nổi bật.</p>` : ''; [...state.ridesHistory].reverse().forEach(ride => { const duration = Math.round((ride.endTime - ride.startTime) / 60000); const el = document.createElement('div'); el.className = 'bg-white rounded-lg shadow'; el.innerHTML = `<div class="p-4"><p class="font-bold">Sếp vừa hoàn thành chuyến đi!</p><p class="text-xs text-gray-500">${ride.startTime.toLocaleString('vi-VN')}</p></div><img src="${ride.media[0]}" alt="Ride Photo" class="w-full h-64 object-cover"><div class="p-4"><p><b>${(ride.distance/1000).toFixed(2)} km</b> trong <b>${duration} phút</b>. Chuyến đi tuyệt vời!</p><div class="text-blue-600 text-sm mt-2 cursor-pointer" onclick="showRideDetailFromFeed(${ride.id})">Xem chi tiết và bình luận...</div></div>`; feedEl.appendChild(el); });}
        function renderRideMedia(ride) {
            const container = document.getElementById('ride-photos-container');
            let imageGridHTML = '';
            if (ride.media && ride.media.length > 0) {
                ride.media.forEach(imgUrl => { imageGridHTML += `<div><img src="${imgUrl}" class="w-full h-24 object-cover rounded-md"></div>`; });
            }
            container.innerHTML = `<h3 class="font-semibold text-lg mb-2">Hình ảnh Chuyến đi</h3><div id="photo-grid" class="grid grid-cols-3 gap-2">${imageGridHTML}</div><button id="add-photo-btn" class="mt-2 w-full text-sm p-2 bg-gray-200 rounded-md hover:bg-gray-300">Thêm ảnh (thủ công)</button>`;
            document.getElementById('add-photo-btn').addEventListener('click', () => {
                ride.media.push('https://placehold.co/600x400/fbbf24/78350f?text=Selfie!');
                renderRideMedia(ride);
            });
        }
        function renderComments(ride) { const container = document.getElementById('comment-container'); container.innerHTML = '<h3 class="font-semibold text-lg mb-4">Bình luận</h3><div id="comment-list" class="space-y-4"></div>'; const listEl = document.getElementById('comment-list'); if(!ride.comments || ride.comments.length === 0) { listEl.innerHTML = '<p class="text-sm text-gray-500">Chưa có bình luận nào.</p>'; } else { ride.comments.forEach(comment => { const user = comment.userId === 'currentUser' ? state.currentUser : otherUsers[comment.userId]; listEl.innerHTML += `<div class="flex items-start space-x-3"><img src="${user.avatar}" class="w-10 h-10 rounded-full"><div class="bg-gray-100 p-3 rounded-lg flex-1"><p class="font-semibold text-sm">${user.name}</p><p class="text-sm">${comment.text}</p></div></div>`; }); } container.innerHTML += `<form id="comment-form" class="mt-4 flex space-x-2"><input id="comment-input" class="flex-grow border rounded-md p-2" placeholder="Viết bình luận..."><button type="submit" class="bg-indigo-600 text-white px-4 rounded-md font-semibold">Gửi</button></form>`; document.getElementById('comment-form').addEventListener('submit', (e) => { e.preventDefault(); addComment(ride); }); }
        function addComment(ride) { const input = document.getElementById('comment-input'); if(input.value.trim() === '') return; ride.comments.push({ userId: 'currentUser', text: input.value }); renderComments(ride); }
        function showRideDetailFromFeed(rideId) { const ride = state.ridesHistory.find(r => r.id === rideId); if(ride) showRideDetail(ride); }

        // === GAMIFICATION & STORE LOGIC ===
        function checkAndUpdateChallenges(user, completedRide) {
            challenges.forEach(challenge => {
                let progress = user.challengesProgress[challenge.id] || { current: 0, completed: false };
                if (progress.completed) return;
                
                let goalMet = false;
                if (challenge.type === 'ride_count') {
                    if (state.ridesHistory.length >= challenge.goal) goalMet = true;
                }
                if (challenge.type === 'total_distance') {
                    const totalDistance = state.ridesHistory.reduce((acc, r) => acc + r.distance, 0);
                    if (totalDistance >= challenge.goal) goalMet = true;
                }

                if (goalMet) {
                    completeChallenge(user, challenge);
                }
            });
        }
        function completeChallenge(user, challenge) {
            user.challengesProgress[challenge.id] = { current: challenge.goal, completed: true };
            user.points += challenge.rewardPoints;
            if (challenge.badgeId && !user.badges.includes(challenge.badgeId)) {
                user.badges.push(challenge.badgeId);
            }
            showAlert(`Chúc mừng! Bạn đã hoàn thành: "${challenge.title}"`);
        }
        function redeemItem(itemId, cost) { if (state.currentUser.points >= cost) { state.currentUser.points -= cost; showAlert(`Bạn đã đổi thành công vật phẩm!`); renderStore(); } else { showAlert(`Bạn không đủ điểm để đổi vật phẩm này.`); } }

        // === AUTH LOGIC ===
        function login() { const user = users[document.getElementById('login-email').value]; if (user && user.password === document.getElementById('login-password').value) { state.currentUser = user; document.getElementById('user-greeting').textContent = `Xin chào, ${state.currentUser.name}!`; showTopLevelScreen('main'); navigate('main-view'); } else { document.getElementById('login-error').textContent = 'Email hoặc mật khẩu không chính xác.'; } }
        function logout() { state.currentUser = null; state.ridesHistory = []; Object.keys(state.mapInstances).forEach(key => { if (state.mapInstances[key]) { state.mapInstances[key].remove(); state.mapInstances[key] = null; } }); showTopLevelScreen('login'); }
        
        // === EVENT LISTENERS ===
        function setupEventListeners() {
            document.getElementById('login-button').addEventListener('click', login);
            document.getElementById('google-login-button').addEventListener('click', login); // Simulate Google login
            document.getElementById('email-login-toggle').addEventListener('click', (e) => {
                e.preventDefault();
                document.getElementById('email-form').classList.toggle('hidden');
            });
            document.getElementById('logout-button').addEventListener('click', logout);
            document.getElementById('start-ride-button').addEventListener('click', startRide);
            document.getElementById('finish-ride-button').addEventListener('click', finishRide);
            document.querySelectorAll('.nav-button').forEach(button => button.addEventListener('click', () => navigate(button.dataset.target)));
            document.querySelectorAll('.upgrade-btn').forEach(btn => btn.addEventListener('click', () => document.getElementById('premium-modal').classList.remove('hidden')));
            document.getElementById('cancel-upgrade-button').addEventListener('click', () => document.getElementById('premium-modal').classList.add('hidden'));
            document.getElementById('pay-button').addEventListener('click', upgradeToPremium);
            document.querySelectorAll('.back-to-history-button').forEach(btn => btn.addEventListener('click', () => { showTopLevelScreen('main'); navigate('history-view'); }));
            document.getElementById('alert-modal-close').addEventListener('click', () => document.getElementById('alert-modal').classList.add('hidden'));
            document.getElementById('analysis-btn').addEventListener('click', () => navigate('analysis-view'));
            document.getElementById('offline-maps-btn').addEventListener('click', () => navigate('offline-maps-view'));
            document.getElementById('wallet-btn').addEventListener('click', () => navigate('wallet-view'));
            document.getElementById('rules-btn').addEventListener('click', () => navigate('rules-view'));
            document.getElementById('view-profile-btn').addEventListener('click', () => navigate('profile-view'));
            document.querySelectorAll('.back-to-profile-btn').forEach(btn => btn.addEventListener('click', () => navigate('profile-view')));
            document.querySelectorAll('.back-to-store-btn').forEach(btn => btn.addEventListener('click', () => navigate('store-view')));
            document.getElementById('store-list').addEventListener('click', (e) => { if(e.target.classList.contains('redeem-btn')) { redeemItem(e.target.dataset.itemId, parseInt(e.target.dataset.itemCost)); }});
            document.querySelectorAll('.route-option-btn').forEach(btn => { btn.addEventListener('click', () => { if (!state.currentUser.isPremium) { document.getElementById('premium-modal').classList.remove('hidden'); return; } document.querySelectorAll('.route-option-btn').forEach(b => b.classList.remove('active')); btn.classList.add('active'); drawPlannerRoute(state.currentPlanKey, btn.dataset.routeType); }); });
            document.getElementById('plan-route-btn').addEventListener('click', planNewRoute);
        }

        // === INITIALIZATION ===
        document.addEventListener('DOMContentLoaded', () => { setupEventListeners(); showTopLevelScreen('login'); });
    </script>
</body>
</html>

