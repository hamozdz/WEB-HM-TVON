<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>سكربتاتTVON</title>
  <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap" rel="stylesheet">
  <style>
    /* Global styles and body/html setup */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    body, html {
      width: 100%;
      min-height: 100%;
      font-family: 'Cairo', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      overflow-x: hidden;
      overflow-y: auto;
    }

    /* Loading screen styles */
    #loading-screen {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 100%;
      height: 100%;
      background-color: #000;
      color: #fff;
      font-size: 1.5rem;
      visibility: visible;
      position: fixed;
      top: 0;
      left: 0;
      z-index: 9999;
    }

    /* Main content container */
    #main-content {
      display: none;
      width: 100%;
      min-height: 100vh;
      background: linear-gradient(-45deg, #0f0c29, #302b63, #24243e, #000);
      background-size: 400% 400%;
      animation: gradientBG 15s ease infinite;
      color: #fff;
      display: flex;
      justify-content: flex-start;
      align-items: center;
      flex-direction: column;
      padding-top: 50px;
      padding-bottom: 50px;
      gap: 20px;
    }

    /* Background gradient animation */
    @keyframes gradientBG {
      0% {background-position: 0% 50%;}
      50% {background-position: 100% 50%;}
      100% {background-position: 0% 50%;}
    }

    /* Fade-in animation for initial elements */
    .fade-in {
      opacity: 0;
      animation: fadeIn 2s ease-in-out forwards;
    }
    @keyframes fadeIn {
      to {
        opacity: 1;
      }
    }

    /* Glow effect for clickable title */
    .glow {
      color: #00ffcc;
      text-shadow: 0 0 10px #00ffcc, 0 0 20px #00ffcc, 0 0 30px #00ffcc;
      cursor: pointer;
      transition: transform 0.3s ease;
    }
    .glow:active {
      transform: scale(0.8);
    }

    /* Button styles */
    #tvon-button {
      display: none;
      padding: 10px 20px;
      font-size: 1.2rem;
      background-color: #00ffcc;
      color: #000;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      transition: background-color 0.3s, transform 0.1s ease-out, box-shadow 0.3s;
      font-family: 'Cairo', sans-serif;
      font-weight: 700;
      box-shadow: 0 5px 15px rgba(0, 255, 204, 0.4);
    }
    #tvon-button:hover {
      background-color: #00ddaa;
      box-shadow: 0 5px 20px rgba(0, 255, 204, 0.6);
    }
    #tvon-button:active {
      transform: scale(0.95);
      background-color: #00bbaa;
      box-shadow: 0 2px 8px rgba(0, 255, 204, 0.7);
    }

    /* Description section styles */
    #description-section {
      width: 80%;
      max-width: 800px;
      margin-top: 40px;
      text-align: justify;
      line-height: 1.8;
      font-size: 1.1rem;
      padding: 20px;
      background-color: rgba(0, 0, 0, 0.3);
      border-radius: 15px;
      box-shadow: 0 0 20px rgba(0, 255, 204, 0.2);
      margin-bottom: 50px;
      transition: opacity 0.5s ease-out, transform 0.5s ease-out;
    }
    #description-section.hidden {
      opacity: 0;
      transform: translateY(20px);
      pointer-events: none;
      height: 0;
      overflow: hidden;
      padding: 0;
      margin-top: 0;
      margin-bottom: 0;
    }
    #description-section h2 {
      text-align: center;
      color: #00ffcc;
      margin-bottom: 20px;
      font-size: 1.8rem;
      text-shadow: 0 0 5px #00ffcc;
    }
    #description-section p {
      margin-bottom: 15px;
    }

    /* Footer text styles */
    #footer-text {
      width: 100%;
      text-align: center;
      font-size: 0.9rem;
      color: rgba(255, 255, 255, 0.7);
      font-family: 'Cairo', sans-serif;
      padding-top: 20px;
    }

    /* Copy message styles */
    #copy-message {
      position: fixed;
      bottom: 80px;
      left: 50%;
      transform: translateX(-50%);
      background-color: rgba(0, 0, 0, 0.7);
      color: #00ffcc;
      padding: 10px 15px;
      border-radius: 8px;
      font-size: 1rem;
      opacity: 0;
      transition: opacity 0.5s ease-in-out;
      z-index: 1000;
    }
    #copy-message.show {
      opacity: 1;
    }

    /* Instruction text styles */
    #instruction-text {
      font-size: 0.9rem;
      color: rgba(255, 255, 255, 0.8);
      margin-top: -10px;
      text-align: center;
      opacity: 0;
      animation: fadeIn 2s ease-in-out forwards;
      animation-delay: 0.5s;
    }

    /* Supported maps section styles */
    #supported-maps-section {
      display: none;
      width: 80%;
      max-width: 800px;
      margin-top: 40px;
      padding: 20px;
      background-color: rgba(0, 0, 0, 0.3);
      border-radius: 15px;
      box-shadow: 0 0 20px rgba(0, 255, 204, 0.2);
      margin-bottom: 50px;
      opacity: 0;
      transform: translateY(20px);
      transition: opacity 0.8s ease-out, transform 0.8s ease-out;
    }
    #supported-maps-section.show {
      display: block;
      opacity: 1;
      transform: translateY(0);
    }
    #supported-maps-section h2 {
      text-align: center;
      color: #00ffcc;
      margin-bottom: 20px;
      font-size: 1.8rem;
      text-shadow: 0 0 5px #00ffcc;
    }
    .map-item {
      margin-bottom: 25px;
      padding-bottom: 15px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }
    .map-item:last-child {
      border-bottom: none;
      margin-bottom: 0;
      padding-bottom: 0;
    }
    .map-item h3 {
      color: #00ffcc;
      font-size: 1.4rem;
      margin-bottom: 5px;
      cursor: pointer; /* Make map titles clickable */
      transition: color 0.2s ease;
    }
    .map-item h3:hover {
      color: #00ddaa; /* Slightly darker on hover */
    }
    .map-item p {
      font-size: 1rem;
      line-height: 1.6;
      color: rgba(255, 255, 255, 0.9);
    }
    .map-image { /* Styles for the map images */
      max-width: 100%;
      height: auto;
      border-radius: 10px;
      margin-top: 15px;
      box-shadow: 0 0 10px rgba(0, 255, 204, 0.3);
      display: none; /* Hidden by default */
      opacity: 0; /* For fade-in animation */
      transform: scale(0.95); /* For slight zoom-in animation */
      transition: opacity 0.5s ease-out, transform 0.5s ease-out;
    }
    .map-image.show-image { /* Class to show image with animation */
      display: block;
      opacity: 1;
      transform: scale(1);
    }
  </style>
</head>
<body>
  <div id="loading-screen">انتظر قليلاً للتحميل...</div>
  <div id="main-content">
    <h1 id="title" class="fade-in glow" style="text-align:center; font-size: 2.5rem;">
      مرحباً بك في سكربتاتTVON
    </h1>
    <p id="instruction-text">اضغط على نص الترحيب لتجد سكربتات</p>

    <button id="tvon-button">سكربتات TVON حالية</button>

    <div id="description-section" class="fade-in">
      <h2>حول سكربتات TVON</h2>
      <p>
        سكربتات TVON، أو TVON SCRIPTS، هي مجموعة من البرامج النصية القوية والمبتكرة التي تم تطويرها وصيانتها بواسطة @unclesnoobe، وهو المالك والمطور الرئيسي في مجتمع ديسكورد الخاص بنا. هذه السكربتات مصممة خصيصًا لتقديم تجربة لعب فريدة ومحسّنة لمستخدمينا في مجموعة واسعة من الألعاب.
      </p>
      <p>
        نحن نركز على تقديم محتوى متنوع يشمل الألعاب العربية والأجنبية على حد سواء، مما يضمن أن يجد كل لاعب ما يناسب اهتماماته. يتميز عملنا بالتحديثات المستمرة والدورية، حيث نلتزم بتقديم أحدث الإصلاحات والتحسينات لضمان أفضل أداء واستقرار ممكن للسكربتات.
      </p>
      <p>
        في حال ظهور أي خطأ بسيط أو مشكلة فنية، فإن فريقنا يعمل بسرعة فائقة على تحديد السبب وإصلاحه، لتقليل أي تأثير على تجربة المستخدم. هدفنا هو توفير بيئة لعب سلسة وخالية من المتاعب قدر الإمكان.
      </p>
      <p>
        إحدى أبرز مميزات سكربتات TVON هي قدرتها على تجاوز الحماية في الألعاب قدر المستطاع. نحن نبذل قصارى جهدنا لتوفير حلول تمكن المستخدمين من الاستمتاع بميزات السكربتات حتى في الألعاب التي تحتوي على أنظمة حماية معقدة، مع الحفاظ على الأمان والاستقرار.
      </p>
      <p>
        نحن نؤمن بأن الابتكار والتطوير المستمر هما مفتاح النجاح، ولذلك فإننا نستمع دائمًا إلى ملاحظات مجتمعنا ونعمل على دمج الأفكار الجديدة والميزات المطلوبة. انضم إلينا اليوم لتكتشف عالماً جديداً من الإمكانيات في ألعابك المفضلة مع سكربتات TVON.
      </p>
    </div>

    <div id="supported-maps-section">
      <h2>المابات المدعومة</h2>
      <div class="map-item">
        <h3 id="mm2-title">MM2</h3>
        <p>
          سكربتات MM2 توفر لك تجربة لعب محسّنة في لعبة Murder Mystery 2. استمتع بميزات فريدة تساعدك على التفوق، سواء كنت بريئاً، قاتلاً، أو شريفاً. تتضمن الميزات تحسينات في الرؤية، والتحرك السريع، والقدرة على كشف الأدوار، مما يمنحك الأفضلية في كل جولة.
        </p>
        <img id="mm2-image" class="map-image" src="https://tr.rbxcdn.com/142823291/420/420/Image/Png" alt="[attachment_0](attachment)" onerror="this.onerror=null;this.src='https://placehold.co/300x200/000/FFF?text=MM2+Image';" />
      </div>
      <div class="map-item">
        <h3 id="timebomb-title">Timebomb</h3>
        <p>
          في لعبة Timebomb، سكربتاتنا تمنحك أدوات استراتيجية للتحكم في مجريات اللعب. سواء كنت تسعى لتفجير القنبلة أو إبطالها، توفر لك هذه السكربتات معلومات حيوية وتحسينات في الأداء لمساعدتك على اتخاذ القرارات الصحيحة في الوقت المناسب وتحقيق النصر.
        </p>
        <img id="timebomb-image" class="map-image" src="https://t4.rbxcdn.com/f9c8f072c4ce126932a392815152862a" alt="[attachment_1](attachment)" onerror="this.onerror=null;this.src='https://placehold.co/300x200/000/FFF?text=Timebomb+Image';" />
      </div>
      <div class="map-item">
        <h3 id="brookhaven-title">BROOKHAVEN</h3>
        <p>
          سكربتات BROOKHAVEN تفتح لك عالماً جديداً من الحرية والإبداع في هذه اللعبة الشهيرة. يمكنك الاستفادة من ميزات تتيح لك تخصيص تجربتك بشكل أكبر، مثل الوصول إلى عناصر حصرية، وتحسينات في الحركة، والتحكم في البيئة المحيطة، مما يجعل مغامراتك في BROOKHAVEN أكثر إثارة.
        </p>
        <img id="brookhaven-image" class="map-image" src="https://i.pinimg.com/736x/83/87/09/8387094d4d3e5e4d2a1c0d4a7a8d0a0.jpg" alt="[attachment_2](attachment)" onerror="this.onerror=null;this.src='https://placehold.co/300x200/000/FFF?text=Brookhaven+Image';" />
      </div>
      <div class="map-item">
        <h3 id="shalet-mohamed-title">شالية محمد</h3>
        <p>
          لعبة "شالية محمد" هي تجربة فريدة وممتعة، وسكربتاتنا تزيد من متعتها. استكشف الشاليه بأكمله مع ميزات إضافية تساعدك على اكتشاف الأسرار، والتفاعل مع العناصر بطرق جديدة، والاستمتاع باللعبة بشكل لم يسبق له مثيل. هذه السكربتات مصممة لتعزيز تجربتك في هذا العالم الخاص.
        </p>
        <img id="shalet-mohamed-image" class="map-image" src="https://tr.rbxcdn.com/17119254036/420/420/Image/Png" alt="[attachment_3](attachment)" onerror="this.onerror=null;this.src='https://placehold.co/300x200/000/FFF?text=Shalet+Mohamed+Image';" />
      </div>
    </div>

    <div id="footer-text">by HM HUB</div>
  </div>
  <audio id="welcome-audio" src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_3fa1325c84.mp3?filename=welcome-110205.mp3" preload="auto"></audio>

  <div id="copy-message">تم نسخ الرابط!</div>

  <script>
    // Get references to elements
    const loadingScreen = document.getElementById('loading-screen');
    const mainContent = document.getElementById('main-content');
    const title = document.getElementById('title');
    const instructionText = document.getElementById('instruction-text');
    const tvonButton = document.getElementById('tvon-button');
    const descriptionSection = document.getElementById('description-section');
    const supportedMapsSection = document.getElementById('supported-maps-section');
    const welcomeAudio = document.getElementById('welcome-audio');
    const copyMessage = document.getElementById('copy-message');
    const linkToCopy = "loadstring(game:HttpGet(\"https://gitea.com/snoobe/TvonHub/raw/branch/main/TvonLoader.lua\"))()";

    // Get references to map titles and images
    const mm2Title = document.getElementById('mm2-title');
    const mm2Image = document.getElementById('mm2-image');
    const timebombTitle = document.getElementById('timebomb-title');
    const timebombImage = document.getElementById('timebomb-image');
    const brookhavenTitle = document.getElementById('brookhaven-title');
    const brookhavenImage = document.getElementById('brookhaven-image');
    const shaletMohamedTitle = document.getElementById('shalet-mohamed-title');
    const shaletMohamedImage = document.getElementById('shalet-mohamed-image');

    let currentVisibleImage = null; // To keep track of the currently visible image

    // Function to toggle image visibility
    function toggleMapImage(imageElement) {
      if (currentVisibleImage && currentVisibleImage !== imageElement) {
        currentVisibleImage.classList.remove('show-image'); // Hide previous image
      }
      imageElement.classList.toggle('show-image'); // Toggle current image
      currentVisibleImage = imageElement.classList.contains('show-image') ? imageElement : null;
    }

    // After one second, the main content appears and the music plays.
    window.addEventListener('DOMContentLoaded', () => {
      setTimeout(() => {
        loadingScreen.style.display = 'none';
        mainContent.style.display = 'flex';
        welcomeAudio.play().catch(() => {
          console.log("لم يتم تشغيل الصوت تلقائيًا بسبب سياسات المتصفح.");
        });
      }, 1000);
    });

    // When the title text is clicked, it disappears and the button appears.
    title.addEventListener('click', () => {
      title.style.display = 'none';
      instructionText.style.display = 'none';
      tvonButton.style.display = 'inline-block';
    });

    // When the main button is clicked, hide description and show supported maps with animation.
    tvonButton.addEventListener('click', () => {
      descriptionSection.classList.add('hidden');
      setTimeout(() => {
        descriptionSection.style.display = 'none';
        supportedMapsSection.classList.add('show');
      }, 500);

      // Copy the link to clipboard
      const tempInput = document.createElement('textarea');
      tempInput.value = linkToCopy;
      document.body.appendChild(tempInput);
      tempInput.select();
      document.execCommand('copy');
      document.body.removeChild(tempInput);

      // Show copy message
      copyMessage.classList.add('show');
      setTimeout(() => {
        copyMessage.classList.remove('show');
      }, 2000);
    });

    // Add click listeners to map titles
    mm2Title.addEventListener('click', () => toggleMapImage(mm2Image));
    timebombTitle.addEventListener('click', () => toggleMapImage(timebombImage));
    brookhavenTitle.addEventListener('click', () => toggleMapImage(brookhavenImage));
    shaletMohamedTitle.addEventListener('click', () => toggleMapImage(shaletMohamedImage));
  </script>
</body>
</html>

