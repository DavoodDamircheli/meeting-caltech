---
theme: gaia-mod
<!--theme: uncover-->
paginate: true
<!--backgroundColor: #fff-->
<!--backgroundImage: url('https://marp.app/assets/hero-background.jpg')-->
math: mathjax
---

<!--_class: lead-->
<!--_paginate: false-->

# **Numerical Simulation of particle beds in 3 dimensions using Peridynamics coupled to the Direct Element Method**


#  **Davood Damircheli**


## **Louisiana State University**


## **Supported by ARO Grant W911NF1910245**
<style>
section {
  font-size: 22px;
}
</style>

$$
% User defined functions
\newcommand{\F}{\mathcal{F}}
\newcommand{\E}{\mathcal{E}}
\newcommand{\K}{\mathbb{K}}
\newcommand{\LL}{\mathcal{L}}
\newcommand{\R}{\mathbb{R}}
\newcommand{\C}{\mathbb{C}}
\newcommand{\N}{\mathbb{N}}
\newcommand{\Z}{\mathbb{Z}}
\newcommand{\br}[1]{\color{red} (#1) \color{black}}
\newcommand{\what}{\bb{??}}
\newcommand{\half}{\frac{1}{2}}
\newcommand{\norm}[1]{\left\lVert#1\right\rVert}
\newcommand{\normiii}[1]{\left\lvert\left\lVert#1\right\rVert\right\lvert}
\newcommand{\abs}[1]{\left\lvert#1\right\rvert}
\newcommand{\jap}[1]{\left\langle #1 \right\rangle}
\newcommand{\inn}[1]{\left\langle #1 \right\rangle}
\newcommand{\indi}[1]{\chi_{\left\{ #1 \right\}}}

%% define bold letters
\newcommand{\cc}{\mathbf{c}}
\newcommand{\ee}{\mathbf{e}}
\newcommand{\ww}{\mathbf{w}}
\newcommand{\xx}{\mathbf{x}}
\newcommand{\bx}{\mathbf{X}}
\newcommand{\yy}{\mathbf{y}}
\newcommand{\pp}{\mathbf{p}}
\newcommand{\qq}{\mathbf{q}}
\newcommand{\rr}{\mathbf{r}}
\newcommand{\uu}{\mathbf{u}}
\newcommand{\bu}{\mathbf{U}}
\newcommand{\UU}{\mathbf{U}}
\newcommand{\ff}{\mathbf{f}}
\newcommand{\FF}{\mathbf{F}}
\newcommand{\bb}{\mathbf{b}}
\newcommand{\Bb}{\mathbf{b}}
\newcommand{\nn}{\mathbf{n}}
\newcommand{\vv}{\mathbf{v}}
\newcommand{\TT}{\mathbf{T}}
\newcommand{\PP}{\mathbf{P}}
\newcommand{\HH}{\mathbf{H}}
\newcommand{\II}{\mathbf{I}}
\newcommand{\CC}{\mathbf{C}}
\newcommand{\QQ}{\mathbf{Q}}
\newcommand{\varepsilonB}{\pmb{\varepsilon}}
\newcommand{\sigmaB}{\pmb{\sigma}}
\newcommand{\xiB}{\pmb{\xi}}
\newcommand{\betaB}{\pmb{\beta}}
\newcommand{\etaB}{\pmb{\eta}}
$$

---

# Joint work with 

# **Robert Lipton**

# **Debdeep Bhattacharya**

## **Now Grinnell College**


## **Supported by ARO Grant W911NF1910245**
---



## 3D Kalthoff Winkler exprement

- Benchmark for dynamic fracture problems.
<center>
<video height="500" controls autoplay loop>
<source src="videos/kalthof.mp4" type="video/mp4">
</video> 
</center>

<!-- _footer: <center><center>-->





---

## 400 particles

- AllocCPUS 80
- Eapsed 02:34:00


<center>
<video height="500" controls autoplay loop>
<source src="videos/400-grains.mp4" type="video/mp4">
</video>
</center>

<!-- _footer: <center><center>-->

---

## 1000 particles
- AllocCPUS 80
- Eapsed 15:23:5


<center>
<video height="500" controls autoplay loop>
<source src="videos/1000-grains.mp4" type="video/mp4">
</video>
</center>

<!-- _footer: <center><center>-->


---

## 2250 particles

- AllocCPUS 80
- Eapsed 23:44:14


<center>
<video height="500" controls autoplay loop>
<source src="videos/2250-grains.mp4" type="video/mp4">
</video>
</center>

<!-- _footer: <center><center>-->



---

## Velocity-Verlet

For $\xx \in D_k$ and $k = 1, 2, \dots, N$

$$
\begin{cases}
\uu(\xx) \leftarrow \uu(\xx) + \Delta t \dot{\uu}(\xx) + \frac{\Delta t^2}{2} \ddot{\uu}(\xx)
\\   
\ddot{\uu}^{\text{old}} := \ddot{\uu}(\xx)
\\
\ddot{\uu}(\xx) \leftarrow \frac{1}{\rho} \left( \ff^{\text{peri}} (\xx) +\ff^{\text{self}} (\xx) +\ff^{\text{nbr}} (\xx) +\ff^{\text{wall}} (\xx) + \Bb(\xx, t) \right)
\\
\dot{\uu}(\xx)  \leftarrow \dot{\uu}(\xx) + \frac{\Delta t}{2}  \left(\ddot{\uu}^{\text{old}} + \ddot{\uu}(\xx)  \right)
\end{cases}
$$

- Parallelization with MPI in C++
- Neighborhood search is costly

---



# Parallelization with MPI in C++

- Nonlocal models are expensive compared to MD simulations: O(nm) computation for each each node (n = #nodes, m = #neighbors)
- Time complexity is typically higher than space complexity

## Parallelization Strategy

- For N particle simulation, we compute at most (N choose 2) contacts
- Each contact computation is distributed to a node

## Possible improvements

- Balancing computational loads across nodes
- Fast particle contact detection (difficult when fracture is enabled)

---


### Thank you



