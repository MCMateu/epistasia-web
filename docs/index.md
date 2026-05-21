# Epistasia: a library for analyzing binary landscapes

<p align="center">
  <img src="figures/LandscapeDefSi.svg" width="850" alt="Landscape schematic">
</p>

## What is a landscape?

In ecology and genetics, we often deal with systems composed of several interacting components — such as species in a community, genes in a genome, or mutations in a genotype. Each component can be **present (1)** or **absent (0)**, so the set of all possible configurations can be represented as

\begin{equation}
\mathbb{F}_2^N = \{0,1\}^N,
\end{equation}

where $N$ is the number of components, and the space contains $2^N$ possible combinations.

A **landscape** is a mathematical function

\begin{equation}
F : \mathbb{F}_2^N \rightarrow \mathbb{R},
\end{equation}

that assigns a quantitative property to each configuration — for example, total biomass, growth rate, ecosystem productivity, or fitness.  In biological terms, it tells us *how the system performs depending on which components are active*.

In practice, a landscape dataset consists of a collection of measured pairs $(\mathbf{x}, F)$,   where each vector $\mathbf{x} = (x_1, x_2, \ldots, x_N)$ indicates the presence (1) or absence (0) of each component,  and $F(\mathbf{x})$ is the measured outcome. Many experiments also include several replicates $R$ for the same configuration, to account for experimental variability.

Such datasets form a **discrete, high-dimensional map** linking the composition of the system  (e.g., which species or genes are present) to an emergent property at the system level. By analyzing these landscapes, we can uncover **interactions**, **nonlinear effects**,  and **higher-order dependencies** that shape the collective behavior of complex biological systems.

## What does Epistasia do?

**Epistasia** is a Python toolkit to analyze **binary landscapes** (genotype–phenotype or community–function maps) and quantify **interactions across orders**, from additive effects to higher-order terms.

Epistasia is designed around three core questions:

1. Which **local, context-dependent interactions** can be detected given experimental noise?
2. Which **background-averaged interactions** remain statistically identifiable across orders?
3. Which interaction orders **matter functionally**, as quantified by the variance spectrum?

The library implements noise-aware estimators, bootstrap null models, and variance decompositions to address these questions in empirical, synthetic, and mechanistic landscapes.

## Download epistasia

- From GitHub:

```bash
pip install "epistasia @ git+https://github.com/MCMateu/epistasia-1.0.git"
```

- From Docker:

Docker provides a ready-to-use environment with Epistasia and JupyterLab installed. This option is useful if you want to run analyses without manually configuring a Python environment.

First, install Docker Desktop:

- Mac / Windows: https://www.docker.com/products/docker-desktop
- Linux: https://docs.docker.com/engine/install/

Then create a working folder for your analyses:

```bash
mkdir -p my-analysis/data my-analysis/notebooks
cd my-analysis
```

Use the following structure:

```text
my-analysis/
├── data/        # place your CSV files here
└── notebooks/   # your Jupyter notebooks will be saved here
```

Download the Docker Compose configuration file:

```bash
curl -L https://raw.githubusercontent.com/MCMateu/epistasia-1.0/main/docker-compose.user.yml -o docker-compose.user.yml
```

Start Epistasia:

```bash
docker compose -f docker-compose.user.yml up
```

The first run will automatically download the Docker image. Later runs should start much faster.

Open JupyterLab in your browser at:

```text
http://localhost:8888
```

JupyterLab may ask for a token the first time you open it. The token is printed in the terminal where Docker is running. Look for a line similar to:

```text
http://127.0.0.1:8888/lab?token=...
```

Copy the value after `token=` and paste it into the JupyterLab login page. To stop the program, press `Ctrl+C` in the terminal


Inside JupyterLab, files placed in `my-analysis/data/` are available under `/app/data/`. For example:

```python
import epistasia as ep

L = ep.landscape_from_file("/app/data/my_landscape.csv")
out = ep.walsh_analysis(L)

ep.plot_variance_and_amplitude(out)
ep.plot_walsh_volcano(out)
```

## Quick start

```python
import epistasia as ep

L = ep.landscape_from_file("my_landscape.csv")   # x1..xN, F (and optionally replicate column)
out = ep.walsh_analysis(L)

ep.plot_variance_and_amplitude(out)
ep.plot_walsh_volcano(out)
```
