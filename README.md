# 1<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>동식물 자원의 분류 게임</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Font Awesome for Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Jua&family=Noto+Sans+KR:wght@300;400;500;700;900&display=swap');
    
    body {
      font-family: 'Noto Sans KR', sans-serif;
      user-select: none;
      -webkit-user-select: none;
    }
    
    .fancy-font {
      font-family: 'Jua', sans-serif;
    }

    /* Card dragging visual styling */
    .dragging {
      opacity: 0.6;
      transform: scale(1.05) rotate(3deg);
      cursor: grabbing;
    }

    .drop-target-active {
      transform: scale(1.03);
      border-color: #10B981 !important;
      box-shadow: 0 10px 20px rgba(16, 185, 129, 0.2);
    }

    /* Custom Animations */
    @keyframes float {
      0%, 100% { transform: translateY(0px) rotate(0deg); }
      50% { transform: translateY(-8px) rotate(1deg); }
    }
    
    .floating-card {
      animation: float 4s ease-in-out infinite;
    }

    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      20%, 60% { transform: translateX(-8px); }
      40%, 80% { transform: translateX(8px); }
    }

    .shake {
      animation: shake 0.4s ease-in-out;
    }

    @keyframes pulse-green {
      0%, 100% { border-color: rgba(16, 185, 129, 0.4); }
      50% { border-color: rgba(16, 185, 129, 1); }
    }

    .pulse-correct {
      animation: pulse-green 1.5s infinite;
    }
  </style>
</head>
<body class="bg-gradient-to-br from-emerald-50 via-teal-50 to-amber-50 min-h-screen flex flex-col items-center justify-between p-4 overflow-x-hidden">

  <!-- Header Section -->
  <header class="w-full max-w-4xl flex justify-between items-center bg-white/80 backdrop-blur-md p-4 rounded-2xl shadow-sm border border-emerald-100/50 mb-4">
    <div class="flex items-center gap-3">
      <div class="bg-emerald-500 text-white p-2.5 rounded-xl shadow-md shadow-emerald-200">
        <i class="fa-solid fa-leaf text-xl"></i>
      </div>
      <div>
        <h1 class="fancy-font text-2xl text-emerald-800 tracking-wide">동식물 자원의 분류</h1>
        <p class="text-xs text-emerald-600/80 font-medium">실과 5학년 1학기 교과 과정</p>
      </div>
    </div>
    <div class="flex gap-2">
      <button onclick="toggleMute()" id="muteBtn" class="text-slate-500 hover:bg-slate-100 p-2 rounded-xl transition duration-200">
        <i class="fa-solid fa-volume-high text-lg"></i>
      </button>
      <button onclick="showInfoModal()" class="text-slate-500 hover:bg-slate-100 p-2 rounded-xl transition duration-200">
        <i class="fa-solid fa-circle-info text-lg"></i>
      </button>
    </div>
  </header>

  <!-- Main Container -->
  <main class="w-full max-w-4xl flex-1 flex flex-col justify-center items-center relative z-10">
    
    <!-- LANDING / START SCREEN -->
    <div id="startScreen" class="w-full bg-white/95 backdrop-blur-lg rounded-3xl p-6 md:p-10 shadow-xl border border-emerald-100 max-w-2xl text-center space-y-6 transition-all duration-300">
      <div class="inline-block bg-emerald-50 text-emerald-700 px-4 py-1.5 rounded-full text-xs font-bold tracking-widest uppercase mb-2">
        <i class="fa-solid fa-graduation-cap mr-1"></i> Interactive Learn-Play
      </div>
      <h2 class="fancy-font text-4xl md:text-5xl text-slate-800 leading-tight">
        교과서 주렁주렁<br><span class="text-emerald-600">동식물 자원 분류 게임</span>
      </h2>
      <p class="text-slate-600 text-sm md:text-base leading-relaxed max-w-md mx-auto">
        우리 생활에 유용하게 이용되는 동물과 식물을 활용 목적에 알맞게 알록달록 바구니에 담아주세요!
      </p>

      <!-- Stage Previews -->
      <div class="grid grid-cols-3 gap-3 max-w-md mx-auto py-2">
        <div class="bg-emerald-50/50 p-3 rounded-xl border border-emerald-100/50">
          <span class="block text-xl">🌱</span>
          <span class="text-xs font-bold text-emerald-800">1단계 식물 자원</span>
        </div>
        <div class="bg-amber-50/50 p-3 rounded-xl border border-amber-100/50">
          <span class="block text-xl">🐄</span>
          <span class="text-xs font-bold text-amber-800">2단계 동물 자원</span>
        </div>
        <div class="bg-indigo-50/50 p-3 rounded-xl border border-indigo-100/50">
          <span class="block text-xl">🧩</span>
          <span class="text-xs font-bold text-indigo-800">3단계 실전 종합</span>
        </div>
      </div>

      <!-- Play Buttons -->
      <div class="flex flex-col sm:flex-row gap-3 justify-center pt-4">
        <button onclick="startGame(1)" class="fancy-font text-xl bg-gradient-to-r from-emerald-500 to-teal-600 text-white px-8 py-4 rounded-2xl shadow-lg shadow-emerald-200 hover:shadow-xl hover:shadow-emerald-300 transform hover:-translate-y-0.5 transition duration-200">
          🌾 식물 단계부터 시작하기
        </button>
        <button onclick="startGame(3)" class="fancy-font text-xl bg-gradient-to-r from-slate-700 to-slate-800 text-white px-8 py-4 rounded-2xl shadow-lg shadow-slate-200 hover:shadow-xl transform hover:-translate-y-0.5 transition duration-200">
          ⚡ 실전 종합 바로 도전
        </button>
      </div>
      
      <p class="text-xs text-slate-400">마우스 드래그 또는 터치와 하단 버튼을 모두 지원합니다.</p>
    </div>

    <!-- GAMEPLAY SCREEN (Initially Hidden) -->
    <div id="gameScreen" class="w-full hidden flex-col items-center gap-6 transition-all duration-300">
      
      <!-- Top Stats Bar -->
      <div class="w-full flex justify-between items-center bg-white/70 backdrop-blur-md p-3 rounded-2xl shadow-sm border border-emerald-50">
        <!-- Stage Info -->
        <div class="flex items-center gap-2">
          <span id="stageBadge" class="bg-emerald-100 text-emerald-800 px-3 py-1 rounded-full text-xs font-bold">1단계: 식물 자원</span>
        </div>
        
        <!-- Score and Timer -->
        <div class="flex items-center gap-4">
          <div class="flex items-center gap-1.5">
            <span class="text-sm font-semibold text-slate-500">점수:</span>
            <span id="scoreText" class="fancy-font text-xl text-emerald-600">000</span>
          </div>
          <div class="h-6 w-px bg-slate-200"></div>
          <div class="flex items-center gap-2">
            <i class="fa-regular fa-clock text-slate-500"></i>
            <span id="timerText" class="font-mono font-bold text-slate-700 w-8">20s</span>
          </div>
        </div>

        <!-- Lives (Hearts) -->
        <div id="livesContainer" class="flex gap-1 text-rose-500 text-lg">
          <i class="fa-solid fa-heart"></i>
          <i class="fa-solid fa-heart"></i>
          <i class="fa-solid fa-heart"></i>
        </div>
      </div>

      <!-- Stage Goal Prompt -->
      <div id="stagePrompt" class="text-center font-bold text-slate-700 text-sm md:text-base">
        💡 아래 자원 카드를 알맞은 분류 바구니로 드래그하여 넣어주세요!
      </div>

      <!-- Playfield: Center Active Card -->
      <div class="relative w-full h-72 flex justify-center items-center">
        <!-- Background Decor / Target Glow -->
        <div class="absolute w-56 h-56 rounded-full bg-emerald-100/20 blur-2xl"></div>

        <!-- The Target Drag Card -->
        <div id="cardContainer" class="absolute z-20">
          <!-- Card Template (Will be generated dynamically) -->
          <div id="activeCard" draggable="true" class="floating-card cursor-grab w-52 h-64 bg-white rounded-3xl shadow-xl border-2 border-emerald-500/10 hover:border-emerald-500/30 p-5 flex flex-col justify-between items-center transition-all duration-150 transform">
            <div class="w-full flex justify-between items-center">
              <span class="text-xs text-slate-400 bg-slate-50 px-2 py-0.5 rounded-full font-medium">자원 분류 카드</span>
              <span class="text-xs text-emerald-600 font-bold" id="cardClassHint">식물 자원</span>
            </div>
            
            <div class="flex flex-col items-center text-center">
              <span id="cardEmoji" class="text-6xl mb-2 filter drop-shadow-sm">🌾</span>
              <span id="cardTitle" class="fancy-font text-2xl text-slate-800">쌀</span>
            </div>

            <div class="w-full bg-emerald-50/50 py-2 px-3 rounded-xl border border-emerald-100/40 text-center">
              <p id="cardDesc" class="text-[11px] text-slate-500 leading-tight">사람이 먹는 주식으로 활용해요.</p>
            </div>
          </div>
        </div>
      </div>

      <!-- Mobile/Touch Accessible Fast Tap Buttons -->
      <div class="w-full max-w-md bg-white/40 p-2.5 rounded-2xl border border-white/60">
        <p class="text-center text-[11px] text-slate-500 font-medium mb-1.5"><i class="fa-solid fa-hand-pointer mr-1"></i> 하단 바구니를 직접 터치(클릭)하거나 드래그할 수 있습니다.</p>
      </div>

      <!-- Target Drop Zones Grid (Changes depending on Stage) -->
      <div id="dropZonesContainer" class="w-full grid grid-cols-2 md:grid-cols-4 gap-4 transition-all duration-300">
        <!-- Dynamic Category Buckets will be injected here -->
      </div>

    </div>

    <!-- REPORT CARD / GAME OVER SCREEN (Initially Hidden) -->
    <div id="resultScreen" class="w-full max-w-lg hidden bg-white/95 backdrop-blur-lg rounded-3xl p-8 shadow-xl border border-emerald-100 text-center space-y-6 transition-all duration-300">
      <div class="w-20 h-20 bg-emerald-100 text-emerald-600 rounded-full flex items-center justify-center mx-auto text-4xl shadow-inner animate-bounce">
        <i class="fa-solid fa-trophy"></i>
      </div>
      
      <div class="space-y-1">
        <h3 class="fancy-font text-3xl text-slate-800">참 잘했어요!</h3>
        <p class="text-sm text-slate-500">동식물 자원의 훌륭한 분류 도우미가 되었습니다!</p>
      </div>

      <!-- Results Grid -->
      <div class="grid grid-cols-2 gap-4 bg-emerald-50/40 p-4 rounded-2xl border border-emerald-100 max-w-sm mx-auto">
        <div class="text-center">
          <span class="block text-xs text-slate-400 font-bold uppercase">최종 점수</span>
          <span id="finalScore" class="fancy-font text-3xl text-emerald-600">0</span>
        </div>
        <div class="text-center">
          <span class="block text-xs text-slate-400 font-bold uppercase">정답률</span>
          <span id="accuracyText" class="fancy-font text-3xl text-teal-600">0%</span>
        </div>
      </div>

      <!-- Rank Badge -->
      <div class="max-w-sm mx-auto p-3.5 bg-gradient-to-r from-emerald-500 to-teal-600 text-white rounded-xl shadow-md">
        <p class="text-[11px] uppercase tracking-wider font-bold opacity-80">분류 전문가 등급</p>
        <h4 id="rankTitle" class="fancy-font text-xl">🏆 주렁주렁 박사님</h4>
      </div>

      <!-- Action Buttons -->
      <div class="flex gap-3 justify-center pt-2">
        <button onclick="restartGame()" class="fancy-font bg-slate-100 text-slate-700 px-6 py-3 rounded-xl hover:bg-slate-200 transition duration-150">
          <i class="fa-solid fa-rotate-left mr-1"></i> 다시하기
        </button>
        <button onclick="goToLanding()" class="fancy-font bg-gradient-to-r from-emerald-500 to-emerald-600 text-white px-6 py-3 rounded-xl shadow-md hover:shadow-lg transition duration-150">
          메인 화면으로
        </button>
      </div>
    </div>

  </main>

  <!-- Footer Section -->
  <footer class="w-full max-w-4xl text-center text-xs text-slate-400 py-4 mt-4 border-t border-emerald-100/20">
    <p>© 2026 동식물 자원의 분류 놀이터. 실과 5학년 교육 자료.</p>
  </footer>

  <!-- LEARN STATS MODAL (Definitions helper) -->
  <div id="infoModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex justify-center items-center opacity-0 pointer-events-none transition-opacity duration-300 p-4">
    <div class="bg-white rounded-3xl w-full max-w-lg shadow-2xl p-6 relative max-h-[85vh] overflow-y-auto">
      <button onclick="hideInfoModal()" class="absolute right-4 top-4 text-slate-400 hover:text-slate-600 p-1.5 rounded-lg">
        <i class="fa-solid fa-xmark text-lg"></i>
      </button>

      <h3 class="fancy-font text-2xl text-emerald-800 mb-4"><i class="fa-solid fa-book-open mr-2"></i>교과서 핵심 요점 정리</h3>
      
      <div class="space-y-4 text-sm text-slate-600 leading-relaxed">
        <div>
          <h4 class="font-bold text-emerald-700 border-b border-emerald-100 pb-1 flex items-center gap-1.5">
            <span class="text-base">🌱</span> 식물 자원 (작물)의 분류
          </h4>
          <ul class="space-y-2 mt-2 pl-1">
            <li><strong>🌾 식용 작물:</strong> 사람이 먹는 주식으로 활용해요. <span class="text-xs text-slate-400">(예: 쌀, 밀)</span></li>
            <li><strong>🍇 원예 작물:</strong> 부식이나 간식으로 먹거나, 보고 즐기는 용도로 활용해요. <span class="text-xs text-slate-400">(예: 포도, 튤립)</span></li>
            <li><strong>☁️ 공예 작물:</strong> 가공 과정을 거쳐 생활용품으로 활용해요. <span class="text-xs text-slate-400">(예: 목화, 수세미)</span></li>
            <li><strong>☘️ 녹비 작물:</strong> 식물의 퇴비(천연 비료)로 활용해요. <span class="text-xs text-slate-400">(예: 토끼풀, 자운영)</span></li>
          </ul>
        </div>

        <div>
          <h4 class="font-bold text-amber-700 border-b border-amber-100 pb-1 flex items-center gap-1.5">
            <span class="text-base">🐄</span> 동물 자원의 분류
          </h4>
          <ul class="space-y-2 mt-2 pl-1">
            <li><strong>🐕 반려동물:</strong> 친구나 가족처럼 지내며 정서적 안정감이나 기쁨을 얻어요. <span class="text-xs text-slate-400">(예: 개, 고양이)</span></li>
            <li><strong>🐓 경제 동물:</strong> 식용으로 이용하거나 여러 생활용품으로 만들어 경제적 이익을 얻어요. <span class="text-xs text-slate-400">(예: 닭, 소)</span></li>
            <li><strong>🐭 특수 동물:</strong> 실험 동물, 동물원 동물, 특수견 등이 해당하며 특수한 목적으로 이용해요. <span class="text-xs text-slate-400">(예: 실험용 쥐, 동물원 판다, 안내견)</span></li>
          </ul>
        </div>
      </div>
    </div>
  </div>

  <!-- WRONG ANSWER/CORRECT FEEDBACK POPUP (Textbook Guided Explanation) -->
  <div id="feedbackModal" class="fixed inset-0 bg-slate-900/40 backdrop-blur-sm z-50 flex justify-center items-center opacity-0 pointer-events-none transition-opacity duration-200 p-4">
    <div id="feedbackBox" class="bg-white rounded-3xl w-full max-w-md shadow-2xl p-6 text-center transform scale-95 transition-transform duration-200">
      <!-- Status Icon -->
      <div id="feedbackIcon" class="w-16 h-16 rounded-full flex items-center justify-center mx-auto text-3xl mb-3 shadow-inner">
        <!-- Injected Icon -->
      </div>
      
      <!-- Status Title -->
      <h3 id="feedbackTitle" class="fancy-font text-2xl mb-1">정답 혹은 오답</h3>
      <!-- Detail Definition -->
      <p id="feedbackDefinition" class="text-xs font-semibold uppercase tracking-wider mb-3 text-emerald-600">식용 작물</p>
      
      <!-- Concept explanation -->
      <div class="bg-slate-50/80 border border-slate-100 p-4 rounded-2xl mb-4 text-sm text-slate-600 leading-relaxed text-left">
        <p id="feedbackExplanation">교과서의 해당 자원 정의가 여기에 상세하게 표출됩니다.</p>
      </div>

      <button onclick="closeFeedback()" class="fancy-font w-full bg-slate-800 text-white py-3.5 rounded-2xl shadow-lg hover:bg-slate-700 transition duration-150">
        확인하고 계속하기 <i class="fa-solid fa-arrow-right ml-1"></i>
      </button>
    </div>
  </div>

  <!-- Confetti Canvas for Victory -->
  <canvas id="confettiCanvas" class="fixed inset-0 pointer-events-none z-40 w-full h-full"></canvas>

  <!-- Script logic implementing high fidelity vibe game -->
  <script>
    // AUDIO SYNTHESIZER (Web Audio API)
    let isMuted = false;
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

    function playTone(type) {
      if (isMuted) return;
      try {
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.connect(gain);
        gain.connect(audioCtx.destination);

        if (type === 'correct') {
          osc.type = 'sine';
          osc.frequency.setValueAtTime(523.25, audioCtx.currentTime); // C5
          osc.frequency.setValueAtTime(659.25, audioCtx.currentTime + 0.1); // E5
          gain.gain.setValueAtTime(0.08, audioCtx.currentTime);
          gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.25);
          osc.start();
          osc.stop(audioCtx.currentTime + 0.25);
        } else if (type === 'wrong') {
          osc.type = 'triangle';
          osc.frequency.setValueAtTime(180, audioCtx.currentTime);
          osc.frequency.setValueAtTime(130, audioCtx.currentTime + 0.08);
          gain.gain.setValueAtTime(0.12, audioCtx.currentTime);
          gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.3);
          osc.start();
          osc.stop(audioCtx.currentTime + 0.3);
        } else if (type === 'stageUp') {
          let notes = [523.25, 659.25, 783.99, 1046.50];
          notes.forEach((freq, idx) => {
            const o = audioCtx.createOscillator();
            const g = audioCtx.createGain();
            o.connect(g);
            g.connect(audioCtx.destination);
            o.type = 'sine';
            o.frequency.setValueAtTime(freq, audioCtx.currentTime + idx * 0.08);
            g.gain.setValueAtTime(0.06, audioCtx.currentTime + idx * 0.08);
            g.gain.exponentialRampToValueAtTime(0.005, audioCtx.currentTime + idx * 0.08 + 0.25);
            o.start(audioCtx.currentTime + idx * 0.08);
            o.stop(audioCtx.currentTime + idx * 0.08 + 0.25);
          });
        } else if (type === 'gameover') {
          let notes = [293.66, 261.63, 220.00, 196.00];
          notes.forEach((freq, idx) => {
            const o = audioCtx.createOscillator();
            const g = audioCtx.createGain();
            o.connect(g);
            g.connect(audioCtx.destination);
            o.type = 'sawtooth';
            o.frequency.setValueAtTime(freq, audioCtx.currentTime + idx * 0.15);
            g.gain.setValueAtTime(0.08, audioCtx.currentTime + idx * 0.15);
            g.gain.exponentialRampToValueAtTime(0.005, audioCtx.currentTime + idx * 0.15 + 0.3);
            o.start(audioCtx.currentTime + idx * 0.15);
            o.stop(audioCtx.currentTime + idx * 0.15 + 0.3);
          });
        }
      } catch (e) {
        console.log("Audio contexts pending interaction: ", e);
      }
    }

    function toggleMute() {
      isMuted = !isMuted;
      const muteIcon = document.querySelector('#muteBtn i');
      if (isMuted) {
        muteIcon.className = "fa-solid fa-volume-xmark text-lg text-rose-500 animate-pulse";
      } else {
        muteIcon.className = "fa-solid fa-volume-high text-lg text-slate-500";
        // Resume context on play
        if (audioCtx.state === 'suspended') audioCtx.resume();
      }
    }

    // GAME DATA BASE (Strict compliance with the uploaded document)
    const ITEMS_DB = [
      // PLANT RESOURCES (Stage 1)
      { id: "p1", name: "쌀", category: "식용 작물", icon: "🌾", type: "plant", desc: "사람이 먹는 주식으로 활용해요.", textBook: "주식 작물로서 우리 밥상에 빠질 수 없는 귀중한 곡물자원입니다." },
      { id: "p2", name: "밀", category: "식용 작물", icon: "🍞", type: "plant", desc: "사람이 먹는 주식으로 활용해요.", textBook: "쌀과 더불어 세계의 3대 곡물 중 하나로 빵, 면류의 주원료입니다." },
      { id: "p3", name: "포도", category: "원예 작물", icon: "🍇", type: "plant", desc: "부식이나 간식으로 먹거나, 보고 즐기는 용도로 활용해요.", textBook: "맛있는 제철 과일이자 간식으로 먹는 대표적인 과수 작물입니다." },
      { id: "p4", name: "튤립", category: "원예 작물", icon: "🌷", type: "plant", desc: "보고 즐기는 용도로 활용해요.", textBook: "정원이나 화단에 심어 사람들에게 시각적 즐거움과 여유를 줍니다." },
      { id: "p5", name: "목화", category: "공예 작물", icon: "☁️", type: "plant", desc: "가공 과정을 거쳐 생활용품으로 활용해요.", textBook: "목화 솜은 실을 뽑아 따뜻한 면직물 옷이나 이불을 만드는 소중한 천연 자원입니다." },
      { id: "p6", name: "수세미", category: "공예 작물", icon: "🧽", type: "plant", desc: "가공 과정을 거쳐 생활용품으로 활용해요.", textBook: "잘 자란 섬유질을 가공하여 친환경 청소 수세미나 목욕 용품으로 사용합니다." },
      { id: "p7", name: "토끼풀", category: "녹비 작물", icon: "☘️", type: "plant", desc: "식물의 퇴비로 활용해요.", textBook: "질소를 고정하여 토양을 기름지게 하는 대표적인 친환경 거름 작물입니다." },
      { id: "p8", name: "자운영", category: "녹비 작물", icon: "🌸", type: "plant", desc: "식물의 퇴비로 활용해요.", textBook: "분홍빛 꽃줄기를 흙과 섞어 썩히면 훌륭한 유기질 천연 퇴비가 되어 땅을 가꿉니다." },
      
      // ANIMAL RESOURCES (Stage 2)
      { id: "a1", name: "개", category: "반려 동물", icon: "🐕", type: "animal", desc: "친구나 가족처럼 지내며 정서적 안정감과 기쁨을 얻어요.", textBook: "동반자로서 사람에게 사랑과 따뜻한 유대감을 선사하는 동물입니다." },
      { id: "a2", name: "고양이", category: "반려 동물", icon: "🐈", type: "animal", desc: "친구나 가족처럼 지내며 정서적 안정감과 기쁨을 얻어요.", textBook: "사람과 같은 주거 공간을 공유하며 다정하게 공존하는 대표적인 반려동물입니다." },
      { id: "a3", name: "닭", category: "경제 동물", icon: "🐓", type: "animal", desc: "식용으로 이용하거나 여러 생활용품으로 만들어 경제적 이익을 얻어요.", textBook: "계란과 고기를 제공하여 인간의 단백질 급원이 되어주는 고마운 가축입니다." },
      { id: "a4", name: "소", category: "경제 동물", icon: "🐄", type: "animal", desc: "우유나 가죽, 식용으로 이용해 경제적 이익을 얻어요.", textBook: "인간에게 우유와 맛있는 고기, 그리고 가죽 가방이나 신발 등을 만드는 원료를 제공합니다." },
      { id: "a5", name: "실험용 쥐", category: "특수 동물", icon: "🐭", type: "animal", desc: "의학 실험 등 특수한 목적으로 이용해요.", textBook: "의학, 생명공학 등의 안전성 검증 및 연구를 돕는 실험 동물입니다." },
      { id: "a6", name: "동물원 판다", category: "특수 동물", icon: "🐼", type: "animal", desc: "동물원 전시 및 종 보존 등 특수한 목적으로 이용해요.", textBook: "멸종위기 보호 및 생태 교육, 관람용으로 지정 사육되는 귀중한 특수 동물입니다." },
      { id: "a7", name: "안내견", category: "특수 동물", icon: "🦮", type: "animal", desc: "장애인 안내 등 특수한 목적으로 이용해요.", textBook: "시각장애인의 안전한 보행을 유도하는 사회적 임무를 가진 훈련된 든든한 동반견입니다." }
    ];

    // STAGES INFO
    const STAGES = {
      1: {
        title: "1단계: 식물 자원의 분류",
        badge: "bg-emerald-100 text-emerald-800",
        categories: ["식용 작물", "원예 작물", "공예 작물", "녹비 작물"],
        items: ITEMS_DB.filter(i => i.type === "plant")
      },
      2: {
        title: "2단계: 동물 자원의 분류",
        badge: "bg-amber-100 text-amber-800",
        categories: ["반려 동물", "경제 동물", "특수 동물"],
        items: ITEMS_DB.filter(i => i.type === "animal")
      },
      3: {
        title: "3단계: 실전 종합 도전",
        badge: "bg-indigo-100 text-indigo-800",
        categories: ["식용 작물", "원예 작물", "공예 작물", "녹비 작물", "반려 동물", "경제 동물", "특수 동물"],
        items: [...ITEMS_DB] // All mixed
      }
    };

    // GAME STATE VARIABLES
    let currentStage = 1;
    let score = 0;
    let lives = 3;
    let activeQueue = [];
    let currentItem = null;
    let totalQuestionsAnswered = 0;
    let correctAnswersCount = 0;
    
    // TIMER VARIABLES
    let timerVal = 20;
    let timerInterval = null;

    // DRAG AND TOUCH POSITION TRACKING
    let isDraggingCard = false;
    let cardStartX = 0;
    let cardStartY = 0;
    const cardEl = document.getElementById('activeCard');

    // INITIAL SETUP
    window.onload = function() {
      // Setup window listeners for responsive elements
      initControls();
    };

    function showInfoModal() {
      const modal = document.getElementById('infoModal');
      modal.classList.remove('opacity-0', 'pointer-events-none');
    }

    function hideInfoModal() {
      const modal = document.getElementById('infoModal');
      modal.classList.add('opacity-0', 'pointer-events-none');
    }

    function goToLanding() {
      document.getElementById('gameScreen').classList.add('hidden');
      document.getElementById('resultScreen').classList.add('hidden');
      document.getElementById('startScreen').classList.remove('hidden');
      clearInterval(timerInterval);
    }

    // GAME ENGINE INITS
    function startGame(stageNum) {
      if (audioCtx.state === 'suspended') {
        audioCtx.resume();
      }

      currentStage = stageNum;
      score = 0;
      lives = 3;
      totalQuestionsAnswered = 0;
      correctAnswersCount = 0;
      
      // Load Stage Data
      const stageConfig = STAGES[currentStage];
      
      // Shuffle Items
      activeQueue = [...stageConfig.items].sort(() => Math.random() - 0.5);

      // DOM UI Transitions
      document.getElementById('startScreen').classList.add('hidden');
      document.getElementById('resultScreen').classList.add('hidden');
      document.getElementById('gameScreen').classList.remove('hidden');
      
      updateLivesUI();
      document.getElementById('scoreText').innerText = String(score).padStart(3, '0');
      
      // Set Stage Title Badge
      const stageBadge = document.getElementById('stageBadge');
      stageBadge.innerText = stageConfig.title;
      stageBadge.className = `${stageConfig.badge} px-3 py-1 rounded-full text-xs font-bold transition-all duration-300`;

      // Draw Buckets
      renderBuckets(stageConfig.categories);

      // Load Next Question Card
      loadNextCard();
    }

    // CREATE DYNAMIC DROPPABLE BASKETS / BUTTONS
    function renderBuckets(categories) {
      const container = document.getElementById('dropZonesContainer');
      container.innerHTML = '';
      
      // Change column layout according to size of classes
      if (categories.length > 4) {
        container.className = "w-full grid grid-cols-2 sm:grid-cols-4 lg:grid-cols-7 gap-3 transition-all duration-300";
      } else {
        container.className = "w-full grid grid-cols-2 md:grid-cols-4 gap-4 transition-all duration-300";
      }

      const colors = {
        "식용 작물": { border: "border-emerald-200", bg: "bg-emerald-50/70", text: "text-emerald-800", icon: "🌾", hover: "hover:bg-emerald-100/50" },
        "원예 작물": { border: "border-pink-200", bg: "bg-pink-50/70", text: "text-pink-800", icon: "🍇", hover: "hover:bg-pink-100/50" },
        "공예 작물": { border: "border-sky-200", bg: "bg-sky-50/70", text: "text-sky-800", icon: "☁️", hover: "hover:bg-sky-100/50" },
        "녹비 작물": { border: "border-teal-200", bg: "bg-teal-50/70", text: "text-teal-800", icon: "☘️", hover: "hover:bg-teal-100/50" },
        "반려 동물": { border: "border-rose-200", bg: "bg-rose-50/70", text: "text-rose-800", icon: "🐕", hover: "hover:bg-rose-100/50" },
        "경제 동물": { border: "border-amber-200", bg: "bg-amber-50/70", text: "text-amber-800", icon: "🐄", hover: "hover:bg-amber-100/50" },
        "특수 동물": { border: "border-indigo-200", bg: "bg-indigo-50/70", text: "text-indigo-800", icon: "🦮", hover: "hover:bg-indigo-100/50" }
      };

      categories.forEach(cat => {
        const theme = colors[cat] || { border: "border-slate-200", bg: "bg-slate-50", text: "text-slate-800", icon: "📦", hover: "hover:bg-slate-100" };
        
        const zone = document.createElement('div');
        zone.className = `drop-zone flex flex-col justify-between items-center p-4 rounded-2xl border-2 ${theme.border} ${theme.bg} ${theme.hover} transition-all duration-200 cursor-pointer h-32 text-center shadow-sm select-none relative`;
        zone.dataset.category = cat;

        zone.innerHTML = `
          <div class="text-3xl filter drop-shadow-sm pointer-events-none">${theme.icon}</div>
          <span class="fancy-font text-xs md:text-sm ${theme.text} pointer-events-none tracking-tight block mt-2">${cat}</span>
          <div class="absolute inset-0 rounded-2xl flex items-center justify-center opacity-0 hover:opacity-10 transition duration-150 bg-emerald-400">
            <span class="text-xs font-bold text-white uppercase">여기에 담기</span>
          </div>
        `;

        // Direct Touch/Click Action Setup
        zone.addEventListener('click', () => handleDirectSelect(cat));

        // Drag & Drop Listeners
        zone.addEventListener('dragover', dragOver);
        zone.addEventListener('dragenter', dragEnter);
        zone.addEventListener('dragleave', dragLeave);
        zone.addEventListener('drop', dropCard);

        container.appendChild(zone);
      });
    }

    // DRAW NEXT CARD
    function loadNextCard() {
      // Stop and restart Timer
      clearInterval(timerInterval);

      // If out of questions, check stage or finish
      if (activeQueue.length === 0) {
        if (currentStage < 3) {
          playTone('stageUp');
          // Increment Stage automatically
          currentStage++;
          const stageConfig = STAGES[currentStage];
          activeQueue = [...stageConfig.items].sort(() => Math.random() - 0.5);
          
          // Re-render
          document.getElementById('stageBadge').innerText = stageConfig.title;
          document.getElementById('stageBadge').className = `${stageConfig.badge} px-3 py-1 rounded-full text-xs font-bold transition-all duration-300`;
          renderBuckets(stageConfig.categories);
          
          // Brief prompt update
          const prompt = document.getElementById('stagePrompt');
          prompt.innerText = `🌟 축하합니다! 다음 단계: ${stageConfig.title}에 돌입했습니다!`;
          setTimeout(() => {
            prompt.innerText = `💡 아래 자원 카드를 알맞은 분류 바구니로 드래그하여 넣어주세요!`;
          }, 2000);
        } else {
          endGame();
          return;
        }
      }

      currentItem = activeQueue.pop();

      // Update active Card elements
      document.getElementById('cardEmoji').innerText = currentItem.icon;
      document.getElementById('cardTitle').innerText = currentItem.name;
      document.getElementById('cardDesc').innerText = currentItem.desc;
      
      const hintLabel = document.getElementById('cardClassHint');
      hintLabel.innerText = currentItem.type === 'plant' ? '식물 자원' : '동물 자원';
      hintLabel.className = currentItem.type === 'plant' ? 'text-xs text-emerald-600 font-bold' : 'text-xs text-amber-600 font-bold';

      // Reset Card animation & transform styling
      cardEl.className = "floating-card cursor-grab w-52 h-64 bg-white rounded-3xl shadow-xl border-2 border-emerald-500/10 hover:border-emerald-500/30 p-5 flex flex-col justify-between items-center transition-all duration-150 transform";
      cardEl.style.transform = '';
      cardEl.style.left = 'auto';
      cardEl.style.top = 'auto';

      // Start Countdown Timer (20 seconds for interactive freshness)
      timerVal = 20;
      document.getElementById('timerText').innerText = timerVal + "s";
      document.getElementById('timerText').className = "font-mono font-bold text-slate-700 w-8";
      
      timerInterval = setInterval(() => {
        timerVal--;
        document.getElementById('timerText').innerText = timerVal + "s";
        
        if (timerVal <= 5) {
          document.getElementById('timerText').className = "font-mono font-bold text-rose-500 animate-pulse w-8";
        }

        if (timerVal <= 0) {
          clearInterval(timerInterval);
          handleTimeOut();
        }
      }, 1000);
    }

    // HANDLE DRAG AND DROP LOGIC
    function initControls() {
      // HTML5 Drag Event for Card
      cardEl.addEventListener('dragstart', (e) => {
        cardEl.classList.add('dragging');
        e.dataTransfer.setData('text/plain', currentItem.category);
      });

      cardEl.addEventListener('dragend', () => {
        cardEl.classList.remove('dragging');
      });

      // Mobile touch-friendly implementation
      cardEl.addEventListener('touchstart', (e) => {
        isDraggingCard = true;
        const touch = e.touches[0];
        cardStartX = touch.clientX;
        cardStartY = touch.clientY;
        cardEl.classList.remove('floating-card'); // Stop auto float animation while dragging
        cardEl.style.transition = 'none';
      }, { passive: true });

      cardEl.addEventListener('touchmove', (e) => {
        if (!isDraggingCard) return;
        const touch = e.touches[0];
        const dx = touch.clientX - cardStartX;
        const dy = touch.clientY - cardStartY;
        
        cardEl.style.transform = `translate(${dx}px, ${dy}px) rotate(${dx * 0.05}deg)`;
      }, { passive: true });

      cardEl.addEventListener('touchend', (e) => {
        if (!isDraggingCard) return;
        isDraggingCard = false;
        cardEl.style.transition = 'transform 0.3s ease-out';
        
        // Detect current drop target underneath touch point
        const changedTouch = e.changedTouches[0];
        const elem = document.elementFromPoint(changedTouch.clientX, changedTouch.clientY);
        
        // Find if dropping on a category bucket element
        const dropZone = elem ? elem.closest('.drop-zone') : null;
        
        if (dropZone) {
          const targetCategory = dropZone.dataset.category;
          evaluateAnswer(targetCategory);
        } else {
          // Snap back
          cardEl.style.transform = 'translate(0px, 0px)';
          cardEl.classList.add('floating-card');
        }
      });
    }

    function dragOver(e) {
      e.preventDefault();
    }

    function dragEnter(e) {
      e.preventDefault();
      this.classList.add('drop-target-active');
    }

    function dragLeave() {
      this.classList.remove('drop-target-active');
    }

    function dropCard(e) {
      this.classList.remove('drop-target-active');
      const targetCategory = this.dataset.category;
      evaluateAnswer(targetCategory);
    }

    // Touch/Click Button Action Direct trigger
    function handleDirectSelect(category) {
      evaluateAnswer(category);
    }

    // ANSWER EVALUATOR
    function evaluateAnswer(selectedCategory) {
      clearInterval(timerInterval);
      totalQuestionsAnswered++;

      if (selectedCategory === currentItem.category) {
        // Success Logic
        score += 100;
        correctAnswersCount++;
        document.getElementById('scoreText').innerText = String(score).padStart(3, '0');
        
        playTone('correct');
        showFeedbackPopup(true);
      } else {
        // Penalty Logic
        lives--;
        updateLivesUI();
        
        playTone('wrong');
        cardEl.classList.add('shake');
        setTimeout(() => {
          cardEl.classList.remove('shake');
        }, 400);

        showFeedbackPopup(false);
      }
    }

    function handleTimeOut() {
      totalQuestionsAnswered++;
      lives--;
      updateLivesUI();
      playTone('wrong');
      showFeedbackPopup(false, true); // true indicates a timeout event
    }

    // POPUP CONCEPT REINFORCEMENT (Guided feedback)
    function showFeedbackPopup(isCorrect, isTimeout = false) {
      const feedbackModal = document.getElementById('feedbackModal');
      const feedbackBox = document.getElementById('feedbackBox');
      const iconBox = document.getElementById('feedbackIcon');
      const title = document.getElementById('feedbackTitle');
      const defLabel = document.getElementById('feedbackDefinition');
      const explanation = document.getElementById('feedbackExplanation');

      // Setup styles based on correct/incorrect status
      if (isCorrect) {
        iconBox.className = "w-16 h-16 rounded-full flex items-center justify-center mx-auto text-3xl mb-3 shadow-inner bg-emerald-100 text-emerald-600";
        iconBox.innerHTML = '<i class="fa-solid fa-check"></i>';
        title.className = "fancy-font text-2xl mb-1 text-emerald-700";
        title.innerText = "참 잘했어요! 정답입니다.";
      } else {
        iconBox.className = "w-16 h-16 rounded-full flex items-center justify-center mx-auto text-3xl mb-3 shadow-inner bg-rose-100 text-rose-600";
        iconBox.innerHTML = isTimeout ? '<i class="fa-regular fa-clock"></i>' : '<i class="fa-solid fa-triangle-exclamation"></i>';
        title.className = "fancy-font text-2xl mb-1 text-rose-700";
        title.innerText = isTimeout ? "시간이 초과되었어요!" : "아쉬워요! 다시 공부해봐요.";
      }

      // Populate Concept Definition
      defLabel.innerText = `${currentItem.name}은(는) ➔ [${currentItem.category}]`;
      defLabel.className = currentItem.type === 'plant' 
        ? 'text-xs font-bold uppercase tracking-wider mb-3 text-emerald-600' 
        : 'text-xs font-bold uppercase tracking-wider mb-3 text-amber-600';

      explanation.innerHTML = `
        <p class="font-bold mb-2 text-slate-800">📌 ${currentItem.category}의 정의</p>
        <p class="mb-3 pl-2 border-l-2 border-emerald-400 text-slate-600">${currentItem.desc}</p>
        <p class="text-xs text-slate-500 bg-emerald-50/50 p-2 rounded-xl mt-1 leading-normal"><i class="fa-solid fa-quote-left mr-1 opacity-50"></i>${currentItem.textBook}<i class="fa-solid fa-quote-right ml-1 opacity-50"></i></p>
      `;

      // Trigger Show Modal
      feedbackModal.classList.remove('opacity-0', 'pointer-events-none');
      feedbackBox.classList.remove('scale-95');
    }

    function closeFeedback() {
      const feedbackModal = document.getElementById('feedbackModal');
      const feedbackBox = document.getElementById('feedbackBox');
      
      feedbackModal.classList.add('opacity-0', 'pointer-events-none');
      feedbackBox.classList.add('scale-95');

      // Check remaining lives
      if (lives <= 0) {
        endGame();
      } else {
        loadNextCard();
      }
    }

    // UPDATE LIVES UI
    function updateLivesUI() {
      const container = document.getElementById('livesContainer');
      container.innerHTML = '';
      
      for (let i = 0; i < 3; i++) {
        const heart = document.createElement('i');
        if (i < lives) {
          heart.className = "fa-solid fa-heart transition-all duration-300";
        } else {
          heart.className = "fa-regular fa-heart text-slate-300 transition-all duration-300";
        }
        container.appendChild(heart);
      }
    }

    // END GAME / SHOW SUMMARY REPORT
    function endGame() {
      clearInterval(timerInterval);

      document.getElementById('gameScreen').classList.add('hidden');
      document.getElementById('resultScreen').classList.remove('hidden');

      // Calculate stats
      document.getElementById('finalScore').innerText = score;
      const accuracy = totalQuestionsAnswered > 0 ? Math.round((correctAnswersCount / totalQuestionsAnswered) * 100) : 0;
      document.getElementById('accuracyText').innerText = accuracy + "%";

      // Badge Calculation
      const rankTitle = document.getElementById('rankTitle');
      if (score >= 1200 && lives === 3) {
        rankTitle.innerText = "🏆 만점! 동식물 자원 주춧돌 명예 박사";
        startConfetti();
      } else if (score >= 800) {
        rankTitle.innerText = "🥇 우수! 에코 분류 최고 전문가";
        startConfetti();
      } else if (score >= 400) {
        rankTitle.innerText = "🥈 통과! 동식물 든든한 가꾸미";
      } else {
        rankTitle.innerText = "🥉 새내기! 더 튼튼히 학습해보기";
        playTone('gameover');
      }
    }

    function restartGame() {
      startGame(currentStage);
    }

    // PREMIUM PARTY CONFETTI EFFECT
    function startConfetti() {
      const canvas = document.getElementById("confettiCanvas");
      const ctx = canvas.getContext("2d");
      
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;

      const colors = ["#10B981", "#3B82F6", "#F59E0B", "#EF4444", "#8B5CF6", "#EC4899"];
      const particles = [];

      for (let i = 0; i < 120; i++) {
        particles.push({
          x: Math.random() * canvas.width,
          y: Math.random() * canvas.height - canvas.height,
          size: Math.random() * 8 + 5,
          color: colors[Math.floor(Math.random() * colors.length)],
          speedX: Math.random() * 3 - 1.5,
          speedY: Math.random() * 5 + 3,
          rotation: Math.random() * 360,
          rotationSpeed: Math.random() * 5 - 2.5
        });
      }

      let animationFrame;
      function animate() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        
        let alive = false;
        particles.forEach(p => {
          p.y += p.speedY;
          p.x += p.speedX;
          p.rotation += p.rotationSpeed;

          if (p.y < canvas.height) alive = true;

          ctx.save();
          ctx.translate(p.x, p.y);
          ctx.rotate((p.rotation * Math.PI) / 180);
          ctx.fillStyle = p.color;
          ctx.fillRect(-p.size / 2, -p.size / 2, p.size, p.size);
          ctx.restore();
        });

        if (alive) {
          animationFrame = requestAnimationFrame(animate);
        } else {
          ctx.clearRect(0, 0, canvas.width, canvas.height);
          cancelAnimationFrame(animationFrame);
        }
      }

      animate();

      // Clear after 4 seconds
      setTimeout(() => {
        cancelAnimationFrame(animationFrame);
        ctx.clearRect(0, 0, canvas.width, canvas.height);
      }, 4500);
    }
  </script>
</body>
</html>
