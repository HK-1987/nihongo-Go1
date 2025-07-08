
import React, { useState, useEffect, useCallback } from 'react';
import { initializeApp } from 'firebase/app';
import { getAuth, signInAnonymously, onAuthStateChanged } from 'firebase/auth';
import { getFirestore, doc, setDoc, getDoc, onSnapshot, updateDoc, increment } from 'firebase/firestore';

// --- BIẾN MÔI TRƯỜNG (DO CANVAS CUNG CẤP) ---
const firebaseConfig = typeof __firebase_config !== 'undefined'
  ? JSON.parse(__firebase_config)
  : { apiKey: "YOUR_API_KEY", authDomain: "YOUR_AUTH_DOMAIN", projectId: "YOUR_PROJECT_ID" };

const appId = typeof __app_id !== 'undefined' ? __app_id : 'nihongo-go-demo';

// --- KHỞI TẠO FIREBASE ---
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);

// --- NGÂN HÀNG DỮ LIỆU N5 (ĐÃ MỞ RỘNG TOÀN DIỆN) ---
const N5Database = {
    srsItems: [
        { id: 'kanji_1', type: 'Kanji', content: '日', meaning: 'Nhật (Ngày)', reading: 'On: にち, じつ / Kun: ひ, -か', examples: [{ word: '日本', reading: 'にほん', meaning: 'Nhật Bản' }, { word: '今日', reading: 'きょう', meaning: 'Hôm nay' }] },
        { id: 'kanji_2', type: 'Kanji', content: '一', meaning: 'Nhất (Một)', reading: 'On: いち / Kun: ひと', examples: [{ word: '一番', reading: 'いちばん', meaning: 'Số một' }, { word: '一つ', reading: 'ひとつ', meaning: 'Một cái' }] },
        { id: 'kanji_3', type: 'Kanji', content: '国', meaning: 'Quốc (Đất nước)', reading: 'On: こく / Kun: くに', examples: [{ word: '国', reading: 'くに', meaning: 'Đất nước' }, { word: '中国人', reading: 'ちゅうごくじん', meaning: 'Người Trung Quốc' }] },
        { id: 'kanji_4', type: 'Kanji', content: '人', meaning: 'Nhân (Người)', reading: 'On: じん, にん / Kun: ひと', examples: [{ word: '日本人', reading: 'にほんじん', meaning: 'Người Nhật' }, { word: '三人', reading: 'さんにん', meaning: 'Ba người' }] },
        { id: 'kanji_5', type: 'Kanji', content: '年', meaning: 'Niên (Năm)', reading: 'On: ねん', examples: [{ word: '今年', reading: 'ことし', meaning: 'Năm nay' }, { word: '来年', reading: 'らいねん', meaning: 'Năm sau' }] },
        { id: 'kanji_6', type: 'Kanji', content: '大', meaning: 'Đại (Lớn)', reading: 'On: だい, たい / Kun: おお', examples: [{ word: '大学', reading: 'だいがく', meaning: 'Đại học' }, { word: '大きい', reading: 'おおきい', meaning: 'To, lớn' }] },
        { id: 'kanji_7', type: 'Kanji', content: '十', meaning: 'Thập (Mười)', reading: 'On: じゅう / Kun: とお', examples: [{ word: '十', reading: 'じゅう', meaning: 'Mười' }, { word: '十日', reading: 'とおか', meaning: 'Ngày 10' }] },
        { id: 'kanji_8', type: 'Kanji', content: '二', meaning: 'Nhị (Hai)', reading: 'On: に / Kun: ふた', examples: [{ word: '二つ', reading: 'ふたつ', meaning: 'Hai cái' }, { word: '二月', reading: 'にがつ', meaning: 'Tháng 2' }] },
        { id: 'kanji_9', type: 'Kanji', content: '本', meaning: 'Bản (Sách)', reading: 'On: ほん / Kun: もと', examples: [{ word: '本', reading: 'ほん', meaning: 'Sách' }, { word: '山本', reading: 'やまもと', meaning: 'Yamamoto (tên người)' }] },
        { id: 'kanji_10', type: 'Kanji', content: '中', meaning: 'Trung (Trong)', reading: 'On: ちゅう / Kun: なか', examples: [{ word: '中国', reading: 'ちゅうごく', meaning: 'Trung Quốc' }, { word: '田中', reading: 'たなか', meaning: 'Tanaka (tên người)' }] },
        { id: 'kanji_11', type: 'Kanji', content: '長', meaning: 'Trường (Dài)', reading: 'On: ちょう / Kun: なが.い', examples: [{ word: '社長', reading: 'しゃちょう', meaning: 'Giám đốc' }, { word: '長い', reading: 'ながい', meaning: 'Dài' }] },
        { id: 'kanji_12', type: 'Kanji', content: '出', meaning: 'Xuất (Ra)', reading: 'On: しゅつ / Kun: で.る, だ.す', examples: [{ word: '出る', reading: 'でる', meaning: 'Đi ra' }, { word: '出口', reading: 'でぐち', meaning: 'Lối ra' }] },
        { id: 'kanji_13', type: 'Kanji', content: '三', meaning: 'Tam (Ba)', reading: 'On: さん / Kun: みっ.つ', examples: [{ word: '三つ', reading: 'みっつ', meaning: 'Ba cái' }, { word: '三月', reading: 'さんがつ', meaning: 'Tháng 3' }] },
        { id: 'kanji_14', type: 'Kanji', content: '時', meaning: 'Thời (Giờ)', reading: 'On: じ / Kun: とき', examples: [{ word: '時間', reading: 'じかん', meaning: 'Thời gian' }, { word: '時々', reading: 'ときどき', meaning: 'Thỉnh thoảng' }] },
        { id: 'kanji_15', type: 'Kanji', content: '行', meaning: 'Hành (Đi)', reading: 'On: こう, ぎょう / Kun: い.く', examples: [{ word: '行く', reading: 'いく', meaning: 'Đi' }, { word: '銀行', reading: 'ぎんこう', meaning: 'Ngân hàng' }] },
        { id: 'vocab_1', type: 'Từ vựng', content: '私', reading: 'わたし', meaning: 'Tôi', example: '私は学生です。' },
        { id: 'vocab_2', type: 'Từ vựng', content: '学生', reading: 'がくせい', meaning: 'Học sinh, sinh viên', example: '彼は学生です。' },
        { id: 'vocab_11', type: 'Từ vựng', content: '行く', reading: 'いく', meaning: 'Đi', example: '学校へ行きます。' },
        { id: 'vocab_12', type: 'Từ vựng', content: '見る', reading: 'みる', meaning: 'Nhìn, xem', example: 'テレビを見ます。' },
        { id: 'vocab_13', type: 'Từ vựng', content: '食べる', reading: 'たべる', meaning: 'Ăn', example: 'ご飯を食べます。' },
        { id: 'vocab_14', type: 'Từ vựng', content: '飲む', reading: 'のむ', meaning: 'Uống', example: '水を飲みます。' },
        { id: 'grammar_1', type: 'Ngữ pháp', content: 'N1 は N2 です', meaning: 'N1 là N2', explanation: 'Mẫu câu khẳng định cơ bản, dùng để giới thiệu hoặc mô tả.', example: '私 は 学生 です。(Tôi là học sinh.)' },
        { id: 'grammar_2', type: 'Ngữ pháp', content: 'N1 は N2 じゃありません', meaning: 'N1 không phải là N2', explanation: 'Mẫu câu phủ định của 「です」.', example: '山田さんは学生じゃありません。(Anh Yamada không phải là học sinh.)' },
        { id: 'grammar_8', type: 'Ngữ pháp', content: 'N を Vます', meaning: 'Làm V với N', explanation: 'Trợ từ を chỉ đối tượng trực tiếp của hành động.', example: 'パンを食べます。(Tôi ăn bánh mì.)' },
        { id: 'grammar_9', type: 'Ngữ pháp', content: 'N に/へ行きます', meaning: 'Đi đến N', explanation: 'Chỉ phương hướng của hành động di chuyển. に và へ có thể thay thế cho nhau.', example: '日本へ行きます。(Tôi đi Nhật.)' },
        { id: 'grammar_10', type: 'Ngữ pháp', content: 'V-ませんか', meaning: 'Cùng làm V không?', explanation: 'Dùng để mời hoặc rủ rê ai đó cùng làm gì một cách lịch sự.', example: '一緒に映画を見ませんか。(Cùng đi xem phim không?)' },
    ],
    assessmentQuestions: [
        { id: 'q_kanji_1', type: 'kanji', difficulty: -1.5, question: 'Chọn cách đọc cho 「日」 trong 「日本」', options: ['ひ', 'に', 'つき', 'か'], answer: 'に' },
        { id: 'q_kanji_2', type: 'kanji', difficulty: -1.0, question: '「火」 có nghĩa là gì?', options: ['Nước', 'Lửa', 'Cây', 'Đất'], answer: 'Lửa' },
        { id: 'q_kanji_3', type: 'kanji', difficulty: 0.0, question: '「人」 đọc là gì?', options: ['ひと', 'き', 'かね', 'みず'], answer: 'ひと' },
        { id: 'q_kanji_4', type: 'kanji', difficulty: 0.5, question: '「大きい」 có chữ Hán là gì?', options: ['大', '太', '犬', '天'], answer: '大' },
        { id: 'q_vocab_1', type: 'vocab', difficulty: -0.5, question: 'Chọn cách đọc cho 「学生」', options: ['がくせい', 'がっこう', 'せんせい', 'にほん'], answer: 'がくせい' },
        { id: 'q_vocab_2', type: 'vocab', difficulty: 0.5, question: '「Uống」 trong tiếng Nhật là gì?', options: ['たべます', 'のみます', 'いきます', 'まなびます'], answer: 'のみます' },
        { id: 'q_vocab_3', type: 'vocab', difficulty: 0.2, question: '「Cái này」 trong tiếng Nhật là gì?', options: ['これ', 'それ', 'あれ', 'どれ'], answer: 'これ' },
        { id: 'q_vocab_4', type: 'vocab', difficulty: -0.8, question: '「ねこ」 nghĩa là gì?', options: ['Chó', 'Cá', 'Mèo', 'Chim'], answer: 'Mèo' },
        { id: 'q_vocab_5', type: 'vocab', difficulty: 1.0, question: '「おいしい」 nghĩa là gì?', options: ['Ngon', 'Dở', 'Đắt', 'Rẻ'], answer: 'Ngon' },
        { id: 'q_vocab_6', type: 'vocab', difficulty: 1.2, question: '「買う」 đọc là gì?', options: ['かう', 'うる', 'のむ', 'みる'], answer: 'かう' },
        { id: 'q_grammar_1', type: 'grammar', difficulty: 0.0, question: 'Điền vào chỗ trống: 田中さんは先生です。山田さん___先生です。', options: ['は', 'が', 'の', 'も'], answer: 'も' },
        { id: 'q_grammar_2', type: 'grammar', difficulty: 1.0, question: 'Dịch sang tiếng Nhật: "Đi đến trường học."', options: ['学校で食べます', '学校に行きます', '学校は先生です', '学校も好きです'], answer: '学校に行きます' },
        { id: 'q_grammar_3', type: 'grammar', difficulty: 2.0, question: 'Điền vào chỗ trống: 私___学生です。', options: ['を', 'が', 'は', 'も'], answer: 'は' },
        { id: 'q_grammar_4', type: 'grammar', difficulty: 0.8, question: 'Chọn trợ từ đúng: 私はバス___学校へ行きます。', options: ['で', 'に', 'を', 'と'], answer: 'で' },
        { id: 'q_grammar_5', type: 'grammar', difficulty: 1.5, question: '「Cùng đi xem phim không?」 là câu nào?', options: ['映画を見ますか。', '映画を見ませんか。', '映画を見たいです。', '映画を見ました。'], answer: '映画を見ませんか。' },
        { id: 'q_grammar_6', type: 'grammar', difficulty: 1.8, question: 'Chọn dạng đúng của tính từ: きのうは___です。', options: ['あついです', 'あつくないです', 'あつかったです', 'あつくないでした'], answer: 'あつかったです' },
        { id: 'q_reading_1', type: 'reading', difficulty: 1.5, readingText: '私 は マイク・ミラー です。IMC の 社員 です。サントスさん も IMC の 社員 です。サントスさん は ブラジル人 です。', question: 'サントスさん は どこの 社員 ですか。(Anh Santos là nhân viên của công ty nào?)', options: ['IMC', 'ブラジル', 'わかりません (Không biết)', 'マイク・ミラー'], answer: 'IMC' },
        { id: 'q_reading_2', type: 'reading', difficulty: 2.0, readingText: 'きのう、私は友達と京都へ行きました。京都はとてもきれいでした。たくさん写真を撮りました。', question: 'きのう、何をしましたか。(Hôm qua đã làm gì?)', options: ['京都へ行きました', '写真を撮りませんでした', '友達と会いませんでした', '京都はきれいじゃありませんでした'], answer: '京都へ行きました' },
        { id: 'q_listening_1', type: 'listening', difficulty: 1.0, question: 'Bạn nghe thấy gì?', audioText: 'おはようございます', options: ['こんにちは', 'さようなら', 'おやすみなさい', 'おはようございます'], answer: 'おはようございます' },
        { id: 'q_listening_2', type: 'listening', difficulty: 1.2, question: 'Bạn nghe thấy gì?', audioText: 'ありがとうございます', options: ['すみません', 'ありがとうございます', 'ごめんなさい', 'どうぞ'], answer: 'ありがとうございます' },
        { id: 'q_listening_3', type: 'listening', difficulty: 1.8, question: 'Bạn nghe thấy câu hỏi gì?', audioText: 'お名前は何ですか', options: ['お元気ですか', '何歳ですか', 'お名前は何ですか', 'ご出身はどちらですか'], answer: 'お名前は何ですか' },
        { id: 'q_listening_4', type: 'listening', difficulty: 2.0, question: 'Bạn nghe thấy câu trả lời nào cho "これは何ですか。"?', audioText: 'それは時計です', options: ['はい、そうです', 'あれは椅子です', 'いいえ、違います', 'それは時計です'], answer: 'それは時計です' }
    ]
};

// --- DỮ LIỆU GIẢ LẬP CHO BẠN BÈ & BẢNG XẾP HẠNG ---
const mockFriendsData = [
    { name: 'Minh Anh', xp: 18500, avatar: '👩‍🎓' },
    { name: 'Quốc Hưng', xp: 16200, avatar: '👨‍💼' },
    { name: 'Lan Chi', xp: 14800, avatar: '👩‍💻' },
];

// --- CÁC THÀNH PHẦN GIAO DIỆN (ICONS) ---
const BookOpen = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"></path><path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"></path></svg>;
const Target = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><circle cx="12" cy="12" r="10"></circle><circle cx="12" cy="12" r="6"></circle><circle cx="12" cy="12" r="2"></circle></svg>;
const Flame = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M8.5 14.5A2.5 2.5 0 0 0 11 12c0-1.38-.5-2-1-3-1.072-2.143-.224-4.054 2-6 .5 2.5 2 4.9 4 6.5 2 1.6 3 3.5 3 5.5a7 7 0 1 1-14 0c0-1.153.433-2.294 1-3a2.5 2.5 0 0 0 2.5 2.5z"></path></svg>;
const Gem = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M6 3h12l4 6-10 13L2 9Z"></path><path d="M12 22V9"></path><path d="m3.29 9 8.71 13 8.71-13"></path><path d="M2 9h20"></path></svg>;
const Award = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><circle cx="12" cy="8" r="6"></circle><path d="M15.477 12.89 17 22l-5-3-5 3 1.523-9.11"></path></svg>;
const Volume2 = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"></polygon><path d="M15.54 8.46a5 5 0 0 1 0 7.07"></path><path d="M19.07 4.93a10 10 0 0 1 0 14.14"></path></svg>;
const FileText = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M14.5 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V7.5L14.5 2z"></path><polyline points="14 2 14 8 20 8"></polyline><line x1="16" y1="13" x2="8" y2="13"></line><line x1="16" y1="17" x2="8" y2="17"></line><line x1="10" y1="9" x2="8" y2="9"></line></svg>;
const ShoppingCart = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><circle cx="9" cy="21" r="1"></circle><circle cx="20" cy="21" r="1"></circle><path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"></path></svg>;
const Sun = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><circle cx="12" cy="12" r="5"></circle><line x1="12" y1="1" x2="12" y2="3"></line><line x1="12" y1="21" x2="12" y2="23"></line><line x1="4.22" y1="4.22" x2="5.64" y2="5.64"></line><line x1="18.36" y1="18.36" x2="19.78" y2="19.78"></line><line x1="1" y1="12" x2="3" y2="12"></line><line x1="21" y1="12" x2="23" y2="12"></line><line x1="4.22" y1="19.78" x2="5.64" y2="18.36"></line><line x1="18.36" y1="5.64" x2="19.78" y2="4.22"></line></svg>;
const Moon = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"></path></svg>;
const Users = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path><circle cx="9" cy="7" r="4"></circle><path d="M23 21v-2a4 4 0 0 0-3-3.87"></path><path d="M16 3.13a4 4 0 0 1 0 7.75"></path></svg>;
const Swords = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M14.5 17.5 3 6l3-3 11.5 11.5"></path><path d="m21 14-9 9"></path><path d="m11.5 3.5 7 7"></path><path d="m9.5 20.5 11-11"></path></svg>;
const BrainCircuit = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M12 2a4.5 4.5 0 0 0-4.5 4.5v.43a3.5 3.5 0 0 0-3.03 3.48A4 4 0 0 0 1 14.5V16a1 1 0 0 0 1 1h.08a3.5 3.5 0 0 0 6.9 0H16a1 1 0 0 0 1-1v-1.5a4 4 0 0 0-3.47-3.98 3.5 3.5 0 0 0-3.03-3.48V6.5A4.5 4.5 0 0 0 12 2z"></path><path d="M12 17a2.5 2.5 0 0 0-2.5 2.5v.09a3.5 3.5 0 0 0 6.9 0v-.09A2.5 2.5 0 0 0 12 17z"></path><path d="M12 12.5a2.5 2.5 0 0 0-2.5-2.5v-.09a3.5 3.5 0 0 1 6.9 0v.09a2.5 2.5 0 0 0-2.4 2.5z"></path><path d="M16.5 6.5a2.5 2.5 0 0 0-2.5-2.5v-.09a3.5 3.5 0 0 1 6.9 0v.09a2.5 2.5 0 0 0-2.4 2.5z"></path><path d="M7.5 6.5a2.5 2.5 0 0 1 2.5-2.5v-.09a3.5 3.5 0 0 0-6.9 0v.09A2.5 2.5 0 0 1 7.5 6.5z"></path></svg>;
const BarChart3 = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M3 3v18h18"></path><path d="M7 16V7"></path><path d="M12 16v-4"></path><path d="M17 16v-9"></path></svg>;

// --- THÀNH PHẦN CHÍNH: App ---
export default function App() {
    const [view, setView] = useState('dashboard');
    const [userData, setUserData] = useState(null);
    const [userId, setUserId] = useState(null);
    const [loading, setLoading] = useState(true);
    const [theme, setTheme] = useState('light');
    const [showOnboarding, setShowOnboarding] = useState(false);
    const [challengeState, setChallengeState] = useState({ active: false, opponent: null });

    useEffect(() => {
        const localTheme = localStorage.getItem('theme');
        if (localTheme) setTheme(localTheme);
    }, []);

    useEffect(() => {
        if (theme === 'dark') {
            document.documentElement.classList.add('dark');
            localStorage.setItem('theme', 'dark');
        } else {
            document.documentElement.classList.remove('dark');
            localStorage.setItem('theme', 'light');
        }
    }, [theme]);

    useEffect(() => {
        const unsubscribeAuth = onAuthStateChanged(auth, (user) => {
            if (user) setUserId(user.uid);
            else signInAnonymously(auth).catch(error => console.error("Lỗi đăng nhập ẩn danh:", error));
        });
        return () => unsubscribeAuth();
    }, []);
    
    useEffect(() => {
        if (!userId) return;
        const userDocRef = doc(db, `artifacts/${appId}/users`, userId);
        const unsubscribeSnapshot = onSnapshot(userDocRef, (docSnap) => {
            if (docSnap.exists()) {
                const data = docSnap.data();
                setUserData(data);
                if (!data.hasCompletedOnboarding) setShowOnboarding(true);
            } else {
                const newUser = {
                    username: "Tân binh", total_xp: 0, koban: 100, streak: 0, current_level_focus: "n5",
                    progress: { n5: { xp: 0, lessons_completed: 0, kanji_learned: 0, vocab_learned: 0 } },
                    inventory: { streak_freezes: 0, themes: ['default'], avatars: ['default'] },
                    hasCompletedOnboarding: false,
                    stats: { wrong_kanji: [], wrong_grammar: [] }
                };
                setDoc(userDocRef, newUser).catch(e => console.error("Lỗi tạo người dùng mới:", e));
                setUserData(newUser);
                setShowOnboarding(true);
            }
            setLoading(false);
        }, (error) => {
            console.error("Lỗi lắng nghe dữ liệu:", error); setLoading(false);
        });
        return () => unsubscribeSnapshot();
    }, [userId]);

    const updateUserData = useCallback(async (newData) => {
        if (!userId) return;
        const userDocRef = doc(db, `artifacts/${appId}/users`, userId);
        try { await updateDoc(userDocRef, newData); } 
        catch (error) { console.error("Lỗi cập nhật dữ liệu người dùng:", error); }
    }, [userId]);

    const handleCloseOnboarding = () => {
        setShowOnboarding(false);
        updateUserData({ hasCompletedOnboarding: true });
    };

    const startChallenge = (opponent) => {
        setChallengeState({ active: true, opponent });
        setView('challenge');
    };

    const endChallenge = () => {
        setChallengeState({ active: false, opponent: null });
        setView('friends');
    };

    if (loading) return <div className="flex items-center justify-center h-screen bg-slate-100 dark:bg-slate-900"><div className="text-xl font-bold text-slate-600 dark:text-slate-300">Đang tải Nihongo GO...</div></div>;

    return (
        <div className="bg-slate-100 dark:bg-slate-900 min-h-screen font-sans text-slate-800 dark:text-slate-200">
            {showOnboarding && <OnboardingModal onClose={handleCloseOnboarding} />}
            <div className="container mx-auto p-4 max-w-4xl">
                <Header userData={userData} theme={theme} setTheme={setTheme} />
                <Nav setView={setView} activeView={view} />
                <main className="mt-4 bg-white dark:bg-slate-800 p-6 rounded-2xl shadow-lg min-h-[60vh]">
         
