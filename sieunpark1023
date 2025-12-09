<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Treasure Hunt Art (Fixed Rotation Overflow)</title>
    <style>
        body, html { margin: 0; padding: 0; width: 100%; height: 100%; overflow: hidden; background: #000; }
        
        /* [추가됨] 로고 스타일 */
        #logo {
            cursor: pointer;
            position: fixed;
            top: 20px;
            left: 20px;
            font-size: 24px;
            font-weight: bold;
            font-family: 'BookkGothic', sans-serif;
            
            /* 배경이 어두운 비디오이므로 글자색을 흰색으로 변경했습니다. */
            color: #fff;
            
            /* 떠다니는 먼지보다 위에 오도록 높은 z-index 부여 */
            z-index: 100;
            
            /* 가독성을 위한 그림자 (선택사항) */
            text-shadow: 0 0 5px rgba(0,0,0,0.5);
            
            transition: color 0.3s ease, font-size 0.3s ease;
        }

        #logo:hover {
            font-size: 26px;
            color: #ccc; /* 호버 시 약간 회색으로 */
        }

        /* 비디오 스타일 */
        #bg-video {
            position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            object-fit: cover; z-index: 1; opacity: 0.8; transition: opacity 1s;
        }

        /* 보물찾기 버튼 (+) */
        #treasure-btn {
            position: absolute; background: transparent; 
            font-size: 25px; font-weight: 900; color: #fff; cursor: pointer;
            z-index: 2; user-select: none;
            text-shadow: 0 0 5px #fff, 0 0 10px #fff, 0 0 20px #ff00de, 0 0 40px #ff00de;
            transition: transform 0.2s;
        }
        #treasure-btn:hover { transform: scale(1.5) rotate(90deg); }

        /* 떠다니는 먼지들 */
        .dust-wrapper {
            position: absolute; z-index: 3; pointer-events: none;
        }

        .dust-photo {
            width: 100%; height: auto; display: block; 
            cursor: pointer; pointer-events: auto; 
            transition: transform 0.05s linear;
        }

        /* 팝업창 (모달) 스타일 */
        #modal {
            display: none; /* Flex로 켜지도록 스크립트에서 제어 */
            position: fixed; top: 50%; left: 50%;
            transform: translate(-50%, -50%); 
            
            /* 팝업창이 화면 밖으로 나가지 않게 최대 크기 제한 */
            max-width: 98vw;
            max-height: 98vh;
            
            padding: 15px;
            background: #ffffff; 
            border: 2px solid #000; 
            text-align: center; z-index: 10;
            box-shadow: 0 0 30px rgba(0, 0, 0, 0.5);
            
            /* 내용물 중앙 정렬 */
            flex-direction: column;
            align-items: center;
            justify-content: center;
            
            /* 혹시라도 내용이 넘치면 스크롤 생성 (안전장치) */
            overflow: auto;
        }

        /* [핵심 수정] 팝업 내부 이미지 - 넘침 방지 */
        #modal-img {
            /* 1. 이미지 공간을 화면 짧은 변 기준의 정사각형으로 강제합니다. */
            width: 80vmin;
            height: 80vmin;
            
            /* 2. 정사각형 공간 안에서 이미지 비율을 유지하며 꽉 차게 보여줍니다. */
            object-fit: contain;
            
            /* 3. 시계방향 90도 회전 (정사각형 안에서 돌므로 튀어나가지 않음) */
            transform: rotate(90deg); 
            
            margin: 0 auto 15px auto; /* 아래쪽 여백 추가 */
            border: 1px solid #ddd;
            display: block;
        }

        /* 닫기 버튼 */
        #close-btn {
            padding: 10px 25px; 
            background: #000; color: #fff;
            border: none; cursor: pointer; font-weight: bold; font-size: 16px;
            transition: background 0.3s;
            /* 버튼이 확실히 위에 오도록 설정 */
            position: relative; z-index: 20; 
            /* 이미지가 회전하면서 버튼 공간을 침범하지 않도록 마진 추가 */
            margin-top: 10px;
        }
        #close-btn:hover { background: #333; }
    </style>
</head>
<body>

    <div id="logo" onclick="goHome()">
        서사, 위기속에서
    </div>

    <video id="bg-video" autoplay muted loop playsinline>
        <source src="" type="video/mp4">
    </video>

    <div id="treasure-btn">+</div>
    
    <div id="dust-container"></div>

    <div id="modal">
        <img id="modal-img" src="" alt="Hidden Treasure">
        <button id="close-btn">닫기</button>
    </div>

    <script>
        /* [추가됨] 홈 이동 함수 */
        const targetUrl = 'https://mingyunbong-netizen.github.io/home/#';

        function goHome() {
            window.location.href = targetUrl;
        }

        /* ================= 먼지 사진들 ================= */
        const imageUrls = [
            'https://i.imgur.com/uNbYHgz.jpeg', 'https://i.imgur.com/yurzaYd.jpeg', 'https://i.imgur.com/fLFmSEz.jpeg', 'https://i.imgur.com/VUPBwNV.jpeg', 'https://i.imgur.com/ytR9hRr.jpeg',
            'https://i.imgur.com/fPilEC5.jpeg', 'https://i.imgur.com/IiDcQf8.jpeg', 'https://i.imgur.com/W0LnO4A.jpeg', 'https://i.imgur.com/80mdaYF.jpeg', 'https://i.imgur.com/2pQl0Ku.jpeg',
            'https://i.imgur.com/Y6sX136.jpeg', 'https://i.imgur.com/gdzv99k.jpeg', 'https://i.imgur.com/No4Xl9j.jpeg', 'https://i.imgur.com/5cy3od9.jpeg', 'https://i.imgur.com/g1ui0nQ.jpeg',
            'https://i.imgur.com/4V6HLhs.jpeg', 'https://i.imgur.com/5ER1hp8.jpeg', 'https://i.imgur.com/Yp7UQuH.jpeg', 'https://i.imgur.com/lImUBxV.jpeg', 'https://i.imgur.com/S58iunQ.jpeg',
            'https://i.imgur.com/iuKM4C6.jpeg', 'https://i.imgur.com/RvSgPUI.jpeg', 'https://i.imgur.com/zt9knbA.jpeg', 'https://i.imgur.com/QLObANu.jpeg', 'https://i.imgur.com/WQDVBGS.jpeg',
            'https://i.imgur.com/OnfeTmu.jpeg', 'https://i.imgur.com/cw0vnNM.jpeg', 'https://i.imgur.com/CR0hkUS.jpeg', 'https://i.imgur.com/imfHkAu.jpeg', 'https://i.imgur.com/eQnMlAE.jpeg',
            'https://i.imgur.com/fwNsCwD.jpeg', 'https://i.imgur.com/DIibhm9.jpeg', 'https://i.imgur.com/0imjVUH.jpeg', 'https://i.imgur.com/u1PRyWs.jpeg', 'https://i.imgur.com/SOgyQuS.jpeg',
            'https://i.imgur.com/2WWlaaU.jpeg', 'https://i.imgur.com/ty1g8Ph.jpeg', 'https://i.imgur.com/dDgCroe.jpeg', 'https://i.imgur.com/vsujkqe.jpeg', 'https://i.imgur.com/oOa1Dxw.jpeg',
            'https://i.imgur.com/Ca5R6t3.jpeg', 'https://i.imgur.com/dyrExtl.jpeg', 'https://i.imgur.com/W6io4lB.jpeg', 'https://i.imgur.com/yW59nCe.jpeg', 'https://i.imgur.com/T7JFH8h.jpeg',
            'https://i.imgur.com/tThOujV.jpeg', 'https://i.imgur.com/Ux6LcGG.jpeg', 'https://i.imgur.com/6GMBV4d.jpeg', 'https://i.imgur.com/RgjtVXV.jpeg', 'https://i.imgur.com/MOf6yfL.jpeg'
        ];


        /* ================= 설정 영역 ================= */
        const stages = [
            // 1단계
            { 
                video: 'https://i.imgur.com/Yby1eUx.mp4', 
                popupImage: 'https://i.imgur.com/x4tYCFH.png' 
            },
            // 2단계
            { 
                video: 'https://i.imgur.com/YFOmRKT.mp4', 
                popupImage: 'https://i.imgur.com/aHyGpiw.png' 
            },
            // 3단계
            { 
                video: 'https://i.imgur.com/RaZj9Ph.mp4', 
                popupImage: 'https://i.imgur.com/QtIYD8y.png' 
            },
            // 4단계
            { 
                video: 'https://i.imgur.com/POp3O6s.mp4', 
                popupImage: 'https://i.imgur.com/7IfZKvD.png' 
            },
            // 5단계
            { 
                video: 'https://i.imgur.com/Z8NBWd7.mp4', 
                popupImage: 'https://i.imgur.com/vr2T2ty.png' 
            }
        ];

        /* ================================================================= */
        const particleCount = 1200; 
        const repulseRadius = 90;

        let currentStageIndex = 0;
        const container = document.getElementById('dust-container');
        const treasureBtn = document.getElementById('treasure-btn');
        const modal = document.getElementById('modal');
        const videoElement = document.getElementById('bg-video');
        const modalImg = document.getElementById('modal-img'); 

        function initGame() {
            if(stages[0].video) videoElement.src = stages[0].video;

            container.innerHTML = ''; 

            for (let i = 0; i < particleCount; i++) {
                const wrapper = document.createElement('div');
                wrapper.classList.add('dust-wrapper');

                const img = document.createElement('img');
                img.classList.add('dust-photo');
                
                img.src = imageUrls[i % imageUrls.length];
                img.draggable = false; 
                
                const width = Math.random() * 30 + 20; 
                wrapper.style.width = `${width}px`;
                wrapper.style.left = `${Math.random() * 100}%`; 
                wrapper.style.top = `${Math.random() * 100}%`;

                const animDuration = Math.random() * 7 + 5; 
                const animDelay = Math.random() * -20;
                wrapper.style.animation = `float ${animDuration}s ease-in-out ${animDelay}s infinite alternate`;

                wrapper.appendChild(img);
                container.appendChild(wrapper);
            }
            moveTreasure();
        }

        const styleSheet = document.createElement("style");
        styleSheet.innerText = `
            @keyframes float {
                0% { transform: translate(0, 0); }
                100% { transform: translate(${Math.random()*40 - 20}px, ${Math.random()*40 - 20}px); }
            }
        `;
        document.head.appendChild(styleSheet);

        // 마우스 움직임 감지
        document.addEventListener('mousemove', (e) => {
            const photos = document.querySelectorAll('.dust-photo');
            const mouseX = e.clientX;
            const mouseY = e.clientY;

            photos.forEach(photo => {
                const rect = photo.getBoundingClientRect();
                const photoX = rect.left + rect.width / 2;
                const photoY = rect.top + rect.height / 2;
                const distX = mouseX - photoX;
                const distY = mouseY - photoY;
                const distance = Math.sqrt(distX * distX + distY * distY);

                if (distance < repulseRadius) {
                    const angle = Math.atan2(distY, distX);
                    const force = (repulseRadius - distance) / repulseRadius;
                    const moveX = Math.cos(angle) * force * -150; 
                    const moveY = Math.sin(angle) * force * -150;
                    photo.style.transform = `translate(${moveX}px, ${moveY}px)`;
                } else {
                    photo.style.transform = `translate(0, 0)`;
                }
            });
        });

        function moveTreasure() {
            treasureBtn.style.left = `${Math.random() * 90 + 5}%`;
            treasureBtn.style.top = `${Math.random() * 90 + 5}%`;
        }

        // 버튼 클릭 시 
        treasureBtn.addEventListener('click', () => {
            const wrappers = document.querySelectorAll('.dust-wrapper');
            wrappers.forEach(p => {
                p.style.opacity = '0'; 
                p.style.transition = 'opacity 0.5s';
            });
            treasureBtn.style.display = 'none';
            
            setTimeout(() => {
                const stage = stages[currentStageIndex];
                modalImg.src = stage.popupImage;
                // [수정됨] flex로 표시하여 중앙 정렬 적용
                modal.style.display = 'flex'; 
            }, 500);
        });

        // 닫기 버튼 클릭 시
        document.getElementById('close-btn').addEventListener('click', () => {
            modal.style.display = 'none';
            currentStageIndex = (currentStageIndex + 1) % stages.length;
            videoElement.src = stages[currentStageIndex].video; 
            
            const colors = ['#111', '#121', '#211', '#112', '#222'];
            videoElement.style.background = colors[currentStageIndex];

            const wrappers = document.querySelectorAll('.dust-wrapper');
            wrappers.forEach(p => { p.style.opacity = '1'; });
            
            moveTreasure();
            treasureBtn.style.display = 'block';
        });

        initGame();
    </script>
</body>
</html>
