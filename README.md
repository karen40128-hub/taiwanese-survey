# taiwanese-survey
問卷
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>國小一年級新生台語使用情況調查</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;500;600;700&display=swap');
        body {
            font-family: 'Noto Sans TC', sans-serif;
        }
        .gradient-bg {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }
        .form-container {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
        }
        .radio-option {
            transition: all 0.3s ease;
        }
        .radio-option:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        .slider {
            -webkit-appearance: none;
            appearance: none;
            height: 8px;
            border-radius: 5px;
            background: #e2e8f0;
            outline: none;
        }
        .slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 24px;
            height: 24px;
            border-radius: 50%;
            background: #667eea;
            cursor: pointer;
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
        }
        .slider::-moz-range-thumb {
            width: 24px;
            height: 24px;
            border-radius: 50%;
            background: #667eea;
            cursor: pointer;
            border: none;
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
        }
        .admin-panel {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 1000;
        }
        .response-item {
            border-left: 4px solid #667eea;
            transition: all 0.3s ease;
        }
        .response-item:hover {
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
    </style>
</head>
<body class="gradient-bg min-h-screen py-8">
    <!-- 管理員面板 -->
    <div class="admin-panel">
        <button id="adminToggle" class="bg-white text-purple-600 px-4 py-2 rounded-lg shadow-lg hover:bg-purple-50 transition-all">
            📊 管理面板
        </button>
        <div id="adminPanel" class="hidden mt-2 bg-white rounded-lg shadow-xl p-4 w-80">
            <h3 class="font-bold text-gray-800 mb-3">問卷管理</h3>
            <div class="space-y-2">
                <button id="viewResponses" class="w-full bg-blue-500 text-white py-2 px-4 rounded hover:bg-blue-600 transition-all">
                    📋 查看回覆 (<span id="responseCount">0</span>)
                </button>
                <button id="downloadCSV" class="w-full bg-green-500 text-white py-2 px-4 rounded hover:bg-green-600 transition-all">
                    📥 下載CSV
                </button>
                <button id="downloadJSON" class="w-full bg-purple-500 text-white py-2 px-4 rounded hover:bg-purple-600 transition-all">
                    📥 下載JSON
                </button>
                <button id="clearData" class="w-full bg-red-500 text-white py-2 px-4 rounded hover:bg-red-600 transition-all">
                    🗑️ 清除資料
                </button>
            </div>
        </div>
    </div>

    <div class="container mx-auto px-4 max-w-4xl">
        <!-- 標題區域 -->
        <div class="text-center mb-8">
            <h1 class="text-4xl font-bold text-white mb-4">🏫 塗城國小一年級新生台語使用情況調查</h1>
            <p class="text-xl text-white opacity-90">親愛的家長，請協助我們了解孩子的語言使用情況與學習態度</p>
        </div>

        <!-- 問卷表單 -->
        <div id="surveyForm" class="form-container rounded-2xl shadow-2xl p-8 mb-8">
            <form id="mainForm">
                <!-- 基本資料 -->
                <div class="mb-8">
                    <h2 class="text-2xl font-semibold text-gray-800 mb-6 flex items-center">
                        👨‍👩‍👧‍👦 基本資料
                    </h2>
                    
                    <div class="grid md:grid-cols-2 gap-6">
                        <div>
                            <label class="block text-gray-700 font-medium mb-2">學生姓名</label>
                            <input type="text" id="studentName" name="studentName" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent transition-all" placeholder="請輸入學生姓名" required>
                        </div>
                        <div>
                            <label class="block text-gray-700 font-medium mb-2">班級</label>
                            <select id="studentClass" name="studentClass" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent transition-all" required>
                                <option value="">請選擇班級</option>
                                <option value="101">101</option>
                                <option value="102">102</option>
                                <option value="103">103</option>
                                <option value="104">104</option>
                                <option value="105">105</option>
                                <option value="106">106</option>
                                <option value="107">107</option>
                                <option value="108">108</option>
                                <option value="109">109</option>
                            </select>
                        </div>
                    </div>

                    <!-- 與孩子同住成員 -->
                    <div class="mb-6 mt-6">
                        <label class="block text-gray-700 font-medium mb-4">與孩子同住的成員？（可複選）</label>
                        <div class="grid md:grid-cols-3 gap-4">
                            <label class="flex items-center bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="checkbox" name="livingMembers" value="爸爸" class="w-5 h-5 text-purple-500 border-gray-300 rounded focus:ring-purple-500">
                                <span class="ml-3 text-gray-700 font-medium">爸爸</span>
                            </label>
                            <label class="flex items-center bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="checkbox" name="livingMembers" value="媽媽" class="w-5 h-5 text-purple-500 border-gray-300 rounded focus:ring-purple-500">
                                <span class="ml-3 text-gray-700 font-medium">媽媽</span>
                            </label>
                            <label class="flex items-center bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="checkbox" name="livingMembers" value="阿公" class="w-5 h-5 text-purple-500 border-gray-300 rounded focus:ring-purple-500">
                                <span class="ml-3 text-gray-700 font-medium">阿公</span>
                            </label>
                            <label class="flex items-center bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="checkbox" name="livingMembers" value="阿嬤" class="w-5 h-5 text-purple-500 border-gray-300 rounded focus:ring-purple-500">
                                <span class="ml-3 text-gray-700 font-medium">阿嬤</span>
                            </label>
                            <label class="flex items-center bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="checkbox" name="livingMembers" value="兄弟姊妹" class="w-5 h-5 text-purple-500 border-gray-300 rounded focus:ring-purple-500">
                                <span class="ml-3 text-gray-700 font-medium">兄弟姊妹</span>
                            </label>
                            <label class="flex items-center bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="checkbox" name="livingMembers" value="其他親戚" class="w-5 h-5 text-purple-500 border-gray-300 rounded focus:ring-purple-500">
                                <span class="ml-3 text-gray-700 font-medium">其他親戚</span>
                            </label>
                        </div>
                    </div>
                </div>

                <!-- 語言使用情況 -->
                <div class="mb-8">
                    <h2 class="text-2xl font-semibold text-gray-800 mb-6 flex items-center">
                        🗣️ 語言使用情況
                    </h2>
                    
                    <!-- 孩子平常在家使用的語言 -->
                    <div class="mb-6">
                        <label class="block text-gray-700 font-medium mb-4">1. 孩子平常在家裡使用的語言？（可複選）</label>
                        <div class="grid md:grid-cols-2 gap-4">
                            <label class="flex items-center bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="checkbox" name="homeLanguages" value="國語" class="w-5 h-5 text-purple-500 border-gray-300 rounded focus:ring-purple-500">
                                <span class="ml-3 text-gray-700 font-medium">國語</span>
                            </label>
                            <label class="flex items-center bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="checkbox" name="homeLanguages" value="台語" class="w-5 h-5 text-purple-500 border-gray-300 rounded focus:ring-purple-500">
                                <span class="ml-3 text-gray-700 font-medium">台語</span>
                            </label>
                            <label class="flex items-center bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="checkbox" name="homeLanguages" value="客語" class="w-5 h-5 text-purple-500 border-gray-300 rounded focus:ring-purple-500">
                                <span class="ml-3 text-gray-700 font-medium">客語</span>
                            </label>
                            <div class="flex items-center bg-white border-2 border-gray-200 rounded-lg p-4">
                                <input type="checkbox" id="otherLanguage" name="homeLanguages" value="其他" class="w-5 h-5 text-purple-500 border-gray-300 rounded focus:ring-purple-500">
                                <label for="otherLanguage" class="ml-3 text-gray-700 font-medium">其他：</label>
                                <input type="text" id="otherLanguageText" name="otherLanguageText" class="ml-2 flex-1 px-2 py-1 border border-gray-300 rounded focus:ring-1 focus:ring-purple-500" placeholder="請填寫">
                            </div>
                        </div>
                    </div>

                    <!-- 孩子聽得懂台語的程度 -->
                    <div class="mb-6 mt-8">
                        <label class="block text-gray-700 font-medium mb-4">2. 孩子聽得懂台語的程度？</label>
                        <div class="grid md:grid-cols-3 gap-4">
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="listen_well" name="listeningLevel" value="幾乎都聽得懂" class="sr-only">
                                <label for="listen_well" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">幾乎都聽得懂</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="listen_half" name="listeningLevel" value="聽得懂一半" class="sr-only">
                                <label for="listen_half" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">聽得懂一半</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="listen_little" name="listeningLevel" value="聽不太懂" class="sr-only">
                                <label for="listen_little" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">聽不太懂</span>
                                </label>
                            </div>
                        </div>
                    </div>

                    <!-- 孩子說台語的程度 -->
                    <div class="mb-6 mt-8">
                        <label class="block text-gray-700 font-medium mb-4">3. 孩子說台語的程度？</label>
                        <div class="grid md:grid-cols-3 gap-4">
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="speak_fluent" name="speakingLevel" value="很會講，能完整表達" class="sr-only">
                                <label for="speak_fluent" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">很會講，能完整表達</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="speak_simple" name="speakingLevel" value="會講一些簡單的詞句" class="sr-only">
                                <label for="speak_simple" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">會講一些簡單的詞句</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="speak_barely" name="speakingLevel" value="幾乎不會講" class="sr-only">
                                <label for="speak_barely" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">幾乎不會講</span>
                                </label>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- 孩子的學習態度 -->
                <div class="mb-8">
                    <h2 class="text-2xl font-semibold text-gray-800 mb-6 flex items-center">
                        📚 孩子的學習態度
                    </h2>
                    
                    <!-- 對語文學習的興趣 -->
                    <div class="mb-6">
                        <label class="block text-gray-700 font-medium mb-4">4. 孩子對台語學習是否有興趣？</label>
                        <div class="grid md:grid-cols-3 gap-4">
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="interest_high" name="languageInterest" value="很有興趣，常主動練習或表達" class="sr-only">
                                <label for="interest_high" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">很有興趣，常主動練習或表達</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="interest_normal" name="languageInterest" value="普通，需要提醒才會練習" class="sr-only">
                                <label for="interest_normal" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">普通，需要提醒才會練習</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="interest_low" name="languageInterest" value="比較沒有興趣" class="sr-only">
                                <label for="interest_low" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">比較沒有興趣</span>
                                </label>
                            </div>
                        </div>
                    </div>

                    <!-- 學習時的態度 -->
                    <div class="mb-6 mt-8">
                        <label class="block text-gray-700 font-medium mb-4">5. 孩子在學習時的態度？</label>
                        <div class="grid md:grid-cols-3 gap-4">
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="attitude_focused" name="learningAttitude" value="很認真，能專心完成老師交代的任務" class="sr-only">
                                <label for="attitude_focused" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">很認真，能專心完成老師交代的任務</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="attitude_normal" name="learningAttitude" value="普通，有時專心，有時容易分心" class="sr-only">
                                <label for="attitude_normal" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">普通，有時專心，有時容易分心</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="attitude_distracted" name="learningAttitude" value="不太專心，需要大人督促" class="sr-only">
                                <label for="attitude_distracted" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">不太專心，需要大人督促</span>
                                </label>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- 孩子的語言自信心 -->
                <div class="mb-8">
                    <h2 class="text-2xl font-semibold text-gray-800 mb-6 flex items-center">
                        🔹 孩子的語言自信心
                    </h2>
                    
                    <!-- 公開場合說話態度 -->
                    <div class="mb-6">
                        <label class="block text-gray-700 font-medium mb-4">6. 孩子在公開場合（如課堂發言、上台）說話的態度？</label>
                        <div class="grid md:grid-cols-3 gap-4">
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="confidence_high" name="speakingConfidence" value="大方、有自信，敢表達" class="sr-only">
                                <label for="confidence_high" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">大方、有自信，敢表達</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="confidence_medium" name="speakingConfidence" value="有點害羞，但願意嘗試" class="sr-only">
                                <label for="confidence_medium" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">有點害羞，但願意嘗試</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="confidence_low" name="speakingConfidence" value="害羞、不太願意" class="sr-only">
                                <label for="confidence_low" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">害羞、不太願意</span>
                                </label>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- 比賽動機與家長支持度 -->
                <div class="mb-8">
                    <h2 class="text-2xl font-semibold text-gray-800 mb-6 flex items-center">
                        🔹 比賽動機與家長支持度
                    </h2>
                    
                    <!-- 孩子參加台語比賽的態度 -->
                    <div class="mb-6">
                        <label class="block text-gray-700 font-medium mb-4">7. 如果孩子參加台語比賽，他／她的態度可能是？</label>
                        <div class="grid md:grid-cols-3 gap-4">
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="child_eager" name="childCompetitionAttitude" value="很有興趣，會主動練習" class="sr-only">
                                <label for="child_eager" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">很有興趣，會主動練習</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="child_willing" name="childCompetitionAttitude" value="願意嘗試，但需要提醒" class="sr-only">
                                <label for="child_willing" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">願意嘗試，但需要提醒</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="child_reluctant" name="childCompetitionAttitude" value="比較沒有意願" class="sr-only">
                                <label for="child_reluctant" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">比較沒有意願</span>
                                </label>
                            </div>
                        </div>
                    </div>

                    <!-- 家長協助練習意願 -->
                    <div class="mb-6 mt-8">
                        <label class="block text-gray-700 font-medium mb-4">8. 家長能否在比賽前協助孩子練習？</label>
                        <div class="grid md:grid-cols-3 gap-4">
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="parent_supportive" name="parentSupport" value="會主動陪伴與支持" class="sr-only">
                                <label for="parent_supportive" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">會主動陪伴與支持</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="parent_conditional" name="parentSupport" value="視時間情況而定" class="sr-only">
                                <label for="parent_conditional" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">視時間情況而定</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="parent_difficult" name="parentSupport" value="可能較難配合" class="sr-only">
                                <label for="parent_difficult" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">可能較難配合</span>
                                </label>
                            </div>
                        </div>
                    </div>

                    <!-- 台語比賽興趣 -->
                    <div class="mb-6 mt-8">
                        <label class="block text-gray-700 font-medium mb-4">9. 對於讓孩子參加台語比賽的興趣程度？</label>
                        <div class="bg-white rounded-lg p-6 border-2 border-gray-200">
                            <div class="flex justify-between text-sm text-gray-600 mb-2">
                                <span>完全沒興趣</span>
                                <span>非常有興趣</span>
                            </div>
                            <input type="range" id="competitionInterest" name="competitionInterest" min="1" max="10" value="5" class="slider w-full">
                            <div class="text-center mt-3">
                                <span class="text-lg font-semibold text-purple-600">興趣指數：<span id="competitionValue">5</span>/10</span>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- 其他意見 -->
                <div class="mb-8">
                    <h2 class="text-2xl font-semibold text-gray-800 mb-6 flex items-center">
                        💭 其他意見
                    </h2>
                    <div>
                        <label class="block text-gray-700 font-medium mb-2">10. 對於學校推廣台語教育的建議或意見</label>
                        <textarea id="suggestions" name="suggestions" rows="4" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent transition-all" placeholder="請分享您的想法和建議..."></textarea>
                    </div>
                </div>

                <!-- 家長身份 -->
                <div class="mb-8">
                    <h2 class="text-2xl font-semibold text-gray-800 mb-6 flex items-center">
                        👤 家長身份
                    </h2>
                    <div>
                        <label class="block text-gray-700 font-medium mb-4">您是孩子的哪位家長？</label>
                        <div class="grid md:grid-cols-3 gap-4">
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="parent_father" name="parentRelation" value="爸爸" class="sr-only">
                                <label for="parent_father" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">爸爸</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="parent_mother" name="parentRelation" value="媽媽" class="sr-only">
                                <label for="parent_mother" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">媽媽</span>
                                </label>
                            </div>
                            <div class="radio-option bg-white border-2 border-gray-200 rounded-lg p-4 cursor-pointer hover:border-purple-400">
                                <input type="radio" id="parent_other" name="parentRelation" value="其他家長" class="sr-only">
                                <label for="parent_other" class="flex items-center cursor-pointer">
                                    <div class="w-5 h-5 border-2 border-gray-300 rounded-full mr-3 flex items-center justify-center">
                                        <div class="w-3 h-3 bg-purple-500 rounded-full hidden"></div>
                                    </div>
                                    <span class="text-gray-700 font-medium">其他家長</span>
                                </label>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- 感謝說明 -->
                <div class="bg-gradient-to-r from-blue-50 to-purple-50 rounded-lg p-6 mb-6 border border-blue-200">
                    <div class="text-center">
                        <div class="text-3xl mb-3">🙏</div>
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">感謝您配合填寫問卷</h3>
                        <p class="text-gray-600 text-sm leading-relaxed">
                            您所提供的資訊將有助於我們更了解孩子的台語學習狀況，<br>
                            讓我們能夠提供更適合的台語教育內容與教學方式。<br>
                            您的參與對孩子的學習發展非常重要！
                        </p>
                    </div>
                </div>

                <!-- 提交按鈕 -->
                <div class="text-center">
                    <button type="submit" class="bg-gradient-to-r from-purple-600 to-blue-600 text-white font-bold py-4 px-12 rounded-full text-lg hover:from-purple-700 hover:to-blue-700 transform hover:scale-105 transition-all duration-300 shadow-lg">
                        📝 提交問卷
                    </button>
                </div>
            </form>
        </div>

        <!-- 感謝訊息（隱藏） -->
        <div id="thankYouMessage" class="hidden form-container rounded-2xl shadow-2xl p-8 text-center">
            <div class="text-6xl mb-4">🎉</div>
            <h2 class="text-3xl font-bold text-gray-800 mb-4">感謝您的參與！</h2>
            <p class="text-lg text-gray-600 mb-6">您的問卷已成功提交，我們會妥善運用這些資料來改善台語教育。</p>
            <div class="bg-purple-100 rounded-lg p-4 mb-6">
                <p class="text-purple-800 font-medium">如有任何問題，請聯絡學校教務處</p>
                <p class="text-purple-600">電話：(02) 1234-5678</p>
            </div>
            <button id="backToForm" class="bg-purple-600 text-white px-6 py-2 rounded-lg hover:bg-purple-700 transition-all">
                返回問卷
            </button>
        </div>

        <!-- 回覆查看頁面（隱藏） -->
        <div id="responsesView" class="hidden form-container rounded-2xl shadow-2xl p-8">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-2xl font-bold text-gray-800">📊 問卷回覆</h2>
                <button id="backToFormFromResponses" class="bg-purple-600 text-white px-4 py-2 rounded-lg hover:bg-purple-700 transition-all">
                    返回問卷
                </button>
            </div>
            <div id="responsesList" class="space-y-4">
                <!-- 回覆內容將在這裡顯示 -->
            </div>
        </div>
    </div>

    <script>
        // 全域變數
        let surveyResponses = JSON.parse(localStorage.getItem('surveyResponses') || '[]');

        // 更新回覆數量顯示
        function updateResponseCount() {
            document.getElementById('responseCount').textContent = surveyResponses.length;
        }

        // 初始化
        updateResponseCount();

        // 自訂單選按鈕樣式
        document.querySelectorAll('input[type="radio"]').forEach(radio => {
            radio.addEventListener('change', function() {
                // 清除同組其他選項的樣式
                document.querySelectorAll(`input[name="${this.name}"]`).forEach(r => {
                    const container = r.closest('.radio-option');
                    const dot = r.parentElement.querySelector('.w-3');
                    if (container) container.classList.remove('border-purple-500', 'bg-purple-50');
                    if (dot) dot.classList.add('hidden');
                });
                
                // 設定選中項目的樣式
                const container = this.closest('.radio-option');
                const dot = this.parentElement.querySelector('.w-3');
                if (container) container.classList.add('border-purple-500', 'bg-purple-50');
                if (dot) dot.classList.remove('hidden');
            });
        });

        // 滑桿數值顯示
        document.getElementById('competitionInterest').addEventListener('input', function() {
            document.getElementById('competitionValue').textContent = this.value;
        });

        // 管理員面板切換
        document.getElementById('adminToggle').addEventListener('click', function() {
            const panel = document.getElementById('adminPanel');
            panel.classList.toggle('hidden');
        });

        // 表單提交處理
        document.getElementById('mainForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            // 收集表單資料
            const formData = new FormData(this);
            const data = {
                submitTime: new Date().toLocaleString('zh-TW'),
                studentName: formData.get('studentName'),
                studentClass: formData.get('studentClass'),
                livingMembers: [],
                homeLanguages: [],
                otherLanguageText: formData.get('otherLanguageText'),
                listeningLevel: formData.get('listeningLevel'),
                speakingLevel: formData.get('speakingLevel'),
                languageInterest: formData.get('languageInterest'),
                learningAttitude: formData.get('learningAttitude'),
                speakingConfidence: formData.get('speakingConfidence'),
                childCompetitionAttitude: formData.get('childCompetitionAttitude'),
                parentSupport: formData.get('parentSupport'),
                competitionInterest: formData.get('competitionInterest'),
                suggestions: formData.get('suggestions'),
                parentRelation: formData.get('parentRelation')
            };
            
            // 處理複選框
            document.querySelectorAll('input[name="livingMembers"]:checked').forEach(cb => {
                data.livingMembers.push(cb.value);
            });

            document.querySelectorAll('input[name="homeLanguages"]:checked').forEach(cb => {
                data.homeLanguages.push(cb.value);
            });
            
            // 儲存到 localStorage
            surveyResponses.push(data);
            localStorage.setItem('surveyResponses', JSON.stringify(surveyResponses));
            
            // 更新回覆數量
            updateResponseCount();
            
            // 顯示感謝訊息
            document.getElementById('surveyForm').style.display = 'none';
            document.getElementById('thankYouMessage').classList.remove('hidden');
            
            // 滾動到頂部
            window.scrollTo({ top: 0, behavior: 'smooth' });
        });

        // 返回問卷按鈕
        document.getElementById('backToForm').addEventListener('click', function() {
            document.getElementById('thankYouMessage').classList.add('hidden');
            document.getElementById('surveyForm').style.display = 'block';
            document.getElementById('mainForm').reset();
            
            // 重置所有單選按鈕樣式
            document.querySelectorAll('.radio-option').forEach(option => {
                option.classList.remove('border-purple-500', 'bg-purple-50');
            });
            document.querySelectorAll('.w-3').forEach(dot => {
                dot.classList.add('hidden');
            });
            
            // 重置滑桿顯示
            document.getElementById('competitionValue').textContent = '5';
        });

        // 查看回覆
        document.getElementById('viewResponses').addEventListener('click', function() {
            displayResponses();
            document.getElementById('surveyForm').style.display = 'none';
            document.getElementById('thankYouMessage').classList.add('hidden');
            document.getElementById('responsesView').classList.remove('hidden');
        });

        // 從回覆頁面返回
        document.getElementById('backToFormFromResponses').addEventListener('click', function() {
            document.getElementById('responsesView').classList.add('hidden');
            document.getElementById('surveyForm').style.display = 'block';
        });

        // 顯示回覆內容
        function displayResponses() {
            const responsesList = document.getElementById('responsesList');
            
            if (surveyResponses.length === 0) {
                responsesList.innerHTML = '<div class="text-center text-gray-500 py-8">目前還沒有任何回覆</div>';
                return;
            }
            
            responsesList.innerHTML = surveyResponses.map((response, index) => `
                <div class="response-item bg-white rounded-lg p-6 shadow-md">
                    <div class="flex justify-between items-start mb-4">
                        <h3 class="text-lg font-semibold text-gray-800">回覆 #${index + 1}</h3>
                        <span class="text-sm text-gray-500">${response.submitTime}</span>
                    </div>
                    <div class="grid md:grid-cols-2 gap-4 text-sm">
                        <div><strong>學生姓名：</strong>${response.studentName}</div>
                        <div><strong>班級：</strong>${response.studentClass}</div>
                        <div><strong>同住成員：</strong>${response.livingMembers.join(', ')}</div>
                        <div><strong>家中語言：</strong>${response.homeLanguages.join(', ')}</div>
                        <div><strong>聽台語程度：</strong>${response.listeningLevel}</div>
                        <div><strong>說台語程度：</strong>${response.speakingLevel}</div>
                        <div><strong>學習興趣：</strong>${response.languageInterest}</div>
                        <div><strong>學習態度：</strong>${response.learningAttitude}</div>
                        <div><strong>說話自信：</strong>${response.speakingConfidence}</div>
                        <div><strong>比賽態度：</strong>${response.childCompetitionAttitude}</div>
                        <div><strong>家長支持：</strong>${response.parentSupport}</div>
                        <div><strong>比賽興趣：</strong>${response.competitionInterest}/10</div>
                        <div><strong>家長身份：</strong>${response.parentRelation}</div>
                    </div>
                    ${response.suggestions ? `<div class="mt-4"><strong>建議意見：</strong><br><div class="bg-gray-50 p-3 rounded mt-2">${response.suggestions}</div></div>` : ''}
                </div>
            `).join('');
        }

        // 下載 CSV
        document.getElementById('downloadCSV').addEventListener('click', function() {
            if (surveyResponses.length === 0) {
                alert('目前沒有資料可以下載');
                return;
            }
            
            const headers = [
                '提交時間', '學生姓名', '班級', '同住成員', '家中語言', '其他語言',
                '聽台語程度', '說台語程度', '學習興趣', '學習態度', '說話自信',
                '比賽態度', '家長支持', '比賽興趣', '建議意見', '家長身份'
            ];
            
            const csvContent = [
                headers.join(','),
                ...surveyResponses.map(response => [
                    response.submitTime,
                    response.studentName,
                    response.studentClass,
                    `"${response.livingMembers.join('; ')}"`,
                    `"${response.homeLanguages.join('; ')}"`,
                    response.otherLanguageText || '',
                    response.listeningLevel,
                    response.speakingLevel,
                    response.languageInterest,
                    response.learningAttitude,
                    response.speakingConfidence,
                    response.childCompetitionAttitude,
                    response.parentSupport,
                    response.competitionInterest,
                    `"${response.suggestions || ''}"`,
                    response.parentRelation
                ].join(','))
            ].join('\n');
            
            // 加入 BOM 以支援中文
            const BOM = '\uFEFF';
            const blob = new Blob([BOM + csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = `台語問卷調查_${new Date().toISOString().split('T')[0]}.csv`;
            link.click();
        });

        // 下載 JSON
        document.getElementById('downloadJSON').addEventListener('click', function() {
            if (surveyResponses.length === 0) {
                alert('目前沒有資料可以下載');
                return;
            }
            
            const blob = new Blob([JSON.stringify(surveyResponses, null, 2)], { type: 'application/json' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = `台語問卷調查_${new Date().toISOString().split('T')[0]}.json`;
            link.click();
        });

        // 清除資料
        document.getElementById('clearData').addEventListener('click', function() {
            if (confirm('確定要清除所有問卷資料嗎？此操作無法復原！')) {
                surveyResponses = [];
                localStorage.removeItem('surveyResponses');
                updateResponseCount();
                alert('資料已清除');
            }
        });

        // 表單驗證
        document.querySelectorAll('input[required], select[required]').forEach(field => {
            field.addEventListener('blur', function() {
                if (!this.value.trim()) {
                    this.classList.add('border-red-500');
                } else {
                    this.classList.remove('border-red-500');
                }
            });
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'977ccb9316384a5c',t:'MTc1NjY0NjExMS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
