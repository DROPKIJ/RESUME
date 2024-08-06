<!-- WorkProjects.svelte -->
<script>
    // Component logic here
  </script>
  
  <div class="projects-grid">
    <div class="project-card">
      <!-- Project card content -->
    </div>
    <div class="project-card">
      <!-- Project card content -->
    </div>
  </div>
  
  <div class="project-details">
    <h2>Propriety software to control embedded systems</h2>
    <ul>
      <li>React, nextjs, etc etc</li>
      <li>Persistent storage, real time feedback, etc</li>
    </ul>
    <p>more text ...</p>
  </div>
  
  <style>
    .projects-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 20px;
    }
    
    .project-card {
      background-color: #ddd;
      padding: 10px;
      border-radius: 5px;
    }
    
    .project-details {
      margin-top: 20px;
    }
  </style>