<script>
  import { tick, onMount } from 'svelte';

  // --- CONFIG ---
  const contactName = "simone chrastek";
  const contactAvatar = "./headshot.jpeg";
  const myAvatar = "";

  const script = {
    step0: {
      bot: "hey!! so glad you're here :3",
      options: [
        { label: "hey!! same", next: "step1" },
        { label: "omg hii", next: "step1" }
      ]
    },
    step1: {
      bot: "i'm simone! i'm currently a junior at ucf studying computer science :3 i'm an aspiring full stack developer and my dream job is to be a SWE at pinterest 📌 i currently work as a contract software engineer at lockheed martin, and outside of classes and work i'm a workshop director for ucf's girl's who code loop! \n if you want to read more about me, click the hamburger bar in the top right!",
      options: [
        { label: "can i see your resume?", next: "resume" },
        { label: "cool! i'm gonna look around a bit..", next: "step2" }
      ]
    },
    step2: {
      bot: "is there anything else you want to chat about?",
      options: [
        { label: "what's your favorite project that you've worked on?", next: "project" },
        { label: "why do you enjoy fullstack development?", next: "enjoy" },
        { label: "what type of music are you into?", next: "music" },
        { label: "i gtg", next: "end" }
      ]
    },
    enjoy: {
      bot: "my favorite thing about fullstack dev is that it allows for me to be versatile in what i'm working on! i can work on either frontend or backend and have the overall ability to build something end-to-end. i think it's also fascinating to understand how an application works in its entirety -- how each component interacts with one another to mend into one, cohesive product. the overall process of building a full scale project is always daunting, but rewarding :)",
      options: [
        { label: "awesome!", next: "step2" },
      ]
    },
    project: {
      bot: "i'm currently head of ucf's girls who code web dev team, where we are building the club's website! although it isn't my most technically challenging project, i'm quite happy to have the opportunity to contribute back to a club that has helped me in so many ways! i designed the entire figma for the site, and i'm able to work with members of the club and use a modern frontend stack to create the site (which btw was long overdue!!)",
      options: [
        { label: "wow! that sounds pretty cool :3", next: "step2" },
        { label: "girls who code is so epic!!", next: "step2" }
      ]
    },
    music: {
      messages: [
        "my music taste is allll over the place, so it's hard to pinpoint a specific genre.. but here's a list of some artists i listen to a lot!",
        "jeff buckley, fka twigs, adela, old p!atd, rehash, pierce the veil, and deftones!"
      ],
      options: [
        { label: "your music taste is elite", next: "step2" },
        { label: "i don't know any of those artists..", next: "step2"}
      ]
    },
    resume: {
      bot: "of course! click here to view it :)",
      link: { text: "here", href: "/resume.pdf" },
      options: [
        { label: "thanks!", next: "step2" }
      ]
    },
    end: {
      bot: "aw, it was fun to talking with you! reach out to me via linkedin or email if you'd like to keep the convo going :3",
      link: { text: "linkedin", href: "https://www.linkedin.com/in/simone-chrastek/" },
      options: null
    }
  };

  let messages = [];
  let isTyping = false;
  let currentStep = 'step0';
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
    await addBotMessage(script[currentStep]);
  }

  async function addBotMessage(step) {
    waitingForReply = true;
    isTyping = true;
    await tick();

    const texts = step.messages ?? [step.bot];

    for (const text of texts) {
      await delay(800);
      messages = [...messages, { from: 'bot', text, link: step.link ?? null, time: getTime() }];
      await tick();
      scrollToBottom();
    }

    isTyping = false;
  }

  async function choose(option) {
    if (!waitingForReply) return;
    waitingForReply = false;
    messages = [...messages, { from: 'me', text: option.label, time: getTime() }];
    await tick();
    scrollToBottom();
    currentStep = option.next;
    if (script[currentStep]) {
      await addBotMessage(script[currentStep]);
    }
    scrollToBottom();
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
      sections: [
        { heading: 'contract software engineer', items: 'contract software engineer — present' },
        { heading: 'workshop director', items: 'workshop director — present' },
        { heading: 'college mentor', items: 'girls who code' },
      ]
    },
    {
      id: 'projects',
      label: '🔌 projects',
      sections: [
        { heading: 'girls who code website', href: 'https://github.com/simone-c18/GWC_WebDev', items: 'designed figma & leading web dev team to build club website using node.js + tailwind' },
        { heading: 'ecosorter', items: 'developed REST api endpoints using express for user auth and data retrieval', href: 'https://github.com/mariamauco/Fall2025WEECSLarge' },
        { heading: 'mealminder', items: 'ai powered ingredient tracker + recipe generator; developed UI using react + tailwind ', href: 'https://github.com/izzyaustt/MealMinder' },
        { heading: 'codecade', items: 'competed in knighthacks project showcase; built and designed UI + implemented JS logic handling', href: 'https://github.com/Kat-Gumerov/LearningBehindTheScreens' },
        { heading: 'intro to love', items: 'competed in gwc projects showcase; acted as PM to build interactive game to learn different coding languages using node.js + react', href: 'https://github.com/simone-c18/gwc-group-9' },
      ]
    },
    {
      id: 'skills',
      label: '🛠️ skills',
      sections: [
        { heading: 'languages', items: 'c/c++, python, java, javascript/typescript, html, css, sql' },
        { heading: 'frameworks/libraries', items: 'node.js, react, express, tailwindcss, svelte, mongodb' },
        { heading: 'tools', items: 'git/github, visual studio code, figma, eclipse, docker, postman, virtualbox, jira, trello' },
        { heading: 'operating systems', items: 'windows, linux, macos' }
      ]
    },
    {
      id: 'links',
      label: '🔗 links',
      sections: [
        { heading: 'email', items: 'simone.chrastek5@gmail.com', itemHref: 'mailto:simone.chrastek5@gmail.com' },
        { heading: 'linkedin', items: 'linkedin.com/in/simone-chrastek', itemHref: 'https://linkedin.com/in/simone-chrastek' },
        { heading: 'github', items: 'github.com/simone-c18', itemHref: 'https://github.com/simone-c18' },
        { heading: 'resume', items: 'view/download ↗', itemHref: '/resume.pdf' }
      ]
    }
  ];

  let menuOpen = false;
  let activePage = null;

  function toggleMenu() { menuOpen = !menuOpen; activePage = null; }
  function openPage(page) { activePage = page; }
  function closePopup() { activePage = null; menuOpen = false; }

  onMount(() => startChat());

  $: currentOptions = waitingForReply && !isTyping && script[currentStep]
    ? script[currentStep].options
    : null;
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
      <div class="popup-overlay" role="dialog" aria-modal="true" on:click={closePopup} on:keydown={closePopup}>
        <div class="popup" role="presentation" on:click|stopPropagation on:keydown|stopPropagation>
          <button class="popup-close" on:click={closePopup}>✕</button>
          <h2 class="popup-title">{activePage.label}</h2>
          {#each activePage.sections as section}
            <div class="popup-section">
              <span class="popup-heading">
                {#if section.href}
                  <a href={section.href} target="_blank" rel="noopener">{section.heading}</a>
                {:else}
                  {section.heading}
                {/if}
              </span>
              <p class="popup-items">
                {#if section.itemHref}
                  <a href={section.itemHref} target="_blank" rel="noopener">{section.items}</a>
                {:else}
                  {section.items}
                {/if}
              </p>
            </div>
          {/each}
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
          <div class="bubble {msg.from}">
            {#if msg.link}
              {msg.text.split(msg.link.text)[0]}<a href={msg.link.href} target="_blank" rel="noopener">{msg.link.text}</a>{msg.text.split(msg.link.text)[1]}
            {:else}
              {msg.text}
            {/if}
          </div>
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
          <button class="option-btn" on:click={() => choose(opt)}>{opt.label}</button>
        {/each}
      </div>
    {:else if !waitingForReply && !script[currentStep]?.options}
      <div class="options end-msg">conversation ended 💌</div>
    {/if}

  </div>
</div>

<style>
  @import url('https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,300;0,400;0,500;0,600;1,300&display=swap');

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

  .bubble a {
    color: #c46ea0;
    text-decoration: underline;
    text-underline-offset: 2px;
  }
  .bubble a:hover { color: #a0507e; }

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
    color: #444;
    border: 1.5px solid #f0c6e4;
    border-radius: 18px;
    padding: 28px 28px 24px;
    width: min(360px, 85%);
    max-height: 75vh;
    overflow-y: auto;
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

  .popup-section {
    margin-bottom: 16px;
  }
  .popup-section:last-child {
    margin-bottom: 0;
  }
  .popup-heading {
    display: block;
    font-weight: 600;
    font-size: 14px;
    color: #444;
    margin-bottom: 4px;
    letter-spacing: 0.01em;
  }
  .popup-heading a {
    color: #444;
    text-decoration: underline;
    text-underline-offset: 3px;
    font-weight: 600;
  }
  .popup-heading a:hover { color: #b06090; }
  .popup-items {
    font-size: 13.5px;
    color: #888;
    line-height: 1.6;
  }
  .popup-items a {
    color: #c46ea0;
    text-decoration: underline;
    text-underline-offset: 2px;
  }
  .popup-items a:hover { color: #a0507e; }

  /* Mobile */
@media (max-width: 600px) {
  .wrapper {
    padding: 0;
    align-items: flex-end;
  }

  .chatbox {
    border-radius: 22px 22px 0 0;
    min-height: 100svh;
    max-height: 100svh;
    width: 100%;
  }

  .bubble {
    max-width: 75%;
  }

  .option-btn {
    min-width: 160px;
    font-size: 13px;
    padding: 9px 16px;
  }

  .popup {
    width: 90%;
    max-height: 70vh;
    padding: 22px 20px 20px;
  }

  .contact-bar {
    margin: 10px 10px 0;
  }

  .messages {
    padding: 14px 14px 10px;
  }

  .options {
    padding: 10px 14px 16px;
  }
}

</style>