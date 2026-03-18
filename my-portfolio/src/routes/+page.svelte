<script>
    import ChatBox from '$lib/Chatbox.svelte';
    
    let hasEntered = false;
    let declined = false;

    function enterChat() {
        hasEntered = true;
    }

    function declineChat() {
        declined = true;
    }
</script>

<svelte:head>
    <link href="https://fonts.googleapis.com/css2?family=Great+Vibes&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
</svelte:head>

{#if hasEntered}
    <div class="chat-wrapper">
        <ChatBox />
    </div>

{:else if declined}
    <div class="landing">
        <div class="card">
            <p class="declined-msg">maybe another time 🌸</p>
            <button class="btn-accept small" on:click={enterChat}>actually, let's chat!</button>
        </div>
    </div>

{:else}
    <div class="landing">
        <div class="blobs">
            <div class="blob blob1"></div>
            <div class="blob blob2"></div>
            <div class="blob blob3"></div>
        </div>

        <div class="card">
            <div class="avatar-ring">
                <img src="./headshot.jpeg" alt="simone" class="avatar" />
            </div>

            <div class="text">
                <p class="invite-label">you've been invited to chat with</p>
                <h1 class="name">Simone Chrastek</h1>
                <p class="tagline">cwep swe @ lockheed · computer science @ ucf </p>
            </div>

            <div class="divider"></div>

            <div class="buttons">
                <button class="btn-accept" on:click={enterChat}>
                    <span class="btn-icon">💬</span> accept
                </button>
                <button class="btn-decline" on:click={declineChat}>
                    decline
                </button>
            </div>
        </div>
    </div>
{/if}

<style>
    @import url('https://fonts.googleapis.com/css2?family=Dokdo&family=Parisienne:ital,wght@0,300;0,400;0,500;1,300&display=swap');

    * { box-sizing: border-box; margin: 0; padding: 0; }

    .chat-wrapper {
        min-height: 100svh;
        width: 100%;
        background: #f5eff5;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .landing {
        min-height: 100svh;
        width: 100%;
        background: #fdf0f7;
        display: flex;
        align-items: center;
        justify-content: center;
        font-family: 'DM Sans', sans-serif;
        position: relative;
        overflow: hidden;
    }

    /* Soft floating blobs in background */
    .blobs { position: absolute; inset: 0; pointer-events: none; }

    .blob {
        position: absolute;
        border-radius: 50%;
        filter: blur(70px);
        opacity: 0.45;
        animation: drift 10s ease-in-out infinite alternate;
    }
    .blob1 {
        width: 400px; height: 400px;
        background: #f9c8e8;
        top: -100px; left: -100px;
        animation-delay: 0s;
    }
    .blob2 {
        width: 300px; height: 300px;
        background: #fbd5f0;
        bottom: -80px; right: -80px;
        animation-delay: 3s;
    }
    .blob3 {
        width: 250px; height: 250px;
        background: #f2b8d6;
        top: 50%; left: 60%;
        animation-delay: 6s;
    }

    @keyframes drift {
        from { transform: translate(0, 0) scale(1); }
        to   { transform: translate(30px, 20px) scale(1.08); }
    }

    /* Card */
    .card {
        background: rgba(255, 255, 255, 0.72);
        backdrop-filter: blur(20px);
        border: 1.5px solid rgba(240, 198, 228, 0.6);
        border-radius: 28px;
        padding: 48px 44px 40px;
        width: min(420px, 90vw);
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 24px;
        box-shadow: 0 20px 60px rgba(200, 120, 180, 0.15);
        position: relative;
        animation: rise 0.6s cubic-bezier(0.22, 1, 0.36, 1) both;
    }

    @keyframes rise {
        from { opacity: 0; transform: translateY(24px) scale(0.97); }
        to   { opacity: 1; transform: translateY(0) scale(1); }
    }

    /* Avatar */
    .avatar-ring {
        width: 90px;
        height: 90px;
        border-radius: 50%;
        padding: 3px;
        background: linear-gradient(135deg, #f2b8d6, #fce4f3, #f2b8d6);
        box-shadow: 0 4px 20px rgba(200, 100, 160, 0.3);
    }
    .avatar {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        object-fit: cover;
        display: block;
        border: 2px solid white;
    }

    /* Text */
    .text {
        text-align: center;
        display: flex;
        flex-direction: column;
        gap: 6px;
    }

    .invite-label {
        font-size: 12px;
        font-weight: 400;
        color: #c49ab8;
        letter-spacing: 0.08em;
        text-transform: uppercase;
    }

    .name {
        font-family: 'Great Vibes', cursive;
        font-size: 52px;
        color: #7a3d62;
        line-height: 1.1;
        letter-spacing: 0.01em;
    }

    .tagline {
        font-size: 13px;
        color: #b088a0;
        font-weight: 300;
        margin-top: 2px;
    }

    /* Divider */
    .divider {
        width: 60px;
        height: 1.5px;
        background: linear-gradient(to right, transparent, #f0c6e4, transparent);
        border-radius: 2px;
    }

    /* Buttons */
    .buttons {
        display: flex;
        gap: 12px;
        width: 100%;
    }

    .btn-accept {
        flex: 1;
        padding: 13px 20px;
        background: #f2b8d6;
        border: 1.5px solid #e8a0c8;
        border-radius: 50px;
        font-family: 'DM Sans', sans-serif;
        font-size: 14px;
        font-weight: 500;
        color: #6a2a50;
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        transition: background 0.2s, transform 0.15s, box-shadow 0.2s;
        box-shadow: 0 4px 16px rgba(200, 100, 160, 0.2);
    }
    .btn-accept:hover {
        background: #eda8cc;
        transform: translateY(-2px);
        box-shadow: 0 8px 24px rgba(200, 100, 160, 0.3);
    }
    .btn-accept:active { transform: scale(0.97); }
    .btn-accept.small {
        font-size: 13px;
        padding: 10px 20px;
        width: auto;
        flex: none;
    }

    .btn-decline {
        flex: 1;
        padding: 13px 20px;
        background: transparent;
        border: 1.5px solid #e8d0e0;
        border-radius: 50px;
        font-family: 'DM Sans', sans-serif;
        font-size: 14px;
        font-weight: 400;
        color: #c0a0b8;
        cursor: pointer;
        transition: background 0.2s, color 0.2s, transform 0.15s;
    }
    .btn-decline:hover {
        background: #fce4f3;
        color: #a07090;
        transform: translateY(-2px);
    }
    .btn-decline:active { transform: scale(0.97); }

    /* Declined state */
    .declined-msg {
        font-size: 15px;
        color: #b088a0;
        margin-bottom: 8px;
        text-align: center;
    }

    /* Mobile */
    @media (max-width: 480px) {
        .card { padding: 36px 28px 32px; }
        .name { font-size: 44px; }
    }
</style>