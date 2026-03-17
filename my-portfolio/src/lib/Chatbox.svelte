<script>
  import { tick } from 'svelte';

  // --- CONFIG ---
  const contactName = "simone chrastek";
  const contactAvatar = "./headshot.jpeg"; // put an image URL here, or leave empty for initials
  const myAvatar = ""; // your avatar URL

  // Conversation "script": each step has a bot message + reply options
  const script = [
    {
      bot: "hey!! so glad you're here 🐻",
      options: ["hey!! same 🥺", "omg hi hi hi"]
    },
    {
      bot: "i'm simone! i'm currently a junior at ucf studying computer science :3 my main focus is full stack development and my dream job is to be a SWE at pinterest 📌 i currently work as a contract software engineer at lockheed martin, and outside of classes and work i'm a workshop director for ucf's girl's who code loop! \n if you want to read more about me, click the hamburger bar in the top right!",
      options: ["can i see your resume?", "cool! just wanted to say hi"]
    },
    {
      bot: "that's so cute honestly 🍓 i've been listening to music all day",
      options: ["same honestly", "ooh what have you been listening to?", "send me a playlist!!"]
    },
    {
      bot: "ok i have to go but let's talk soon 💕",
      options: ["yes please!", "already missing u 🥲", "byeee 💗"]
    },
    {
      bot: "💌",
      options: null // end of conversation
    }
  ];

  let messages = [];
  let currentStep = 0;
  let waitingForReply = false;

  function getTime() {
    const now = new Date();
    let h = now.getHours();
    const m = now.getMinutes().toString().padStart(2, '0');
    const ampm = h >= 12 ? 'pm' : 'am';
    h = h % 12 || 12;
    return `${h}:${m} ${ampm}`;
  }

  async function startChat() {
    if (currentStep < script.length) {
      await addBotMessage(script[currentStep].bot);
    }
  }

  async function addBotMessage(text) {
    waitingForReply = true;
    await tick();
    // simulate typing delay
    await delay(600);
    messages = [...messages, { from: 'bot', text, time: getTime() }];
    await tick();
    scrollToBottom();
  }

  async function choose(option) {
    if (waitingForReply === false) return;
    waitingForReply = false;
    messages = [...messages, { from: 'me', text: option, time: getTime() }];
    await tick();
    scrollToBottom();
    currentStep++;
    if (currentStep < script.length) {
      await addBotMessage(script[currentStep].bot);
    }
  }

  function scrollToBottom() {
    const el = document.getElementById('chat-scroll');
    if (el) el.scrollTop = el.scrollHeight;
  }

  function delay(ms) {
    return new Promise(r => setTimeout(r, ms));
  }

  const pages = [
    {
      id: 'experience',
      label: '💼 experience',
      content: `find me elsewhere:\n— instagram: @simone\n— letterboxd: simone\n— spotify: simone\n— pinterest: simone`
    },
    {
      id: 'projects',
      label: '🔌 projects',
      content: `find me elsewhere:\n— instagram: @simone\n— letterboxd: simone\n— spotify: simone\n— pinterest: simone`
    },
    {
      id: 'skills',
      label: '🛠️ skills',
      content: `languages:\n c/c++, python, java, javascript/typescript, html, css, sql \nframeworks/libraries:\n node.js, react, express, tailwindcss, svelte, mongodb \n tools:\n git/github, visual studio code, figma, eclipse, docker, postman, virtualbox, jira, trello \n operating systems:\n windows, linux, macos`
    },
    {
      id: 'links',
      label: '🔗 links',
      html: `reach out to me here!<br><br>
    — email: <a href="mailto:simone.chrastek5@gmail.com">simone.chrastek5@gmail.com</a><br>
    — linkedin: <a href="https://linkedin.com/in/simone-chrastek" target="_blank" rel="noopener">simone-chrastek</a><br>
    — github: <a href="https://github.com/yourhandle" target="_blank" rel="noopener">yourhandle</a><br>
    — resume: <a href="/resume.pdf" target="_blank" rel="noopener">view ↗</a>`
    }
  ];

  let menuOpen = false;
  let activePage = null;

  function toggleMenu() { menuOpen = !menuOpen; activePage = null; }
  function openPage(page) { activePage = page; }
  function closePopup() { activePage = null; menuOpen = false; }



  // Start conversation on mount
  import { onMount } from 'svelte';
  onMount(() => startChat());

  $: currentOptions = waitingForReply && currentStep < script.length ? script[currentStep].options : null;
</script>

<div class="wrapper">
  <div class="chatbox">

    <!-- Header -->
    <div class="header">
      
      <div class="room-name">simone's chat room</div>
      <button class="menu-btn" aria-label="menu" on:click={toggleMenu}>☰</button>
    </div>

    <!-- Dropdown menu -->
    {#if menuOpen && !activePage}
      <div class="menu-dropdown">
        {#each pages as page}
          <button class="menu-item" on:click={() => openPage(page)}>{page.label}</button>
        {/each}
      </div>
    {/if}

    <!-- Page popup -->
    {#if activePage}
      <div class="popup-overlay" on:click={closePopup}>
        <div class="popup" on:click|stopPropagation>
          <button class="popup-close" on:click={closePopup}>✕</button>
          <h2 class="popup-title">{activePage.label}</h2>
          <p class="popup-content">{activePage.content}</p>
        </div>
      </div>
    {/if}

    <!-- Contact bar -->
    <div class="contact-bar">
      <div class="contact-avatar">
        {#if contactAvatar}
          <img src={contactAvatar} alt={contactName} />
        {:else}
          <div class="avatar-initials">{contactName[0].toUpperCase()}</div>
        {/if}
      </div>
      <div class="contact-info">
        <span class="contact-name">{contactName}</span>
        <span class="status">online</span>
      </div>
      <span class="header-time">{getTime()}</span>
    </div>

    <!-- Messages -->
    <div class="messages" id="chat-scroll">
      {#each messages as msg}
        <div class="message-row {msg.from}">
          {#if msg.from === 'bot'}
            <div class="msg-avatar">
              {#if contactAvatar}
                <img src={contactAvatar} alt={contactName} />
              {:else}
                <div class="avatar-initials sm">{contactName[0].toUpperCase()}</div>
              {/if}
            </div>
          {/if}
          <div class="bubble {msg.from}">{msg.text}</div>
          {#if msg.from === 'me'}
            <div class="msg-avatar my-avatar">
              {#if myAvatar}
                <img src={myAvatar} alt="me" />
              {:else}
                <span>🐻</span>
              {/if}
            </div>
          {/if}
        </div>
      {/each}

      {#if waitingForReply && messages.length === 0}
        <div class="typing-row">
          <div class="typing-dots"><span></span><span></span><span></span></div>
        </div>
      {/if}
    </div>

    <!-- Reply options -->
    {#if currentOptions}
      <div class="options">
        {#each currentOptions as opt}
          <button class="option-btn" on:click={() => choose(opt)}>{opt}</button>
        {/each}
      </div>
    {:else if !waitingForReply && currentStep >= script.length}
      <div class="options end-msg">conversation ended 💌</div>
    {/if}

  </div>
</div>

<style>
  @import url('https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,300;0,400;0,500;1,300&display=swap');

  * { box-sizing: border-box; margin: 0; padding: 0; }

  .wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    padding: 20px;
    font-family: 'DM Sans', sans-serif;
  }

  .chatbox {
    width: min(820px, 100%);
    max-width: 98vw;
    background: #f0eded;
    border-radius: 22px;
    overflow: hidden;
    box-shadow: 0 30px 80px rgba(0,0,0,0.35);
    display: flex;
    flex-direction: column;
    min-height: 520px;
    max-height: 90vh;
    position: relative;
  }

  /* Header */
  .header {
    background: #f2b8d6;
    padding: 18px 24px 14px;
    display: flex;
    align-items: center;
    gap: 12px;
    position: relative;
  }

  .avatar-top {
    width: 44px;
    height: 44px;
    border-radius: 50%;
    overflow: hidden;
    border: 2px solid white;
    display: flex;
    align-items: center;
    justify-content: center;
    background: white;
    font-size: 22px;
    position: absolute;
    top: -10px;
    left: 50%;
    transform: translateX(-50%);
  }
  .avatar-top img { width: 100%; height: 100%; object-fit: cover; }

  .room-name {
    flex: 1;
    text-align: center;
    background: rgba(255,255,255,0.55);
    border-radius: 30px;
    padding: 8px 20px;
    font-size: 15px;
    font-weight: 400;
    color: #444;
    letter-spacing: 0.01em;
  }

  .menu-btn {
    background: none;
    border: none;
    font-size: 20px;
    cursor: pointer;
    color: #555;
    margin-left: 4px;
  }

  /* Contact bar */
  .contact-bar {
    background: #f7cfe6;
    padding: 14px 20px;
    display: flex;
    align-items: center;
    gap: 12px;
    margin: 14px 16px 0;
    border-radius: 14px;
  }

  .contact-avatar {
    width: 42px;
    height: 42px;
    border-radius: 50%;
    overflow: hidden;
    background: #ddd;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }
  .contact-avatar img { width: 100%; height: 100%; object-fit: cover; }

  .avatar-initials {
    font-size: 16px;
    font-weight: 500;
    color: #aaa;
  }
  .avatar-initials.sm { font-size: 12px; }

  .contact-info {
    display: flex;
    flex-direction: column;
    gap: 1px;
  }
  .contact-name { font-weight: 500; font-size: 15px; color: #333; }
  .status { font-size: 12px; color: #888; }
  .header-time { margin-left: auto; font-size: 13px; color: #888; }

  /* Messages */
  .messages {
    flex: 1;
    padding: 18px 20px 12px;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 6px;
    min-height: 180px;
  }

  .message-row {
    display: flex;
    align-items: flex-end;
    gap: 8px;
  }
  .message-row.me { justify-content: flex-end; }
  .message-row.bot { justify-content: flex-start; }

  .msg-avatar {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    overflow: hidden;
    background: white;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 14px;
    flex-shrink: 0;
    border: 1px solid #eee;
  }
  .msg-avatar img { width: 100%; height: 100%; object-fit: cover; }
  .my-avatar { background: #f9e0ef; }

  .bubble {
    padding: 10px 18px;
    border-radius: 22px;
    font-size: 14px;
    line-height: 1.45;
    max-width: min(55%, 400px);
    word-break: break-word;
    animation: pop 0.2s ease;
  }
  .bubble.bot {
    background: #fce4f3;
    border: 1.5px solid #f0c6e4;
    color: #444;
    border-bottom-left-radius: 6px;
  }
  .bubble.me {
    background: #fce4f3;
    border: 1.5px solid #f0c6e4;
    color: #444;
    border-bottom-right-radius: 6px;
  }

  @keyframes pop {
    from { opacity: 0; transform: scale(0.92) translateY(4px); }
    to { opacity: 1; transform: scale(1) translateY(0); }
  }

  .typing-row {
    display: flex;
    padding: 4px 8px;
  }
  .typing-dots {
    display: flex;
    gap: 4px;
    background: #fce4f3;
    border: 1.5px solid #f0c6e4;
    padding: 10px 14px;
    border-radius: 20px;
  }
  .typing-dots span {
    width: 7px; height: 7px;
    background: #d8a0c8;
    border-radius: 50%;
    animation: bounce 1.2s infinite;
  }
  .typing-dots span:nth-child(2) { animation-delay: 0.2s; }
  .typing-dots span:nth-child(3) { animation-delay: 0.4s; }
  @keyframes bounce {
    0%, 60%, 100% { transform: translateY(0); }
    30% { transform: translateY(-5px); }
  }

  /* Options */
  .options {
    padding: 12px 20px 20px;
    display: flex;
    flex-direction: column;
    gap: 8px;
    align-items: flex-end;
  }

  .option-btn {
    background: #fce4f3;
    border: 1.5px solid #f0c6e4;
    border-radius: 22px;
    padding: 10px 22px;
    font-family: 'DM Sans', sans-serif;
    font-size: 13.5px;
    color: #555;
    cursor: pointer;
    transition: background 0.15s, transform 0.1s;
    text-align: left;
    width: fit-content;
    min-width: 200px;
  }
  .option-btn:hover {
    background: #f7cfe6;
    transform: scale(1.02);
  }
  .option-btn:active {
    transform: scale(0.98);
  }

  .end-msg {
    font-size: 13px;
    color: #bbb;
    text-align: center;
    justify-content: center;
    padding-bottom: 24px;
  }

  /* Menu dropdown */
  .menu-dropdown {
    position: absolute;
    top: 58px;
    right: 16px;
    background: white;
    border: 1.5px solid #f0c6e4;
    border-radius: 14px;
    overflow: hidden;
    box-shadow: 0 8px 30px rgba(0,0,0,0.12);
    z-index: 100;
    min-width: 180px;
    animation: pop 0.15s ease;
  }

  .menu-item {
    display: block;
    width: 100%;
    padding: 12px 18px;
    text-align: left;
    background: none;
    border: none;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    color: #555;
    cursor: pointer;
    transition: background 0.1s;
  }
  .menu-item:hover { background: #fce4f3; }
  .menu-item + .menu-item { border-top: 1px solid #f5e0ef; }

  /* Popup overlay */
  .popup-overlay {
    position: absolute;
    inset: 0;
    background: rgba(240, 220, 235, 0.55);
    backdrop-filter: blur(4px);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 200;
    border-radius: 22px;
    animation: fadein 0.2s ease;
  }

  @keyframes fadein {
    from { opacity: 0; }
    to { opacity: 1; }
  }

  .popup {
    background: white;
    border: 1.5px solid #f0c6e4;
    border-radius: 18px;
    padding: 28px 28px 24px;
    width: min(360px, 85%);
    position: relative;
    box-shadow: 0 10px 40px rgba(0,0,0,0.1);
    animation: pop 0.2s ease;
  }

  .popup-close {
    position: absolute;
    top: 14px;
    right: 16px;
    background: none;
    border: none;
    font-size: 16px;
    color: #bbb;
    cursor: pointer;
  }
  .popup-close:hover { color: #888; }

  .popup-title {
    font-size: 16px;
    font-weight: 500;
    color: #444;
    margin-bottom: 14px;
  }

  .popup-content {
    font-size: 14px;
    color: #666;
    line-height: 1.7;
    white-space: pre-line;
  }
</style>