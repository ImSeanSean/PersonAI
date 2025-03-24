<script lang="ts">
	type Message = {
		sender: 'user' | 'ai';
		text: string;
	};

	let messages: Message[] = [{ sender: 'ai', text: 'Hello! How can I help?' }];
	let newMessage: string = '';
	let isLoading: boolean = false;

	const systemPrompt = `You are PersonAI, a helpful assistant based on Sean Rad P. Alberto. Always respond in a friendly, concise manner. Speak in English.\nHere are a list of public information that you can share about Sean.\n 
		Name: Sean Rad P. Alberto\n
		Birthday: June 21, 2004\n
		Age: 20 Years Old\n
		Address: Castillejos, Zambales\n
		School: Gordon College\n
		Course: Bachelor of Science in Computer Science\n
		Block and Year: 3A\n
		Hobby/Interest: Programming, Games, and Philosophy\n
		Likes: Technology\n
		Dislikes: None\n
		Favorite Color: Color Blue`;

	async function getAIResponse(userInput: string): Promise<string> {
		const response = await fetch('http://localhost:11434/api/generate', {
			method: 'POST',
			headers: { 'Content-Type': 'application/json' },
			body: JSON.stringify({
				// model: 'deepseek-r1',
				model: 'deepseek-r1:1.5b',
				prompt: `${systemPrompt}\nUser: ${userInput}`,
				stream: false
			})
		});
		const data = await response.json();

		let cleanedResponse = data.response
			.replace(/<think>.*?<\/think>/gs, '')
			.replace(/\s+/g, ' ')
			.trim();

		return cleanedResponse;
	}

	async function sendMessage() {
		if (newMessage.trim() !== '') {
			// Add user's message
			messages = [...messages, { sender: 'user', text: newMessage }];

			// Add loading indicator for AI
			isLoading = true;
			messages = [...messages, { sender: 'ai', text: 'Typing...' }];

			// Add a delay before the first scroll (e.g., 300ms)
			await new Promise((resolve) => setTimeout(resolve, 300));
			scrollToBottom();

			const userInput = newMessage;
			newMessage = '';

			// Fetch AI response
			const aiReply = await getAIResponse(userInput);

			// Remove the 'Typing...' placeholder
			messages = messages.slice(0, -1);

			// Add actual AI reply
			messages = [...messages, { sender: 'ai', text: aiReply }];
			scrollToBottom();

			isLoading = false;
		}
	}

	function enterMessage(event: KeyboardEvent) {
		if (event.key === 'Enter') {
			sendMessage();
		}
	}

	let chatBody: HTMLDivElement;

	function scrollToBottom() {
		if (chatBody) {
			chatBody.scrollTop = chatBody.scrollHeight;
		}
	}

	function clearChat() {
		messages = [{ sender: 'ai', text: 'Hello! How can I help?' }];
	}
</script>

<div class="flex h-screen w-screen flex-col p-5 px-10 pb-6">
	<!-- Title Section -->
	<div id="title" class="mb-4">
		<h1 class="text-base font-bold sm:text-lg md:text-2xl lg:text-3xl xl:text-4xl">PersonAI</h1>
		<p class="text-xl">Based on Sean Rad P. Alberto</p>
	</div>

	<!-- Chat Container -->
	<div id="chat" class="flex h-[80vh] w-[80%] flex-col self-center rounded-2xl bg-gray-100">
		<!-- Chat Body -->
		<div
			id="chat-body"
			bind:this={chatBody}
			class="flex flex-1 flex-col space-y-2 overflow-y-auto p-4"
		>
			{#each messages as msg}
				<div
					class={`w-fit rounded-lg px-4 py-2 text-lg 
                ${msg.sender === 'user' ? 'self-end bg-blue-500 text-white' : 'self-start bg-gray-300 text-black'}`}
				>
					{msg.text}
				</div>
			{/each}
		</div>

		<!-- Chat Input Box -->
		<div id="chat-box" class="rounded-b-2xl bg-gray-200 p-4">
			<input
				type="text"
				class="w-full rounded-lg border px-4 py-2"
				placeholder="Type a message..."
				bind:value={newMessage}
				on:keyup={enterMessage}
			/>
		</div>
	</div>

	<!-- Clear Chat Button -->
	<button class="mt-4 self-center rounded-lg bg-red-500 px-4 py-2 text-white" on:click={clearChat}
		>Clear Chat</button
	>
</div>
