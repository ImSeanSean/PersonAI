<div class="flex h-screen w-screen flex-col p-5 px-10 pb-6">
	<!-- Title Section -->
	<div id="title" class="mb-4">
		<h1 class="text-base font-bold sm:text-lg md:text-2xl lg:text-3xl xl:text-4xl">PersonAI</h1>
		<p class="text-xl">Based on Sean Rad P. Alberto</p>
	</div>

	<!-- Chat Container -->
	<div id="chat" class="flex w-[80%] flex-1 flex-col self-center rounded-2xl bg-gray-100">
		<!-- Chat Body -->
		<div id="chat-body" class="flex flex-1 flex-col overflow-y-auto p-4">
			<div class="w-fit self-start rounded-lg bg-gray-300 px-4 py-2 text-lg">
				Hello! How can I help?
			</div>
			<div class="w-fit self-end rounded-lg bg-blue-500 px-4 py-2 text-lg text-white">
				Hi! I need some info.
			</div>
		</div>

		<!-- Chat Input Box -->
		<div id="chat-box" class="rounded-b-2xl bg-gray-200 p-4">
			<input
				type="text"
				class="w-full rounded-lg border px-4 py-2"
				placeholder="Type a message..."
			/>
		</div>
	</div>
</div>
