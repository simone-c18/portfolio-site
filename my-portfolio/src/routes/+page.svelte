<script>
    import ChatBox from '$lib/Chatbox.svelte';
    
    let hasEntered = false;

    function enterChat() {
        hasEntered = true;
    }
</script>

{#if !hasEntered}
    <div class="h-screen w-full bg-black flex flex-col items-center justify-center gap-6">
        <h1 class="text-white text-4xl animate-pulse">Simone Chrastek</h1>
        <h2 class="text-white text-lg">has invited you to chat!</h2>
        
        <div class="flex flex-row gap-4">
            <button 
                on:click={enterChat}
                class="px-10 py-4 bg-pink-300 text-black font-bold rounded-lg hover:bg-pink-400 transition-all"
            >
                ACCEPT
            </button>

            <button 
                class="px-10 py-4 bg-gray-700 text-white font-bold rounded-lg hover:bg-gray-600 transition-all"
            >
                DECLINE
            </button>
        </div>
    </div>

{:else}
    <div class="min-h-screen w-full bg-black flex items-center justify-center">
        <ChatBox />
    </div>
{/if}