<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>대한체육회 100주년 타임라인 (중앙 팝업형 모던 UI)</title>
    <style>
        /* 사용자 지정 폰트 (Paperozi) 로드 */
        @font-face { font-family: 'Paperozi'; src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/2408-3@1.0/Paperlogy-1Thin.woff2') format('woff2'); font-weight: 100; font-display: swap; }
        @font-face { font-family: 'Paperozi'; src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/2408-3@1.0/Paperlogy-2ExtraLight.woff2') format('woff2'); font-weight: 200; font-display: swap; }
        @font-face { font-family: 'Paperozi'; src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/2408-3@1.0/Paperlogy-3Light.woff2') format('woff2'); font-weight: 300; font-display: swap; }
        @font-face { font-family: 'Paperozi'; src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/2408-3@1.0/Paperlogy-4Regular.woff2') format('woff2'); font-weight: 400; font-display: swap; }
        @font-face { font-family: 'Paperozi'; src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/2408-3@1.0/Paperlogy-5Medium.woff2') format('woff2'); font-weight: 500; font-display: swap; }
        @font-face { font-family: 'Paperozi'; src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/2408-3@1.0/Paperlogy-6SemiBold.woff2') format('woff2'); font-weight: 600; font-display: swap; }
        @font-face { font-family: 'Paperozi'; src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/2408-3@1.0/Paperlogy-7Bold.woff2') format('woff2'); font-weight: 700; font-display: swap; }
        @font-face { font-family: 'Paperozi'; src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/2408-3@1.0/Paperlogy-8ExtraBold.woff2') format('woff2'); font-weight: 800; font-display: swap; }
        @font-face { font-family: 'Paperozi'; src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/2408-3@1.0/Paperlogy-9Black.woff2') format('woff2'); font-weight: 900; font-display: swap; }

        /* 기본 스타일 초기화 */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Paperozi', sans-serif;
            -webkit-tap-highlight-color: transparent; 
        }

        :root {
            --bg-color: #F4F6F9; 
            --primary-blue: #0059B3; 
            --inactive-gray: #C4CDD5; 
            --text-dark: #333333;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-dark);
            width: 100vw;
            height: 100vh;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            position: relative;
        }

        /* 🌟 배경 이미지 영역 */
        #intro-bg, #active-bg {
            position: absolute;
            top: 0; left: 0; width: 100vw; height: 100vh;
            background-size: cover; background-position: center; background-repeat: no-repeat;
            z-index: 0; transition: opacity 0.6s ease-in-out; pointer-events: none;
        }
        
        #intro-bg { background-image: url('intro.png'); opacity: 1; }
        #intro-bg.hidden { opacity: 0; }

        #active-bg { background-image: url(''); opacity: 0; }
        #active-bg.visible { opacity: 1; }

        /* 타임라인 메인 래퍼 */
        #timeline-viewport {
            flex: 1;
            width: 100vw;
            overflow-x: auto;
            overflow-y: hidden;
            scroll-behavior: smooth;
            -webkit-overflow-scrolling: touch;
            position: relative;
            display: flex;
            align-items: center;
            cursor: grab; 
            user-select: none; 
            z-index: 5; 
        }
        #timeline-viewport:active { cursor: grabbing; }
        #timeline-viewport::-webkit-scrollbar { display: none; }

        #timeline-track {
            display: flex;
            height: 80vh; 
            position: relative;
            padding: 0 50vw; 
            /* 🌟 [신규] 팝업이 떴을 때 뒤쪽 연표가 부드럽게 흐려지도록 애니메이션 설정 */
            transition: all 0.5s ease;
        }

        /* 🌟 [신규] 팝업이 떴을 때 배경(연표)을 흐리게 만들어서 헷갈림 방지 */
        body.modal-open #timeline-track {
            opacity: 0.15;
            filter: blur(10px);
            pointer-events: none;
        }

        /* 🌈 중앙 무한 가로선 */
        #center-line {
            position: absolute;
            top: 50%;
            left: 0;
            width: 100%;
            height: 3px;
            background: linear-gradient(90deg, #0081C8, #FCB131, #000000, #00A651, #EE334E);
            background-size: 50vw 100%;
            transform: translateY(-50%);
            z-index: 1;
            opacity: 0.6;
        }

        .timeline-item {
            width: 18vw; 
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            z-index: 2;
        }

        .node-area {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 3.5vh;
            height: 3.5vh;
            z-index: 3; 
            cursor: pointer; 
        }

        .dot {
            width: 100%; 
            height: 100%; 
            background: #ffffff;
            border: 3px solid var(--inactive-gray);
            border-radius: 50%;
            transition: all 0.4s ease;
            position: relative;
            z-index: 2;
        }

        .year-label {
            position: absolute;
            font-size: 2.5vh;
            font-weight: 700;
            color: #88929D;
            transition: all 0.4s ease;
            letter-spacing: 0.05vw;
            white-space: nowrap;
        }
        .align-top .year-label { bottom: 7vh; left: 50%; transform: translateX(-50%); }
        .align-bottom .year-label { top: 7vh; left: 50%; transform: translateX(-50%); }

        .vertical-line {
            position: absolute;
            left: 50%;
            width: 2px;
            background: var(--inactive-gray);
            transition: height 0.3s ease, background 0.3s ease;
            transform: translateX(-50%);
            z-index: 1;
        }
        .align-top .vertical-line { bottom: 50%; height: 5vh; } 
        .align-bottom .vertical-line { top: 50%; height: 5vh; }    

        /* =======================================
           ✨ 활성화(클릭) 상태 스타일
           ======================================= */
        .timeline-item.active .dot {
            border-color: var(--primary-blue);
            border-width: 4px;
            transform: scale(1.3);
            box-shadow: 0 4px 15px rgba(0, 89, 179, 0.3);
        }

        .timeline-item.active .year-label {
            color: var(--primary-blue);
            font-weight: 900;
        }
        .timeline-item.active.align-top .year-label { transform: translateX(-50%) scale(1.2); }
        .timeline-item.active.align-bottom .year-label { transform: translateX(-50%) scale(1.2); }

        .timeline-item.active .vertical-line { background: var(--primary-blue); height: 6vh; }

        .timeline-item.adjacent .dot { border-color: #A0ABB5; }

        /* =======================================
           🚀 화면 한가운데 거대한 하얀색 네모 모달 팝업 CSS 
           ======================================= */
        #image-modal {
            position: fixed;
            top: 0; left: 0;
            width: 100vw; height: 100vh;
            z-index: 100;
            display: flex;
            flex-direction: column; 
            align-items: center;
            justify-content: center;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.4s ease;
            background: transparent; 
        }

        #image-modal.visible {
            opacity: 1;
            pointer-events: auto;
        }

        /* 🌟 [신규] 팝업 뒤로 지나가는 무지개 연대표 선 */
        #modal-center-line {
            position: absolute;
            top: 50%;
            left: 0;
            width: 100%;
            height: 3px;
            background: linear-gradient(90deg, #0081C8, #FCB131, #000000, #00A651, #EE334E);
            background-size: 50vw 100%;
            transform: translateY(-50%) scaleX(0); /* 처음엔 길이가 0 (숨김) */
            transform-origin: center;
            transition: transform 0.6s cubic-bezier(0.16, 1, 0.3, 1);
            z-index: 0;
            opacity: 0.6;
        }

        #image-modal.visible #modal-center-line {
            transform: translateY(-50%) scaleX(1); /* 팝업이 열릴 때 양옆으로 뻗어나감 */
        }

        /* 🌟 하얀색 둥근 네모 박스 래퍼 */
        #modal-content-wrapper {
            background: #ffffff; 
            border-radius: 32px; 
            padding: 2.5vh; 
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.15), 0 0 0 4px var(--primary-blue); 
            transform: scale(0.85); 
            transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1);
            display: inline-block; 
            line-height: 0; 
            position: relative; 
            z-index: 2; /* 🌟 추가: 무지개 선보다 앞에 오도록 설정 */
        }

        #image-modal.visible #modal-content-wrapper { transform: scale(1); }

        /* 🌟 팝업 박스 밖의 연도 타이틀 텍스트 스타일 */
        #modal-year {
            font-size: 5vh;
            font-weight: 900;
            color: var(--primary-blue); 
            text-shadow: 0 0 15px rgba(255, 255, 255, 0.9), 0 2px 4px rgba(0, 0, 0, 0.1); 
            margin-bottom: 2vh; 
            letter-spacing: 0.1vw;
            text-align: center;
            transform: scale(0.85); 
            transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1);
            line-height: 1;
            position: relative;
            z-index: 2; /* 🌟 추가: 무지개 선보다 앞에 오도록 설정 */
        }

        #image-modal.visible #modal-year { transform: scale(1); }

        #modal-image {
            /* 🌟 [수정] 박스 세로 크기를 살짝 줄여서 아래 X 버튼이 들어갈 공간을 넉넉히 확보 */
            max-width: 80vw; 
            max-height: 68vh; 
            width: auto;
            height: auto;
            display: block;
            -webkit-user-drag: none;
        }

        /* =======================================
           🚀 [수정] 네비게이션 버튼에 연도(-1, +1) 글씨 추가!
           ======================================= */
        .nav-btn {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            height: 6vh;
            padding: 0 2.5vh; /* 🌟 넓은 알약 모양으로 변경 */
            background: rgba(255, 255, 255, 0.85); 
            backdrop-filter: blur(5px); 
            border-radius: 3vh; /* 둥근 모서리 */
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 1.5vh; /* 화살표와 연도 숫자 사이의 간격 */
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(0,0,0,0.15);
            transition: all 0.2s ease;
            z-index: 101;
            color: var(--primary-blue);
            font-size: 2.2vh;
            font-weight: 800; /* 연도가 잘 보이게 볼드체 */
            white-space: nowrap; /* 글씨가 줄바꿈 되지 않게 고정 */
        }

        .nav-btn:hover {
            background: rgba(255, 255, 255, 1);
            transform: translateY(-50%) scale(1.05);
        }

        /* 🌟 화살표를 박스 바깥쪽에 여유 있게 배치 */
        #prev-btn { right: calc(100% + 2vw); left: auto; }
        #next-btn { left: calc(100% + 2vw); right: auto; }

        .arrow-icon {
            display: inline-block;
            border: solid var(--primary-blue);
            border-width: 0 4px 4px 0;
            padding: 5px;
        }
        /* 화살표 위치 미세 조정 */
        #prev-btn .arrow-icon { transform: rotate(135deg); margin-bottom: 2px;}
        #next-btn .arrow-icon { transform: rotate(-45deg); margin-bottom: 2px;}

        /* 하단 닫기(X) 버튼 */
        #close-btn {
            position: absolute;
            /* 🌟 [수정] X 버튼을 박스 쪽으로 아주 살짝 더 당겨서 안정감 있게 배치 */
            bottom: -7vh; 
            left: 50%;
            transform: translateX(-50%);
            width: 6vh;
            height: 6vh;
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(5px);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(0,0,0,0.15);
            transition: all 0.2s ease;
            z-index: 101;
        }

        #close-btn:hover {
            background: rgba(255, 255, 255, 1);
            transform: translateX(-50%) scale(1.1);
        }

        .close-icon {
            position: relative;
            width: 2.5vh;
            height: 2.5vh;
        }
        .close-icon::before, .close-icon::after {
            content: '';
            position: absolute;
            top: 50%;
            left: 0;
            width: 100%;
            height: 4px;
            background-color: var(--primary-blue);
            border-radius: 2px;
        }
        .close-icon::before { transform: rotate(45deg); }
        .close-icon::after { transform: rotate(-45deg); }
        
    </style>
</head>
<body>

    <div id="intro-bg"></div>
    <div id="active-bg"></div>

    <div id="timeline-viewport">
        <div id="timeline-track">
            <div id="center-line"></div>
            <!-- 자바스크립트로 생성될 아이템 영역 -->
        </div>
    </div>

    <!-- 🌟 화면 중앙 팝업 모달 영역 -->
    <div id="image-modal">
        <!-- 🌟 [신규] 팝업 뒤를 가로지르는 무지개 선 -->
        <div id="modal-center-line"></div>

        <div id="modal-year"></div> 
        <div id="modal-content-wrapper">
            <img id="modal-image" src="" alt="Year Information">
            
            <!-- 🌟 [수정] 화살표 버튼 안에 -1, +1 연도를 표시할 텍스트 영역 추가! -->
            <div id="prev-btn" class="nav-btn">
                <span class="arrow-icon prev"></span>
                <span id="prev-year-text"></span>
            </div>
            <div id="next-btn" class="nav-btn">
                <span id="next-year-text"></span>
                <span class="arrow-icon next"></span>
            </div>

            <!-- 닫기 버튼 -->
            <div id="close-btn"><div class="close-icon"></div></div>
        </div>
    </div>

    <script>
        // ==========================================
        // [데이터 설정 구간]
        // ==========================================
        const olympicData = [
            { year: 1920, fileName: "1920.png" },
            { year: 1921, fileName: "1921.png" },
            { year: 1922, fileName: "1922.png" },
            { year: 1923, fileName: "1923.png" },
            { year: 1924, fileName: "1924.png" },
            // 🌟 임시 테스트용 이미지들
            { year: 1925, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1925+Giant+Modal+PNG" },
            { year: 1926, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1926+Giant+Modal+PNG" },
            { year: 1927, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1927+Giant+Modal+PNG" },
            { year: 1928, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1928+Giant+Modal+PNG" },
            { year: 1929, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1929+Giant+Modal+PNG" },
            { year: 1930, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1930+Giant+Modal+PNG" },
            { year: 1931, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1931+Giant+Modal+PNG" },
            { year: 1932, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1932+Giant+Modal+PNG" },
            { year: 1933, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1933+Giant+Modal+PNG" },
            { year: 1934, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1934+Giant+Modal+PNG" },
            { year: 1935, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1935+Giant+Modal+PNG" },
            { year: 1936, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1936+Giant+Modal+PNG" },
            { year: 1937, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1937+Giant+Modal+PNG" },
            { year: 1938, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1938+Giant+Modal+PNG" },
            { year: 1939, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1939+Giant+Modal+PNG" },
            { year: 1940, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1940+Giant+Modal+PNG" },
            { year: 1945, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1945+Giant+Modal+PNG" },
            { year: 1946, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1946+Giant+Modal+PNG" },
            { year: 1947, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1947+Giant+Modal+PNG" },
            { year: 1948, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1948+Giant+Modal+PNG" },
            { year: 1949, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1949+Giant+Modal+PNG" },
            { year: 1950, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1950+Giant+Modal+PNG" },
            { year: 1951, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1951+Giant+Modal+PNG" },
            { year: 1952, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1952+Giant+Modal+PNG" },
            { year: 1953, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1953+Giant+Modal+PNG" },
            { year: 1954, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1954+Giant+Modal+PNG" },
            { year: 1955, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1955+Giant+Modal+PNG" },
            { year: 1956, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1956+Giant+Modal+PNG" },
            { year: 1957, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1957+Giant+Modal+PNG" },
            { year: 1958, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1958+Giant+Modal+PNG" },
            { year: 1959, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1959+Giant+Modal+PNG" },
            { year: 1960, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1960+Giant+Modal+PNG" },
            { year: 1964, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1964+Giant+Modal+PNG" },
            { year: 1965, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1965+Giant+Modal+PNG" },
            { year: 1966, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1966+Giant+Modal+PNG" },
            { year: 1967, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1967+Giant+Modal+PNG" },
            { year: 1968, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1968+Giant+Modal+PNG" },
            { year: 1969, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1969+Giant+Modal+PNG" },
            { year: 1970, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1970+Giant+Modal+PNG" },
            { year: 1971, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1971+Giant+Modal+PNG" },
            { year: 1972, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1972+Giant+Modal+PNG" },
            { year: 1973, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1973+Giant+Modal+PNG" },
            { year: 1974, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1974+Giant+Modal+PNG" },
            { year: 1975, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1975+Giant+Modal+PNG" },
            { year: 1976, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1976+Giant+Modal+PNG" },
            { year: 1977, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1977+Giant+Modal+PNG" },
            { year: 1978, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1978+Giant+Modal+PNG" },
            { year: 1979, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1979+Giant+Modal+PNG" },
            { year: 1980, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1980+Giant+Modal+PNG" },
            { year: 1981, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1981+Giant+Modal+PNG" },
            { year: 1982, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1982+Giant+Modal+PNG" },
            { year: 1983, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1983+Giant+Modal+PNG" },
            { year: 1984, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1984+Giant+Modal+PNG" },
            { year: 1985, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1985+Giant+Modal+PNG" },
            { year: 1986, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1986+Giant+Modal+PNG" },
            { year: 1988, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1988+Giant+Modal+PNG" },
            { year: 1989, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1989+Giant+Modal+PNG" },
            { year: 1990, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1990+Giant+Modal+PNG" },
            { year: 1991, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1991+Giant+Modal+PNG" },
            { year: 1992, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1992+Giant+Modal+PNG" },
            { year: 1993, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1993+Giant+Modal+PNG" },
            { year: 1994, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1994+Giant+Modal+PNG" },
            { year: 1995, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1995+Giant+Modal+PNG" },
            { year: 1996, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1996+Giant+Modal+PNG" },
            { year: 1997, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1997+Giant+Modal+PNG" },
            { year: 1998, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1998+Giant+Modal+PNG" },
            { year: 1999, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=1999+Giant+Modal+PNG" },
            { year: 2000, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2000+Giant+Modal+PNG" },
            { year: 2001, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2001+Giant+Modal+PNG" },
            { year: 2002, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2002+Giant+Modal+PNG" },
            { year: 2003, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2003+Giant+Modal+PNG" },
            { year: 2004, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2004+Giant+Modal+PNG" },
            { year: 2005, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2005+Giant+Modal+PNG" },
            { year: 2006, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2006+Giant+Modal+PNG" },
            { year: 2007, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2007+Giant+Modal+PNG" },
            { year: 2008, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2008+Giant+Modal+PNG" },
            { year: 2009, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2009+Giant+Modal+PNG" },
            { year: 2010, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2010+Giant+Modal+PNG" },
            { year: 2011, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2011+Giant+Modal+PNG" },
            { year: 2012, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2012+Giant+Modal+PNG" },
            { year: 2013, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2013+Giant+Modal+PNG" },
            { year: 2014, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2014+Giant+Modal+PNG" },
            { year: 2015, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2015+Giant+Modal+PNG" },
            { year: 2016, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2016+Giant+Modal+PNG" },
            { year: 2017, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2017+Giant+Modal+PNG" },
            { year: 2018, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2018+Giant+Modal+PNG" },
            { year: 2019, fileName: "https://placehold.co/1200x1000/transparent/0059B3?text=2019+Giant+Modal+PNG" }
        ];

        const track = document.getElementById('timeline-track');
        const introBg = document.getElementById('intro-bg'); 
        const activeBg = document.getElementById('active-bg'); 
        const imageModal = document.getElementById('image-modal');
        const modalImage = document.getElementById('modal-image');
        const modalYear = document.getElementById('modal-year'); 
        
        // 네비게이션 및 닫기 버튼
        const prevBtn = document.getElementById('prev-btn');
        const nextBtn = document.getElementById('next-btn');
        const closeBtn = document.getElementById('close-btn');

        let activeIndex = -1;
        let lastIndex = -1; 

        let autoSlideDuration = 60000;   
        let manualSlideDuration = 180000; 
        let idleDelay = 30000;           
        let autoPlayTimer = null;
        let isUserViewing = false;       

        function playNext() {
            isUserViewing = false; 
            let nextIndex = (lastIndex + 1) % olympicData.length;
            const items = document.querySelectorAll('.timeline-item');
            if(items[nextIndex]) {
                activateItem(items[nextIndex], nextIndex);
            }
        }

        function resetIdleTimer() {
            clearTimeout(autoPlayTimer);
            if (!isDown) {
                if (activeIndex !== -1) {
                    let delay = isUserViewing ? manualSlideDuration : idleDelay;
                    autoPlayTimer = setTimeout(playNext, delay);
                } else {
                    autoPlayTimer = setTimeout(playNext, idleDelay);
                }
            }
        }

        function setSlideTimer() {
            clearTimeout(autoPlayTimer);
            let delay = isUserViewing ? manualSlideDuration : autoSlideDuration;
            autoPlayTimer = setTimeout(playNext, delay);
        }

        function closeAllItems() {
            activeIndex = -1;
            isUserViewing = false; 
            document.querySelectorAll('.timeline-item').forEach(item => {
                item.classList.remove('active', 'adjacent');
            });
            
            introBg.classList.remove('hidden'); 
            activeBg.classList.remove('visible');
            
            imageModal.classList.remove('visible');
            
            // 🌟 팝업이 닫히면 다시 타임라인 궤도를 선명하게 복구하고 활성화
            document.body.classList.remove('modal-open');
            
            resetIdleTimer(); 
        }

        imageModal.addEventListener('click', (e) => {
            if(e.target === imageModal) {
                closeAllItems();
            }
        });

        closeBtn.addEventListener('click', (e) => {
            e.stopPropagation(); 
            closeAllItems();
        });

        prevBtn.addEventListener('click', (e) => {
            e.stopPropagation();
            if (activeIndex > 0) {
                const items = document.querySelectorAll('.timeline-item');
                isUserViewing = true;
                activateItem(items[activeIndex - 1], activeIndex - 1);
            }
        });

        nextBtn.addEventListener('click', (e) => {
            e.stopPropagation();
            if (activeIndex < olympicData.length - 1) {
                const items = document.querySelectorAll('.timeline-item');
                isUserViewing = true;
                activateItem(items[activeIndex + 1], activeIndex + 1);
            }
        });

        const viewport = document.getElementById('timeline-viewport');
        let isDown = false;
        let startX;
        let scrollLeft;
        let isDragging = false; 

        viewport.addEventListener('mousedown', (e) => {
            isDown = true;
            isDragging = false;
            resetIdleTimer();
            startX = e.pageX - viewport.offsetLeft;
            scrollLeft = viewport.scrollLeft;
        });

        viewport.addEventListener('mouseleave', () => {
            isDown = false;
            resetIdleTimer();
        });

        viewport.addEventListener('mouseup', (e) => {
            isDown = false;
            resetIdleTimer();
        });

        viewport.addEventListener('mousemove', (e) => {
            if (!isDown) return;
            e.preventDefault();
            resetIdleTimer();
            const x = e.pageX - viewport.offsetLeft;
            const walk = (x - startX) * 1.5; 
            
            if (Math.abs(walk) > 5) {
                isDragging = true; 
            }
            viewport.scrollLeft = scrollLeft - walk;
        });

        viewport.addEventListener('touchstart', () => { isDown = true; resetIdleTimer(); }, {passive: true});
        viewport.addEventListener('touchend', () => { isDown = false; resetIdleTimer(); });
        viewport.addEventListener('touchmove', resetIdleTimer, {passive: true});
        viewport.addEventListener('wheel', resetIdleTimer, {passive: true});

        function initTimeline() {
            olympicData.forEach((data, index) => {
                const isTopAlign = index % 2 === 0;
                
                const itemWrapper = document.createElement('div');
                itemWrapper.className = `timeline-item ${isTopAlign ? 'align-top' : 'align-bottom'}`;
                
                const nodeArea = document.createElement('div');
                nodeArea.className = 'node-area';
                nodeArea.innerHTML = `
                    <div class="vertical-line"></div>
                    <div class="dot"></div>
                    <div class="year-label">${data.year}</div>
                `;

                itemWrapper.appendChild(nodeArea);

                nodeArea.addEventListener('click', (e) => {
                    if (isDragging) return;
                    e.stopPropagation();
                    
                    if (activeIndex === index) {
                        closeAllItems(); 
                    } else {
                        isUserViewing = true; 
                        activateItem(itemWrapper, index);
                    }
                });

                track.appendChild(itemWrapper);
            });
        }

        function activateItem(selectedElement, index) {
            activeIndex = index;
            lastIndex = index;

            // 🌟 팝업이 뜰 때 배경의 타임라인을 뿌옇게 블러 처리하여 시선을 차단
            document.body.classList.add('modal-open');

            introBg.classList.add('hidden');
            activeBg.classList.add('visible');

            // 중앙 팝업 모달 데이터 세팅
            modalYear.innerText = olympicData[index].year; 
            modalImage.src = olympicData[index].fileName;
            imageModal.classList.add('visible');

            // 🌟 이전(-1) 버튼: 맨 처음 데이터가 아니면 -1 연도 숫자 표시 및 활성화
            if (index > 0) {
                prevBtn.style.opacity = "1";
                prevBtn.style.pointerEvents = "auto";
                document.getElementById('prev-year-text').innerText = olympicData[index - 1].year;
            } else {
                prevBtn.style.opacity = "0";
                prevBtn.style.pointerEvents = "none";
            }
            
            // 🌟 다음(+1) 버튼: 맨 마지막 데이터가 아니면 +1 연도 숫자 표시 및 활성화
            if (index < olympicData.length - 1) {
                nextBtn.style.opacity = "1";
                nextBtn.style.pointerEvents = "auto";
                document.getElementById('next-year-text').innerText = olympicData[index + 1].year;
            } else {
                nextBtn.style.opacity = "0";
                nextBtn.style.pointerEvents = "none";
            }

            setSlideTimer(); 

            document.querySelectorAll('.timeline-item').forEach((item, i) => {
                item.classList.remove('active', 'adjacent');
                if (i === index) {
                    item.classList.add('active'); 
                } else if (i === index - 1 || i === index + 1) {
                    item.classList.add('adjacent'); 
                }
            });

            selectedElement.scrollIntoView({
                behavior: 'smooth',
                inline: 'center',
                block: 'nearest'
            });
        }

        initTimeline();
        resetIdleTimer();

    </script>
</body>
</html>
