<svelte:head>
  <title>Projects - Summer Zhou</title>
</svelte:head>

<nav>
  <a href=".">Home</a>
  <a class="current" href="projects">Projects</a>
  <a href="contact">Contact</a>
  <a href="resume">Resume</a>
  <a href="https://github.com/SummerZ27" target="_blank" rel="noopener noreferrer">GitHub</a>
</nav>

<main>
  <h1>Projects</h1>

  <div class="projects">
    <section>
      <h2>Dramaverse (Agentic AI Short-Drama Platform)</h2>
      <div class="project-content">
        <img src="/images/dramaverse.png" alt="Dramaverse AI Short-Drama Platform interface">
        <p>Built an end-to-end agentic workflow for AI-native short-drama creation, integrating multiple specialized agents (scriptwriting, art direction, casting, keyframe generation, video synthesis, voice, and sound design) into a coherent production pipeline. Developed LoRA-based character consistency training (60+ curated images per character) to preserve identity across scenes, and experimented with ControlNet, depth conditioning, and high-resolution generation workflows on GPU clusters (RunPod). Focused on solving core challenges in AI video generation — consistent faces, cinematic camera motion, lighting realism, and natural lip-sync — aiming to move beyond "AI-slop" toward scalable, studio-quality storytelling.</p>
      </div>
    </section>

    <section>
      <h2>Toki (Manufacturing Scheduling & ERP Layer)</h2>
      <p>Designed and implemented a dynamic production-line scheduling system for a mid-sized automotive glass manufacturing factory, modeling machine-specific constraints (tempering lines, lamination lines, frame lines) and urgent-order prioritization logic. Built a pipeline scheduler that integrates multi-department inputs and dynamically updates feasible production sequences under operational constraints. Focused on real-world systems integration challenges — aligning heterogeneous data sources, designing Excel-compatible uploads for factory workers, and constructing a modular SaaS layer that can evolve toward a vertically-tailored ERP platform for manufacturing optimization.</p>
    </section>

    <section>
      <h2>Gene Information Web Application</h2>
      <p>Developed a full-stack bioinformatics web application that retrieves and aggregates gene-related data from Ensembl and MyGene APIs, including canonical mRNA/CDS sequences, GC content, and external resource links. Implemented batch query processing to significantly reduce API latency and structured logic to extract a unique, biologically relevant CDS per gene symbol among multiple transcript variants. Redesigned the UI with a structured grid layout and custom dark theme, integrating dynamic CSV-based external resources (NCBI, Google Scholar, Ensembl) to create a cohesive, research-oriented gene exploration interface.</p>
    </section>

    <section>
      <h2>Time Series Forecasting via Modified Transformer Attention Mechanism</h2>
      <p>Conducted research on hierarchical revenue forecasting for a medical device company, replacing standard Transformer attention with a modified attention-on-attention mechanism to better capture multi-level temporal dependencies and cross-series coherence. Experimented with linear models, LSTMs, and baseline Transformers before introducing architectural changes that reweighted temporal features and enforced hierarchical consistency through customized loss functions (MSE, weighted aggregation loss). Evaluated performance across granular revenue levels, focusing on improving long-horizon forecast stability and ratio alignment across product hierarchies.</p>
    </section>
  </div>
</main>
