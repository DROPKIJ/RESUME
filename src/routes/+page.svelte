<script lang="ts">
  import { writable } from 'svelte/store';

  type Section = 'work' | 'personal' | 'uni';
  const sections: Section[] = ['work', 'personal', 'uni'];
  const currentSection = writable<Section>('work');

  type Project = {
    title: string;
    technologies: string;
    features: string;
    images: string[];
  };

  const projects: Record<Section, Project[]> = {
    work: [
      {
        title: "Progressive Web Application",
        technologies: "Next.js, Node.js, React, TailwindCSS, Persistent-storage, RESTful API",
        features: "A real-time monitoring and control solution for company hardware",
        images: ["/FPS.gif", "/FPS1.gif", "/FPS2.gif"]
      }
    ],
    personal: [
      {
        title: "Musix Matrix - Music Theory Game",
        technologies: "Python, Tkinter, Midi equipment detection, sound fonts",
        features: "A minigame I designed to teach music theory",
        images: ["/musicMatrix.gif", "/musicMatrix2.gif"]
      },
      {
        title: "Matrix Audio Player",
        technologies: "PyQt, Python, C++ ",
        features: "Prototype for an Audio Player I designed",
        images: ["/Boxes.gif"]
      }
    ],
    uni: [
      {
        title: "Loop Mania",
        technologies: "Java, JavaFX",
        features: "A tower defense game with RPG elements",
        images: ["/LoopMania.gif"]
      },
      {
        title: "Fury of Dracula",
        technologies: "C, Advanced Abstract Data Structures, Graph Algorithms",
        features: "A digital implementation of the board game 'Fury of Dracula'",
        images: ["/FuryofDracula.gif"]
      }
    ]
  };

  const currentProjectIndex = writable<Record<Section, number>>({
    work: 0,
    personal: 0,
    uni: 0
  });

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
    currentProjectIndex.update(indices => ({ ...indices }));
  }

  function nextProject() {
    currentProjectIndex.update(indices => {
      const section = $currentSection;
      const currentIndex = indices[section];
      const nextIndex = (currentIndex + 1) % projects[section].length;
      return { ...indices, [section]: nextIndex };
    });
  }

  function handleHeaderKeydown(event: KeyboardEvent) {
    if (event.key === 'Enter' || event.key === ' ') {
      event.preventDefault();
      nextSection();
    }
  }

  function handleContentKeydown(event: KeyboardEvent) {
    if (event.key === 'Enter' || event.key === ' ') {
      event.preventDefault();
      nextProject();
    }
  }

  let headerText = "Click here to change project type | Click projects to cycle through [Work Projects only has 1]";
</script>

<main>
  <header class="top-bar">
    <button 
      class="title-box" 
      on:click={nextSection} 
      on:keydown={handleHeaderKeydown}
      aria-label="Click to change section"
    >
      <h1>{getSectionTitle($currentSection)}</h1>
      <p class="header-instructions">{headerText}</p>
    </button>
    <div class="name-box">
      <div>Marcus Grounds</div>
      <div class="email">marcusgrounds.work@gmail.com</div>
    </div>
  </header>
  <section class="content-wrapper" role="main" aria-live="polite">
    <button 
      class="content-button"
      on:click={nextProject}
      on:keydown={handleContentKeydown}
      aria-label="Click to view next project"
    >
      {#if $currentSection === 'work'}
        <div class="work-projects-container">
          <div class="work-projects">
            <div class="work-interface">
              {#each projects.work[$currentProjectIndex.work].images as image}
                <img src={image} alt="Work Interface" class="fps-gif" loading="eager" />
              {/each}
            </div>
            <div class="project-details">
              <h2>{projects.work[$currentProjectIndex.work].title}</h2>
              <ul>
                <li>{projects.work[$currentProjectIndex.work].technologies}</li>
                <li>{projects.work[$currentProjectIndex.work].features}</li>
              </ul>
            </div>
          </div>
        </div>
      {:else if $currentSection === 'personal'}
        <div class="personal-projects-container">
          <div class="personal-projects">
            <div class="personal-interface">
              {#each projects.personal[$currentProjectIndex.personal].images as image}
                <img src={image} alt="Personal Project Interface" class="music-matrix-gif" loading="lazy" />
              {/each}
            </div>
            <div class="project-details">
              <h2>{projects.personal[$currentProjectIndex.personal].title}</h2>
              <ul>
                <li>{projects.personal[$currentProjectIndex.personal].technologies}</li>
                <li>{projects.personal[$currentProjectIndex.personal].features}</li>
              </ul>
            </div>
          </div>
        </div>
      {:else}
        <div class="uni-projects-container">
          <div class="uni-projects">
            <div class="uni-interface">
              {#each projects.uni[$currentProjectIndex.uni].images as image}
                <img src={image} alt="Uni Project Interface" class="uni-gif" loading="lazy" />
              {/each}
            </div>
            <div class="project-details">
              <h2>{projects.uni[$currentProjectIndex.uni].title}</h2>
              <ul>
                <li>{projects.uni[$currentProjectIndex.uni].technologies}</li>
                <li>{projects.uni[$currentProjectIndex.uni].features}</li>
              </ul>
            </div>
          </div>
        </div>
      {/if}
    </button>
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
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  .name-box {
    background-color: #69dcff;
    padding: 10px 20px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-end;
    text-align: right;
  }

  .email {
    font-size: 0.8em;
    margin-top: 5px;
  }
  h1 {
    margin: 0;
  }
  .header-instructions {
    font-size: 0.8em;
    margin: 5px 0 0;
    opacity: 0.8;
  }
  .content-wrapper {
    flex-grow: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
  }
  .content-button {
    padding: 20px;
    background: none;
    border: none;
    color: inherit;
    font: inherit;
    cursor: pointer;
    width: 100%;
    height: 100%;
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
    text-align: left;
  }
  .project-details h2 {
    margin-top: 0;
  }
  .project-details ul {
    list-style-type: disc;
    padding-left: 20px;
    margin-top: 10px;
  }
  .project-details li {
    margin-bottom: 5px;
  }
  @media (prefers-reduced-motion: reduce) {
    .fps-gif, .music-matrix-gif, .uni-gif {
      animation: none;
    }
  }
</style>