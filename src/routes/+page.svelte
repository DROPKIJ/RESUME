<script lang="ts">
    import { writable } from 'svelte/store';
  
    type Section = 'work' | 'personal' | 'uni';
    const sections: Section[] = ['work', 'personal', 'uni'];
    const currentSection = writable<Section>('work');
  
    function getSectionTitle(section: Section): string {
      switch(section) {
        case 'work': return 'Work Projects';
        case 'personal': return 'Personal Projects';
        case 'uni': return 'Uni Projects';
      }
    }
  
    function nextSection() {
      currentSection.update(current => {
        const currentIndex = sections.indexOf(current);
        return sections[(currentIndex + 1) % sections.length];
      });
    }
  
    function handleKeydown(event: KeyboardEvent) {
      if (event.key === 'Enter' || event.key === ' ') {
        event.preventDefault();
        nextSection();
      }
    }
  </script>
  
  <main>
    <header class="top-bar">
      <button 
        class="title-box" 
        on:click={nextSection} 
        on:keydown={handleKeydown}
        aria-label="Click to change section"
        tabindex="0"
      >
        <h1>{getSectionTitle($currentSection)}</h1>
      </button>
      <div class="name-box">Marcus Grounds</div>
    </header>
    <section class="content" role="main" aria-live="polite">
      {#if $currentSection === 'work'}
        <div class="work-projects-container">
          <div class="work-projects">
            <div class="work-interface">
              <img src="/FPS.gif" alt="FPS Interface 1" class="fps-gif" loading="eager" />
              <img src="/FPS1.gif" alt="FPS Interface 2" class="fps-gif" loading="eager" />
              <img src="/FPS2.gif" alt="FPS Interface 3" class="fps-gif" loading="eager" />
            </div>
            <div class="project-details">
              <h2>Progressive Web Application</h2>
              <ul>
                <li>Next.js, Node.js, React, TailwindCSS</li>
                <li>Persistent storage, real-time feedback, RestfulAPI</li>
              </ul>
            </div>
          </div>
        </div>
      {:else if $currentSection === 'personal'}
        <div class="personal-projects-container">
          <div class="personal-projects">
            <div class="personal-interface">
              <img src="/musicMatrix.gif" alt="Music Matrix Interface 1" class="music-matrix-gif" loading="lazy" />
              <img src="/musicMatrix2.gif" alt="Music Matrix Interface 2" class="music-matrix-gif" loading="lazy" />
            </div>
            <div class="project-details">
              <h2>Musix Matrix - Music Theory Game</h2>
              <ul>
                <li>Python, Tkinter</li>
                <li>Midi equipment detection, sound fonts</li>
              </ul>
              <p>more text ...</p>
            </div>
          </div>
        </div>
      {:else}
        <div class="uni-projects-container">
          <div class="uni-projects">
            <div class="uni-interface">
              <img src="/LoopMania.gif" alt="Uni Project 1" class="uni-gif" loading="lazy" />
            </div>
            <div class="project-details">
              <h2>University Project Title</h2>
              <ul>
                <li>Technologies used in uni projects</li>
                <li>Key features of uni projects</li>
              </ul>
              <p>Description of the university project...</p>
            </div>
          </div>
        </div>
      {/if}
    </section>
  </main>
  
  <style>
    :global(body, html) {
      margin: 0;
      padding: 0;
      height: 100%;
      background-color: #333;
      color: white;
      font-family: Arial, sans-serif;
    }
    main {
      width: 100%;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }
    .top-bar {
      display: flex;
      width: 100%;
    }
    .title-box {
      flex-grow: 1;
      background-color: #6a5acd;
      padding: 10px 20px;
      cursor: pointer;
      border: none;
      color: inherit;
      font: inherit;
      text-align: left;
    }

    .name-box {
      background-color: #ff69b4;
      padding: 10px 20px;
      display: flex;
      align-items: center;
    }
    h1 {
      margin: 0;
    }
    .content {
      flex-grow: 1;
      padding: 20px;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    .work-projects-container, .personal-projects-container, .uni-projects-container {
      background-color: #444;
      padding: 20px;
      border-radius: 5px;
    }
    .work-projects-container {
      width: 1300px;
    }
    .personal-projects-container {
      width: 1000px;
      height: auto;
    }
    .uni-projects-container {
      width: 1100px;
    }
    .work-projects, .personal-projects, .uni-projects {
      display: flex;
      gap: 20px;
      width: 100%;
      height: 100%;
    }
    .work-projects {
      flex-direction: row;
    }
    .work-interface, .personal-interface, .uni-interface {
      flex: 1;
      display: flex;
      background-color: #555;
      padding: 15px;
      border-radius: 5px;
    }
    .work-interface {
      flex: 2;
      justify-content: space-between;
      align-items: center;
      overflow-x: auto;
    }
    .personal-interface, .uni-interface {
      flex-direction: column;
      justify-content: center;
      align-items: center;
      gap: 15px;
    }
    .fps-gif, .music-matrix-gif, .uni-gif {
      width: 100%;
      height: auto;
      display: block;
    }
    .project-details {
      flex: 1;
      background-color: #555;
      padding: 15px;
      border-radius: 5px;
      overflow-y: auto;
    }
    h2 {
      margin-top: 0;
    }
    ul {
      padding-left: 20px;
    }
    @media (prefers-reduced-motion: reduce) {
      .fps-gif, .music-matrix-gif, .uni-gif {
        animation: none;
      }
    }
  </style>