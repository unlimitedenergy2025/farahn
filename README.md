<html lang="ar">
<head>
    <meta charset="utf-8"/>
    <meta content="width=device-width, initial-scale=1.0" name="viewport"/>
    <title>فرحان - Chat GPT</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css" rel="stylesheet"/>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap" rel="stylesheet"/>
    <style>
        body {
            font-family: 'Cairo', sans-serif;
        }
    </style>
</head>
<body class="bg-gray-100 flex flex-col h-screen">
    <header class="bg-blue-600 text-white p-4 flex justify-between items-center">
        <h1 class="text-2xl font-bold">فرحان - Chat GPT</h1>
        <nav>
            <ul class="flex space-x-4">
                <li><a class="hover:underline" href="#">الرئيسية</a></li>
                <li><a class="hover:underline" href="#">حول</a></li>
                <li><a class="hover:underline" href="#">تواصل معنا</a></li>
            </ul>
        </nav>
    </header>
    <main class="flex-grow container mx-auto p-4">
        <div class="bg-white rounded-lg shadow-lg p-6 flex flex-col h-full">
            <div id="chat-container" class="flex flex-col space-y-4 overflow-y-auto flex-grow">
                <!-- Chat messages will be appended here -->
            </div>
            <div class="mt-4 flex">
                <input class="w-full p-2 border rounded-lg" id="user-input" placeholder="اكتب رسالتك هنا..." type="text"/>
                <button class="bg-blue-600 text-white p-2 rounded-lg ml-2" id="send-button">إرسال</button>
            </div>
        </div>
    </main>
    <footer class="bg-blue-600 text-white p-4 text-center">
        <p>© 2023 فرحان - Chat GPT. جميع الحقوق محفوظة.</p>
    </footer>
    <script>
        async function getChatGPTResponse(question) {
            const apiKey = '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';
            const response = await fetch('https://api.openai.com/v1/completions', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${apiKey}`
                },
                body: JSON.stringify({
                    model: "text-davinci-003",
                    prompt: question,
                    max_tokens: 150
                })
            });
            const data = await response.json();
            return data.choices[0].text.trim();
        }

        document.getElementById('send-button').addEventListener('click', async function() {
            const userInput = document.getElementById('user-input').value;
            if (userInput.trim() === '') return;

            const chatContainer = document.getElementById('chat-container');

            // Append user message
            const userMessage = document.createElement('div');
            userMessage.className = 'flex items-start space-x-4 self-end';
            userMessage.innerHTML = `
                <div class="bg-blue-600 text-white p-4 rounded-lg">
                    <p>${userInput}</p>
                </div>
                <img src="https://placehold.co/50x50" alt="صورة رمزية للمستخدم" class="w-12 h-12 rounded-full" />
            `;
            chatContainer.appendChild(userMessage);

            // Clear input
            document.getElementById('user-input').value = '';

            // Get AI response
            const aiResponseText = await getChatGPTResponse(userInput);

            // Append AI response
            const aiResponse = document.createElement('div');
            aiResponse.className = 'flex items-start space-x-4';
            aiResponse.innerHTML = `
                <img src="https://placehold.co/50x50" alt="صورة رمزية للذكاء الاصطناعي" class="w-12 h-12 rounded-full" />
                <div class="bg-gray-200 p-4 rounded-lg">
                    <p>${aiResponseText}</p>
                </div>
            `;
            chatContainer.appendChild(aiResponse);
            chatContainer.scrollTop = chatContainer.scrollHeight;
        });
    </script>
</body>
</html>
