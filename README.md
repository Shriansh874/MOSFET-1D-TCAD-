# 1‑D MOSFET Simulator — Physics & Colab

This repo accompanies a **Google Colab notebook** that numerically solves the fundamental 1‑D MOSFET equations and lets you explore them with IPyWidgets sliders.

---

## 1 Equations Implemented 

*(all variables in cgs units; default T = 300 K)*

### 1.1 Poisson (electrostatics)

<img src="https://latex.codecogs.com/png.latex?\frac{d}{dx}\Bigl(\varepsilon\,\frac{d\varphi}{dx}\Bigr)%20=%20-q\,(p-n+N_D%5E%2B%20-%20N_A%5E%2D)" alt="Poisson equation" />

Dirichlet contacts: φ = 0 V at ohmic source & drain.  
Gate bias V<sub>g</sub> is applied as a uniform shift Δφ = V<sub>g</sub> inside the channel region.

### 1.2 Carrier statistics (equilibrium)

<img src="https://latex.codecogs.com/png.latex?n%20=%20n_i%20e%5E%7B\frac{q\varphi}{kT}\7D,%20\quad%20p%20=%20n_i%20e%5E%7B-\frac{q\varphi}{kT}\7D" alt="Carrier statistics" />

Evaluated every Newton step so φ and charge remain self‑consistent.

### 1.3 Mobility

1. **Concentration dependence (Caughey–Thomas)**  
   <img src="https://latex.codecogs.com/png.latex?\mu_{\text{lat}}(N)%20=%20\mu_{\min}%20+%20\frac{\mu_{\max}-\mu_{\min}}{1+\left(\frac{N}{N_{\text{ref}}}\right)^\alpha}" alt="Caughey–Thomas mobility" />

2. **High‑field velocity saturation (Kroemer)**  
   <img src="https://latex.codecogs.com/png.latex?\mu(E)%20=%20\frac{\mu_{\text{lat}}}{1+\mu_{\text{lat}}|E|/v_{\text{sat}}},\quad%20E=-\frac{d\varphi}{dx}" alt="Velocity saturation" />

### 1.4 Current densities (drift–diffusion)

<img src="https://latex.codecogs.com/png.latex?J_n%20=%20q\mu_n\,n\,E%20-%20qD_n\,\frac{dn}{dx},\quad%20J_p%20=%20q\mu_p\,p\,E%20+%20qD_p\,\frac{dp}{dx}" alt="Current densities" />

### 1.5 Generation & Recombination

* **Shockley–Read–Hall**  
  <img src="https://latex.codecogs.com/png.latex?R_{\text{SRH}}%20=%20\frac{n\,p-n_i^2}{\tau_p(n+n_i)\;+\;\tau_n(p+n_i)}" alt="SRH recombination" />

* **Auger**  
  <img src="https://latex.codecogs.com/png.latex?R_{\text{Auger}}%20=%20\bigl(C_n\,n%20+%20C_p\,p\bigr)\,(n\,p-n_i^2)" alt="Auger recombination" />

* **Impact ionisation (Selberherr)**  
  <img src="https://latex.codecogs.com/png.latex?\alpha(E)%20=%20\alpha_0\,\exp\Bigl[-\bigl(E_{\text{crit}}/E\bigr)^\beta\Bigr],\quad%20G_{\text{II}}%20=%20\alpha(E)\,\frac{|J_n|+|J_p|}{q}" alt="Impact ionisation" />

The GUI plots the magnitude  
<img src="https://latex.codecogs.com/png.latex?|G_{\text{II}}-R_{\text{SRH}}-R_{\text{Auger}}|\;(\mathrm{cm}^{-3}\,\mathrm{s}^{-1})" alt="Net G–R rate" />  
in log‑scale, revealing where avalanche could start.

> **Quantum stub** — `quantum.py` can be swapped with a Schrödinger–Poisson routine returning an extra charge term ρ<sub>Q</sub>(φ).

---
