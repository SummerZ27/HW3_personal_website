<script>
  import { base } from '$app/paths';
</script>

<svelte:head>
  <title>Resume - Summer Zhou</title>
</svelte:head>

<nav>
  <a href=".">Home</a>
  <a href="projects">Projects</a>
  <a href="contact">Contact</a>
  <a class="current" href="resume">Resume</a>
  <a href="https://github.com/SummerZ27" target="_blank" rel="noopener noreferrer">GitHub</a>
</nav>

<main class="resume-page">
  <div class="resume-header">
    <h1>Resume</h1>
    <a href="{base}/files/resume.pdf" target="_blank" rel="noopener noreferrer" class="pdf-button">PDF Version</a>
  </div>

  <div class="resume-content">
    <section class="resume-heading">
      <div class="heading-info">
        <h2>Summer Zhou</h2>
        <div class="contact-info">
          <p><strong>Email:</strong> <a href="mailto:xzhou27@mit.edu">xzhou27@mit.edu</a></p>
          <p><strong>Mobile:</strong> +1-475-243-5329</p>
          <p>Cambridge, MA</p>
        </div>
      </div>
    </section>

    <section>
      <h2>Education</h2>
      <article>
        <div class="resume-item-header">
          <div>
            <h3>Massachusetts Institute of Technology (MIT)</h3>
            <p class="location">Cambridge, MA</p>
          </div>
          <div class="dates">
            <time datetime="2023">2023</time> -- <time datetime="2027">2027</time>
          </div>
        </div>
        <p class="degree">Bachelor of Science in Computer Science and Engineering (6-3)</p>
        <p><strong>Relevant Coursework:</strong> Discrete Math, Probability and Random Variables, Fundamentals of Statistics, Linear Algebra, Differential Equations, Multivariable Calculus, Design and Analysis of Algorithms, Computation Structures, Quantitative Methods for Natural Language Processing, Introduction to C and Assembly, Introduction to Machine Learning, Introduction to Algorithms</p>
      </article>
    </section>

    <section>
      <h2>Past Internship</h2>

      <article>
        <div class="resume-item-header">
          <div>
            <h3>Tower Research Capital</h3>
            <p class="location">New York, NY</p>
          </div>
          <div class="dates">
            <time datetime="2026-06">Summer 2026</time>
          </div>
        </div>
        <p class="position">Incoming Quantitative Trading Intern</p>
      </article>

      <article>
        <div class="resume-item-header">
          <div>
            <h3>Biostate AI Inc.</h3>
            <p class="location">Palo Alto, CA/Remote</p>
          </div>
          <div class="dates">
            <time datetime="2024-11">Nov 2024</time> -- <time datetime="2025-08">Aug 2025</time>
          </div>
        </div>
        <p class="position">Software Engineering/Machine Learning Intern</p>
        <ul>
          <li>Contributed to building a full-stack ML pipeline for prognostic signal prediction from RNA-seq data, experimented with self-supervised learning, foundation model integration, and Transformer variants. Results on my research can be found at <a href="https://www.biorxiv.org/content/10.1101/2025.08.15.670537v1" target="_blank" rel="noopener noreferrer">https://www.biorxiv.org/content/10.1101/2025.08.15.670537v1</a></li>
          <li>Produced benchmark results for the K-Dense Analyst system, a hierarchical multi-agent system we created for fully automated bioinformatics analysis, as described in <a href="https://arxiv.org/abs/2508.07043" target="_blank" rel="noopener noreferrer">https://arxiv.org/abs/2508.07043</a>.</li>
          <li>Designed and built a full-stack, responsive web portal for rapid gene information lookup, emphasizing broad data coverage, performance, and a user-friendly interface. Website: <a href="https://gene.biostate.ai/" target="_blank" rel="noopener noreferrer">https://gene.biostate.ai/</a></li>
          <li>Built a comprehensive pipeline that retrieves published papers, ranks their relevancy, and integrates the Claude API to generate, score, and refine blog posts through prompt engineering.</li>
          <li>Produced detailed technical patent diagrams for "Profiling Expressed Mutations Through Normalized RNA Sequencing."</li>
        </ul>
      </article>

      <article>
        <div class="resume-item-header">
          <div>
            <h3>MIT-IBM Watson AI Lab, MIT ORC</h3>
            <p class="location">Cambridge, MA</p>
          </div>
          <div class="dates">
            <time datetime="2024-06">Summer 2024</time>
          </div>
        </div>
        <p class="position">Machine Learning Research Intern</p>
        <ul>
          <li>Partnered with MIT Operations Research Center and IBM researchers to develop Transformer and LSTM time-series forecasting models for Boston Scientific, significantly reducing MAPE relative to baseline.</li>
          <li>Developed hierarchical loss functions for multi‑tier sales forecasting, leveraging softmax‑based ratio normalization to ensure consistency across aggregation levels.</li>
          <li>Processed and prepared large-scale sales datasets using NumPy and pandas.</li>
        </ul>
      </article>

      <article>
        <div class="resume-item-header">
          <div>
            <h3>Tencent</h3>
            <p class="location">Shenzhen, China</p>
          </div>
          <div class="dates">
            <time datetime="2023-06">Summer 2023</time>
          </div>
        </div>
        <p class="position">Machine Learning Research Intern</p>
        <ul>
          <li>Optimized ML-based super-resolution models (ECBSR, ESRGAN) to enhance graphics quality in Tencent gaming and streaming products, achieving improvement over the benchmark in visual quality metrics while reducing inference time.</li>
          <li>Conducted literature review and research on state-of-the-art super-resolution models to inform optimization strategies.</li>
        </ul>
      </article>

      <article>
        <div class="resume-item-header">
          <div>
            <h3>Research Science Institute</h3>
            <p class="location">Cambridge, MA</p>
          </div>
          <div class="dates">
            <time datetime="2022-06">Summer 2022</time>
          </div>
        </div>
        <p class="position">Number Theory Research</p>
        <ul>
          <li>Researched in MIT Mathematics Department, investigated "Products of Values of Polynomials in Finite Fields," applying combinatorial and algebraic techniques to derive new theorems.</li>
        </ul>
      </article>
    </section>

    <section>
      <h2>Awards</h2>
      <ul>
        <li>2019--2023 USACO (United States of America Computing Olympiad) -- Platinum Division Qualifier</li>
        <li>2022 NACLO (North America Computational Linguistics Olympiad) -- Invitational Round 19th, 17th in US, 1st in MA</li>
        <li>2022 Yale Girls in Math -- Individual Top 15</li>
        <li>2020 ARML China (American Regional Mathematics League with ASDAN China) -- National Individual Top 100</li>
        <li>2018--2023 American Math Competition -- AIME Qualifier</li>
      </ul>
    </section>

    <section>
      <h2>Skills</h2>
      <ul>
        <li><strong>Programming Languages:</strong> Python, C++, C, Java, HTML, CSS, React, LaTeX</li>
        <li><strong>Libraries:</strong> React, TensorFlow, PyTorch, scikit-learn, NumPy, Pandas</li>
        <li><strong>Additional Skills:</strong> Machine Learning, Natural Language Processing, Computer Vision, App Development, Web Development</li>
      </ul>
    </section>
  </div>
</main>
