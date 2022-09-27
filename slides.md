---
title: 'Data Science on Polaris'
center: true
width: 960
height: 540
bg: #1D1651
transition: slide
preloadIframes: false
highlightTheme: 'monokai'
maxScale: 20.0
date created: Monday, May 16th 2022, 7:07:14 am
date modified: Wednesday, September 21st 2022, 9:18:51 pm
---

<!-- .slide bg="[[assets/background.png]]" -->

<grid align="center" drop="5 20" drag="90 60" style="background-color:#1D1651; border-radius:8px; opacity:0.9;">

# Python on Polaris <!-- .element style="color:white; font-size:2.25em; padding-top:5%; border-bottom:8px solid #1e8BC9; padding-bottom:2%; text-align:center!important;" align="stretch" -->

[<i class="fas fa-book fa-1x" alt="`fas:Book`"/> User Guides](https://argonne-lcf.github.io/user-guides/polaris/data-science-workflows/python/) <!-- .element style="font-size:1.1em; color:#B8E2DE!important; font-family:'JuliaMono', 'Courier New';line-height:0.7em;" -->

[<i class="fab fa-github fa-1x" alt="`fas:Github`"/> Getting Started](https://github.com/argonne-lcf/GettingStarted) <!-- .element style="font-size:1.1em; color:#B8E2DE!important; font-family:'JuliaMono', 'Courier New';line-height:0.7em;" -->

<a href="https://www.samforeman.me"><i class="fas fa-home fa-1x" alt="`fas:Home`" style="padding-right:2px;"/></a> Sam Foreman <!-- .element style="color:#655A90;" -->

2022-09-28 <!-- .element style="font-size:0.9em; color:#5E5381;padding:auto;margin:auto;atext-align:center!important;" -->

</grid>

<grid drag="100 20" drop="bottom" align="bottomright" >
<img src="https://raw.githubusercontent.com/saforem2/physicsSeminar/main/assets/Argonne_cmyk_white.svg" alt="Argonne National Laboratory">
</grid>


---

<!-- .slide template="[[left-border]]" -->

# Using Conda

- Additional information in our [<i class="fas fa-book fa-1x" alt="`fas:Book`"/> user-guide](https://argonne-lcf.github.io/user-guides/polaris/data-science-workflows/python/)
- We provide prebuilt `conda` environment containing GPU-supported builds of:
	- `pytorch`, `tensorflow` both with `horovod` support
	- `jax`, many other commonly used libraries
- To activate / use: (either from an interactive job or inside a job script):
	- `module load conda; conda activate base`

---

<!-- .slide template="[[left-border]]" style="font-size:0.8em;" -->

## Virtual Environments: `venv`

- To install additional libraries, we can create a virtual environment using `venv`
- Make sure you're currently inside the **base** `conda` environment:
	- `module load conda; conda activate base`
- Now, create `venv` **on top of** `base`:
  <pre><code class="bash" data-trim data-noescape>
  $ python3 -m venv /path/to/venv --system-site-packages
  $ source /path/to/venv/bin/activate
  $ which python3
  /path/to/venv/bin/python3
  $ # Now you can `python3 -m pip install ...` etc
  </code></pre>

> [!warning] Warning!
> 1. `--system-site-packages` tells the `venv` to use system packages
> 2. You must replace the path `/path/to/venv` in the above commands with a suitably chosen directory which you are able to write to.


---

<!-- .slide template="[[left-border]]" -->

# Note about `venv`'s

- The value of `--system-site-packages` can be changed by modifying its value in `/path/to/venv/pyvenv.cfg`
- To install a **different** version of a package that is already installed in the base environment:

  ```bash
  $ python3 -m pip install --ignore-installed ... # or -I
   ```

- The shared `base` environment is not writable
	- Impossible to remove or uninstall packages


- If you need additional flexibility, we can **clone** the base environment

---

<!-- .slide template="[[left-border]]" -->

# Clone base `conda` environment

- If we need additional flexibility or to install packages which **require** a `conda` install, we can clone the base environment
	- requires copying the entirety of the base environment
	- **large storage requirement**, can get out of hand quickly
- The shared `base` environment is not writable
	- Impossible to remove or uninstall packages
- This can be done by:

  ```
  $ module load conda
  $ conda activate base
  (base) $ conda create --clone base --prefix="/path/to/envs/base-clone"
  ```


---
<!-- .slide template="[[left-border]]" -->

# Containers on Polaris[^1]
- Polaris uses Nvidia A100 GPUs -->
	- We can take advantage of Nvidia optimized containers

- The container system on Polaris is [`singularity`](https://docs.sylabs.io/guides/3.5/user-guide/introduction.html):

  ```bash
  module avail singularity # see available
  module load singularity  # load default version
  # To load a specific version:
  module load singularity/3.8.7
  ```
[^1]: [<i class="fas fa-book fa-1x" alt="`fas:Book`"/> Containers - ALCF User Guides](https://argonne-lcf.github.io/user-guides/polaris/data-science-workflows/containers/containers/)

---

<!-- .slide template="[[left-border]]" style="text-align:left!important;" -->

# Singularity

- Two options for creating containers:
	1. Using Docker on local machine and publishing to DockerHub
	2. Using a Singularity recipe file and building on a Polaris worker node

- Detailed instructions with additional information can be found at:
	- [<i class="fas fa-book fa-1x" alt="`fas:Book`"/> Containers - ALCF User Guides](https://argonne-lcf.github.io/user-guides/polaris/data-science-workflows/containers/containers/)

---

<!-- .slide template="[[left-border]]" style="text-align:left!important;" -->

# Frameworks

- All of the modern ML frameworks are installed and available from the provided `base` conda environment.
- Additional information (including general tips / best practices) can be found at:
	- [<i class="fas fa-book fa-1x" alt="`fas:Book`"/> JAX - ALCF User Guides](https://argonne-lcf.github.io/user-guides/polaris/data-science-workflows/frameworks/jax/)
	- [<i class="fas fa-book fa-1x" alt="`fas:Book`"/> Pytorch - ALCF User Guides](https://argonne-lcf.github.io/user-guides/polaris/data-science-workflows/frameworks/pytorch/)
	- [<i class="fas fa-book fa-1x" alt="`fas:Book`"/> Tensorflow - ALCF User Guides](https://argonne-lcf.github.io/user-guides/polaris/data-science-workflows/frameworks/tensorflow/)
	- [<i class="fas fa-book fa-1x" alt="`fas:Book`"/> DeepSped - ALCF User Guides](https://argonne-lcf.github.io/user-guides/polaris/data-science-workflows/frameworks/deepspeed/)

---

<!-- .slide template="[[left-border]]" style="text-align:left!important;" -->

# Debugging Tools
- [<i class="fas fa-book fa-1x" alt="`fas:Book`"/> CUDA-GDB - ALCF User Guides](https://argonne-lcf.github.io/user-guides/polaris/debugging-tools/CUDA-GDB/)
- `CUDA-GDB` is the Nvidia tool for debugging CUDA applications running on Polaris.
- `CUDA-GDB` is an extension to `GDB`, the GNU Project debugger.
	- The tool provides developers with a mechanism for debugging CUDA applications running on actual hardware.
	- This enables developers to debug applications without the potential variations introduced by simulation and emulation environments

---
<!-- .slide style="text-size:70%;border-left:8px solid #1E8BC9 ;height:100%;" -->
# Jupyter Notebooks on Polaris
0. Start an interactive job (`qsub -I ...`)
1. Load `base` conda environment (as a starting point):
  ```shell
  $ module load conda
  $ conda activate base
  ```

2. Install Jupyter kernel and launch notebook
  ```shell
  $ python3 -m pip install ipykernel
  $ python3 -m ipykernel install --name="base-venv" 
  $ jupyter notebook --port=8899 --no-browser > /tmp/jlab8899.log 2>&1 &
  $ hostname
  x3002c0s31b0n0
  ```


---
## Port Forwarding

![](https://raw.githubusercontent.com/saforem2/ATPESC-StatisticalLearning/main/docs/assets/port-forwarding.svg) <!-- .element align="center" -->

---
<!-- .slide style="text-align:left;text-size:60%;" -->

### Port Forwarding

6. Connect `localhost` to compute node running Jupyter.

7. Starting from your <span style="color:#00CCFF">**local machine**</span>
  ```bash
  # 1: localhost <--> polaris-login-01
  ssh -L localhost:8899:localhost:8899 <username>@polaris.alcf.anl.gov
  # 2: polaris-login-01 <--> compute node
  ssh -L localhost:8899:localhost:8899 <username>@x3002c0s31b0n0
  ```
8. From a web browser on your local machine, navigate to:
  [`https://localhost:8899/`](https://localhost:8899)

<div style="font-size:0.75em;" align="center">

> [!warning] Warning!
> Only **one** port (`8899` in this example) can be used at a time.
> Because of this, if someone is already using the port you try and specify, your connection **WILL NOT WORK**
> To remedy this, (randomly?) choose a different port (e.g. `8891`, `8873`, etc.)

</div>

---
<!-- .slide style="text-align:left;" -->

# <span style="border-bottom:8px solid #00CCFF;"> Linear Regression</span>

---


<style>

:root {
    --r-heading-text-transform: none;
    --r-heading-font: 'Inter', 'Arial', "OpenSans-Bold", "Open Sans", Helvetica, Impact, sans-serif;
    --r-main-background-color: #1D1651;
    --r-main-font: 'Inter', "Arial", "Open Sans", "Coming Soon", "SourceSansPro", Helvetica Neue, sans-serif;
    --r-heading-letter-spacing: -0.45px;
    --r-heading-word-spacing: 0.5px;
    --r-heading-text-transform: none;
    --r-heading-text-shadow: none;
    --r-heading-font-weight: 700;
    --r-heading1-text-shadow: none;
    --r-main-font-size: 22px;
	--r-main-line-height: 1.5em;
    --r-monospace-font-size: 18px;
    --r-heading1-size: 1.33em;
    --r-heading2-size: 1.25em;
    --r-heading3-size: 1.2em;
    --r-heading4-size: 1.15em;
    --r-heading5-size: 1.05em;
    --r-heading6-size: 1.025em;
	--r-heading-line-height:1em;
    --r-code-font: 'JuliaMono', 'Courier New', "agave Nerd Font", monospace;
    --r-link-color: #B8E2DE;
    --r-link-color-dark: #f92672;
    --r-link-color-hover: #63ff51;
    --r-controls-color: #228BE6;
    --r-progress-color: #404040;
    --r-selection-background-color: rgba(255, 255, 0, 0.15);
    --r-selection-color: rgb(255, 255, 0);
    --r-main-color: #c8c8c8;
    --text-muted: #757575;
    --text-faint: #404040;
    --r-heading-color: #FFF;
    --r-background-color: #fff;
    -webkit-font-smoothing:subpixel-antialiased;
}

.reveal pre {
  display: block;
  margin: auto;
  text-align: justify;
  width:90%;
  font-family: var(--r-code-font);
  font-size: var(--r-monospace-font-size);
  min-width:90%!important;
  word-wrap: break-word;
  padding: auto;
  box-shadow: 0 5px px rgba(0, 0, 0, 0.5);
  white-space: pre-wrap;
}

.reveal p {
	margin:auto!important;
	padding:auto!important;
}

.reveal pre code {
	display: inline-block;
    top: 0;
    bottom: 0;
	margin:auto;
	padding:auto;
    font-size: 0.9em;
    background:#181344;
    color: #8B80B8;
	text-align: justify;
    letter-spacing: -0.45px!important;
    word-spacing: -0.5px!important;
}

.reveal {
    font-family: var(--r-main-font), sans-serif;
    font-size: var(--r-main-font-size);
    font-weight: normal;
    color: var(--r-main-color);
    background-color: var(--r-main-background-color);
}

.code {
	font-size:0.9em!important;
}

.reveal h1,
.reveal h2,
.reveal h3,
.reveal h4 {
    margin: var(--r-heading-margin);
    margin-left:4%;
	padding-top: 1%;
    color: #E0E0E0;
    font-family: var(--r-heading-font);
    line-height: var(--r-heading-line-height);
    word-spacing: var(--r-heading-word-spacing);
    text-transform: var(--r-heading-text-transform);
	text-align:left!important;
    text-shadow: var(--r-heading-text-shadow);
    word-wrap: break-word;
}

.reveal h1 {
    font-weight: 700;
    font-size: var(--r-heading1-size);
}

.reveal h2 {
    font-weight: 600;
    font-size: var(--r-heading2-size);
}

.reveal h3 {
    font-weight: 500;
    font-size: var(--r-heading3-size);
}

.reveal h4 {
    font-weight: 500;
    font-size: var(--r-heading4-size);
}

.reveal h5 {
    font-weight: 500;
    font-size: var(--r-heading5-size);
}
.reveal h6 {
    font-weight: 400;
    font-size: var(--r-heading6-size);
}

.reveal ul, ol {
	text-align:left;
}

.reveal ul ul,
.reveal ul ol,
.reveal ol ol,
.reveal ol ul {
  text-align:left;
}

.reveal ul ul {
    list-style: circle;
}
.container {
  position: relative;
}

.make-it-pop {
  filter: drop-shadow(0 0 10px purple);
}

.bottomright {
  position: absolute;
  bottom: 8px;
  right: 16px;
  font-size: 18px;
}

@media (max-width: 95%) {
  section {
    -webkit-flex-direction: column;
    flex-direction: column;
  }
}

.row {
  display: flex;
}

.column {
  flex: 50%;
}


#left {
  margin: 0 0 5px 5px;
  text-align: left;
  float: left;
  z-index: -10;
  width: 48%;
  font-size: 0.85em;
}

#right {
  margin: 0 0 5px 0;
  float: right;
  max-width: 48%;
  text-align: left;
  z-index: -10;
  width: 48%;
  font-size: 0.85em;
}

#darkBack {
    background-color: #1D1651;
    color: #efefef;
    .reveal a {
        color: #F92672;
        transition: color 0.15s ease;
    }
    .reveal a:hover {
        color: var(--r-link-color-hover);
    }
}

.multiCol {
    display: table;
    table-layout: fixed; /* don't fudge depending on content */
    width: 100%;
    text-align: left; /* matter of taste, makes imho sense */
    .col {
        display: table-cell;
        vertical-align: top;
        width: 50%;
        padding: 2% 0 2% 3%; /* some vertical, and between columns */
        &:first-of-type { padding-left: 0; } /* there's nothing before col1 */
    }
}

.reveal blockquote:before {
  content: "❝";
  font-size: 2.5em;
  margin-right: .05em;
  line-height: 0.1em;
  vertical-align: -0.3em;
  text-align: left;
  color: var(--text-faint);
  font-style: normal !important;
}

.reveal blockquote p {
  color: var(--text-muted);
  font-style: normal !important;
  font-align: left;
  display: inline;
  text-align: left;
}

.reveal blockquote em{
  color: var(--text-muted);
  text-align: left;
}

.reveal blockquote {
  border-radius: 8px !important;
  margin: 0.5rem 0rem 0.5rem 0rem;
  text-align: left;
  padding-top: 1rem;
  padding-left: 2rem;
  padding-bottom: 1rem;
  padding-right: 2rem;
  width: auto;
  font-style: normal !important;
}

.hljs {
    background: #201646 !important;
    border-radius: 8px !important;
}

.hljs-built_in {
  color: #EC6358;
}

#blue {
    color: #00CCFF;
}
#bright {
    color: #00A2FF;
}
#cyan {
    color: #00CCFF;
}
#purple {
    color: #AE81ff;
}
#green {
    color: #63FF51;
}
#yellow {
    color: #FD971F;
}
#lightpink {
    color: #E64980;
}
#pink {
    color: #F92672;
}
#red {
    color: #FA5252;
}
#grey {
    color: #666666;
}

#noteinverse {
    background-color: #1D1651;
    border-radius: 5px;
    border-color: #1D1651;
    color: #efefef;
    padding: auto;
    margin: auto;
}

#note {
    background-color: rgba(255, 255, 255, 0.1);
    padding: 10px;
    border-radius: 8px;
    border-color: rgba(240, 240, 240, 1.0);
    color: rgb(255, 255, 255);
    margin: auto;
}

#mark {
    margin: auto;
    padding: auto;
    background-color: #FD971F;
    color: #1D1651;
    font-weight: 700;
    border-radius: 7px;
}

#dark {
    background-color: #1D1651;
    color: #efefef;
    --r-link-color: #00CCFF!important;
    --r-header-color: #666666;
    -webkit-font-smoothing:subpixel-antialiased;
}

#halfsize {
    font-size: 0.5em;
}

#large {
  font-size: 1.25em;
}

.horizontal_dotted_line{
  border-bottom: 2px dotted gray;
}

.footer {
  font-size: 60%;
  vertical-align:bottom;
  color:#bdbdbd;
  font-weight:400;
  margin-left:-5px;
}
.note {
  color:#f8f8f8;
  border-radius:8px;
  background-color:#35353540;
  border-color:#66666640;
  padding: auto;
  margin:auto;
}

.callout {
  border-left: 6px solid rgb(var(--callout-color));
  background-color: #181242;
  opacity:95%;
  text-align:left;
  margin-left: 4%;
  border-radius:2px;
  page-break-inside: avoid;
}

.callout > ul,ol {
	line-height:1em!important;
}

.callout-title {
  display: flex;
  text-align:left;
  color: #f8f8f8;
  padding-left:10px;
  gap: 10px;
  font-size:1em;
  top: 0;
  position:relative;
  margin-top:0px!important;
  margin-bottom:0em!important;
  padding-top:0px!important;
  padding-bottom:0px!important;
  border-radius:2px;
  background-color: rgba(var(--callout-color), 0.3);
}

.callout-content {
	padding: 1px!important;
	top: 0;
	margin-left: 1%;
    padding-bottom:0.75em;
	padding:auto;
    line-height:1em!important;
}

.reveal .code-wrapper code {
	width: 98%;
}

.reveal code {
  font-family: var(--r-code-font);
  text-transform: none;
  tab-size: 4;
  padding:1.5px;
  background:#181344;
  color: #8EC5E4;
  border-radius:5px;
  letter-spacing: -0.45px!important;
  word-spacing: -0.5px!important;
}

#customcontrols > ul {
  display: none!important;
}

#customcontrols button {
  display: none!important;
}

.reveal .slides > section.present, .reveal .slides > section > section.present {
  min-height: 100% !important;
  display: flex !important;
  flex-direction: column !important;
  justify-content: center !important;
  position: absolute !important;
  top: 0 !important;
}
section > h1 {
  position: absolute !important;
  top: 0 !important;
  margin-left: auto !important;
  margin-right: auto !important;
  left: 0 !important;
  right: 0 !important;
}

.print-pdf .reveal .slides > section.present, .print-pdf .reveal .slides > section > section.present {
  min-height: 770px !important;
  position: relative !important;
}

.hljs-comment, .hljs-quote, .hljs-deletion, .hljs-meta {
	color: #5E5481;
}

.strong {
	color: #F92672!important;
	font-weight: 800;
}

.ol, .ul {
	margin-left: 4%;
	line-height:1.3em;
}

</style>