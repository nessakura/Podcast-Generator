
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Podcast Content Generator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #ff6b6b, #4ecdc4);
            color: white;
            padding: 30px;
            text-align: center;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
        }

        .content {
            padding: 30px;
        }

        .input-section {
            margin-bottom: 30px;
        }

        .input-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #333;
            font-size: 1.1em;
        }

        input[type="text"], textarea {
            width: 100%;
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 16px;
            transition: all 0.3s ease;
            font-family: inherit;
        }

        input[type="text"]:focus, textarea:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        textarea {
            height: 120px;
            resize: vertical;
        }

        .generate-btn {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 1.1em;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 1px;
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
        }

        .generate-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(102, 126, 234, 0.4);
        }

        .generate-btn:active {
            transform: translateY(0);
        }

        .generate-btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none;
        }

        .results-section {
            margin-top: 30px;
            padding-top: 30px;
            border-top: 2px solid #f0f0f0;
        }

        /* Adjusted styles for combined script display */
        .full-script-display {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 20px;
            border-left: 5px solid #667eea;
        }

        .full-script-display h3 {
            color: #667eea;
            margin-bottom: 15px;
            font-size: 1.3em;
        }

        .full-script-content {
            background: white;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 15px;
            border: 1px solid #e0e0e0;
            white-space: pre-wrap; /* Preserve line breaks */
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.8;
            font-size: 16px;
            max-height: 600px; /* Allow more height for combined script */
            overflow-y: auto;
        }

        /* Original speaker-result sections can be removed or hidden if only combined view is desired */
        .speaker-result {
            display: none; /* Hide individual speaker sections */
        }


        .audio-controls {
            display: flex;
            gap: 10px;
            align-items: center;
            flex-wrap: wrap;
        }

        .play-btn {
            background: linear-gradient(135deg, #4ecdc4, #44a08d);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 25px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        .play-btn:hover {
            transform: translateY(-1px);
            box-shadow: 0 5px 15px rgba(78, 205, 196, 0.3);
        }

        .play-btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
        }

        .loading {
            display: none;
            text-align: center;
            padding: 20px;
            color: #667eea;
        }

        .loading.show {
            display: block;
        }

        .spinner {
            width: 40px;
            height: 40px;
            border: 4px solid #f3f3f3;
            border-top: 4px solid #667eea;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto 10px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .error {
            background: #ffebee;
            color: #c62828;
            padding: 15px;
            border-radius: 10px;
            margin: 10px 0;
            border-left: 4px solid #c62828;
        }

        .api-key-section {
            background: #fff3cd;
            border: 1px solid #ffeaa7;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 20px;
        }

        .api-key-section h3 {
            color: #856404;
            margin-bottom: 10px;
        }

        .api-key-section p {
            color: #856404;
            margin-bottom: 15px;
            font-size: 0.9em;
        }

        @media (max-width: 768px) {
            .container {
                margin: 10px;
                border-radius: 15px;
            }
            
            .content {
                padding: 20px;
            }
            
            .header h1 {
                font-size: 2em;
            }
            
            .audio-controls {
                flex-direction: column;
                align-items: stretch;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎙️ Podcast Generator</h1>
            <p>สร้าง Podcast ด้วย Gemini 2.5 Flash TTS</p>
        </div>

        <div class="content">
            <div class="api-key-section">
                <h3>🔑 API Key Configuration</h3>
                <p>กรุณาใส่ Google AI Studio API Key (Gemini) ของคุณ สำหรับสร้างบทสนทนา (ไม่มีการเก็บข้อมูล)</p>
                <div class="input-group">
                    <input type="password" id="apiKey" placeholder="ใส่ Gemini API Key ของคุณ" />
                </div>
            </div>

            <div class="input-section">
                <div class="input-group">
                    <label for="topic">🎯 หัวข้อ Podcast:</label>
                    <input type="text" id="topic" placeholder="เช่น: เทคโนโลجี AI ในอนาคต, การเดินทางในอวกาศ, ฯลฯ" />
                </div>

                <div class="input-group">
                    <label for="details">📝 รายละเอียดเพิ่มเติม:</label>
                    <textarea id="details" placeholder="ใส่รายละเอียดเพิ่มเติมเกี่ยวกับหัวข้อที่ต้องการให้สนทนา เช่น มุมมองที่น่าสนใจ, ประเด็นที่ควรพูดถึง, ข้อมูลเฉพาะ ฯลฯ"></textarea>
                </div>

                <button class="generate-btn" onclick="generatePodcast()">
                    📝 สร้างบทสนทนา Podcast
                </button>
            </div>

            <div class="loading" id="loading">
                <div class="spinner"></div>
                <p>กำลังสร้างบทสนทนา Podcast...</p>
            </div>

            <div class="results-section" id="results" style="display: none;">
                <div class="full-script-display">
                    <h3>บทสนทนา Podcast (จัดเรียง)</h3>
                    <div class="full-script-content" id="fullScriptDisplay"></div>
                </div>

                <div class="speaker-result" style="display: none;"> 
                    <h3>👨 Speaker 1 (ชาย)</h3>
                    <div class="script-content" id="speaker1Script"></div>
                </div>

                <div class="speaker-result" style="display: none;">
                    <h3>👩 Speaker 2 (หญิง)</h3>
                    <div class="script-content" id="speaker2Script"></div>
                </div>
            </div>
        </div>
    </div>

    <script>
        let currentScript = { speaker1: '', speaker2: '', full: '' };
        let isGenerating = false;

        async function generatePodcast() {
            const apiKey = document.getElementById('apiKey').value.trim();
            const topic = document.getElementById('topic').value.trim();
            const details = document.getElementById('details').value.trim();

            if (!apiKey) {
                showError('กรุณาใส่ API Key');
                return;
            }

            if (!topic) {
                showError('กรุณาใส่หัวข้อ Podcast');
                return;
            }

            if (isGenerating) return;
            isGenerating = true;

            showLoading(true);
            hideError();

            try {
                const script = await generateScript(apiKey, topic, details);
                
                const parsedSpeakers = parseAndInterleaveScript(script);
                currentScript.speaker1 = parsedSpeakers.speaker1;
                currentScript.speaker2 = parsedSpeakers.speaker2;
                currentScript.full = parsedSpeakers.full;

                displayScript(currentScript);

                document.getElementById('results').style.display = 'block';
                
            } catch (error) {
                console.error('Error:', error);
                showError('เกิดข้อผิดพลาด: ' + error.message);
            } finally {
                showLoading(false);
                isGenerating = false;
            }
        }

        async function generateScript(apiKey, topic, details) {
            const prompt = `เริ่มต้นบทสนทนาด้วยการแนะนำช่อง "AI News Flow" ซึ่งเป็นช่องให้ความรู้เกี่ยวกับด้านเทคโนโลยี AI และข่าวสารต่างๆ จากนั้นจึงเริ่มบทสนทนา Podcast ระหว่าง 2 คน เรื่อง "${topic}" ความยาว 5-6 นาที

รายละเอียดเพิ่มเติม: ${details}

กฎในการสร้างบทสนทนา:
1. Speaker 1 เป็นผู้ชาย, Speaker 2 เป็นผู้หญิง
2. ให้สนทนากันแบบเป็นธรรมชาติ น่าสนใจ และมีข้อมูลที่เป็นประโยชน์
3. ไม่ต้องพูดชื่อกันระหว่างสนทนา
4. ใช้รูปแบบ:
   Speaker 1: [ข้อความ]
   Speaker 2: [ข้อความ]
   (ย้ำ: ต้องสลับกันไปมาเสมอ เช่น Speaker 1: ... Speaker 2: ... Speaker 1: ... ห้ามมี Speaker คนเดิมพูดสองรอบติดกัน)
5. ความยาวประมาณ 5-6 นาที เมื่ออ่าน (ประมาณ 800-1000 คำ รวมทั้งสองคน)
6. ให้มีการแลกเปลี่ยนความคิดเห็น ถามตอบ และสรุปในตอนท้าย
7. ใช้ภาษาไทยที่เข้าใจง่าย เหมาะสำหรับผู้ฟัง Podcast
8. ให้ Speaker 1 (ชาย) มีน้ำเสียงที่มั่นใจ วิเคราะห์ อธิบายรายละเอียด
9. ให้ Speaker 2 (หญิง) มีน้ำเสียงที่อ่อนโยน ใจเย็น ชอบถามคำถาม และสรุปประเด็น
10. สร้างบทสนทนาให้ยาวพอที่จะฟังได้ 5-6 นาที โดยแบ่งเป็นหลายรอบการสนทนา
11. เริ่มด้วยการแนะนำหัวข้อ กลางเป็นการอภิปรายลึก และจบด้วยการสรุป

Note: ห้ามเขียน **Speaker 1:** กับ **Speaker 2:** ไม่ถูกต้อง ต้องเปลี่ยนเป็นแบบนี้ Speaker 1: กับ Speaker 2: เท่านั้น
เริ่มสร้างบทสนทนา:`;

            const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash-exp:generateContent?key=${apiKey}`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({
                    contents: [{
                        parts: [{
                            text: prompt
                        }]
                    }]
                })
            });

            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }

            const data = await response.json();
            
            if (!data.candidates || !data.candidates[0] || !data.candidates[0].content || !data.candidates[0].content.parts || !data.candidates[0].content.parts[0]) {
                throw new Error('ไม่สามารถสร้างเนื้อหาได้ อาจเกิดจาก API Key ไม่ถูกต้อง หรือโมเดลไม่ตอบสนอง');
            }

            return data.candidates[0].content.parts[0].text;
        }

        function parseAndInterleaveScript(script) {
            const lines = script.split('\n').map(line => line.trim()).filter(line => line.length > 0);
            let speaker1Content = '';
            let speaker2Content = '';
            let fullScriptContent = '';

            for (const line of lines) {
                if (line.startsWith('Speaker 1:')) {
                    const text = line.replace('Speaker 1:', '').trim();
                    speaker1Content += text + '\n';
                    // เปลี่ยนจาก '👨 Speaker 1:' เป็น 'Speaker 1:'
                    fullScriptContent += '<span style="color: #667eea; font-weight: bold;">Speaker 1:</span> ' + text + '\n\n';
                } else if (line.startsWith('Speaker 2:')) {
                    const text = line.replace('Speaker 2:', '').trim();
                    speaker2Content += text + '\n';
                    // เปลี่ยนจาก '👩 Speaker 2:' เป็น 'Speaker 2:'
                    fullScriptContent += '<span style="color: #4ecdc4; font-weight: bold;">Speaker 2:</span> ' + text + '\n\n';
                } else {
                    fullScriptContent += line + '\n\n';
                }
            }
            return { speaker1: speaker1Content, speaker2: speaker2Content, full: fullScriptContent };
        }

        function displayScript(speakers) {
            document.getElementById('speaker1Script').textContent = speakers.speaker1.trim();
            document.getElementById('speaker2Script').textContent = speakers.speaker2.trim();
            
            document.getElementById('fullScriptDisplay').innerHTML = speakers.full.trim();
        }

        function showLoading(show) {
            const loading = document.getElementById('loading');
            if (show) {
                loading.classList.add('show');
            } else {
                loading.classList.remove('show');
            }
        }

        function showError(message) {
            hideError();
            const errorDiv = document.createElement('div');
            errorDiv.className = 'error';
            errorDiv.textContent = message;
            document.querySelector('.input-section').appendChild(errorDiv);
        }

        function hideError() {
            const error = document.querySelector('.error');
            if (error) {
                error.remove();
            }
        }

        document.getElementById('topic').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                generatePodcast();
            }
        });

        document.getElementById('details').addEventListener('keypress', function(e) {
            if (e.key === 'Enter' && e.ctrlKey) {
                generatePodcast();
            }
        });
    </script>
</body>
</html>
