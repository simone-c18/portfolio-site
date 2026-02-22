<script>
    let hasEntered = true;

    function enterChat(){
        hasEntered = true;
    }

</script>

{#if !hasEntered}
    <div class="h-screen w-full bg-black flex flex-col items-center justify-center ">
        <h1 class="text-white text-4xl mb-8 animate-pulse">Simone Chrastek</h1>
        <h2>has invited you to chat!</h2>
        
        <div class="flex flex-row gap-4">
            <button 
            on:click={enterChat}
            class="px-10 py-4 bg-mypink text-black font-bold rounded-lg hover:bg-blue-500 transition-all shadow-[0_0_20px_rgba(37,99,235,0.5)]"
            >
            ACCEPT
            </button>

            <button 
            on:click={enterChat}
            class="px-10 py-4 bg-mypink text-black font-bold rounded-lg hover:bg-blue-500 transition-all shadow-[0_0_20px_rgba(37,99,235,0.5)]"
            >
            DECLINE
            </button>
        </div>
  </div>

{:else}
    <div class="h-screen w-full bg-black flex flex-col items-center justify-center p-4">
        <div class="w-full max-w-4xl aspect-square md:aspect-video bg-white items-center flex justify-center rounded-xl">
            
        </div>

    </div>

{/if}



<!-- <style>

</style> -->


<!-- <p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>  -->
