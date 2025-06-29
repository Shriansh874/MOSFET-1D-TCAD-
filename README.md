# 1‑D MOSFET Simulator — Physics & Colab

This repo accompanies a **Google Colab notebook** that numerically solves the fundamental 1‑D MOSFET equations and lets you explore them with IPyWidgets sliders.



## 1 Equations Implemented 

*(all variables in cgs units; default T = 300 K)*

### 1.1 Poisson (electrostatics)

$$
\frac{d}{dx}\left(\varepsilon\,\frac{d\varphi}{dx}\right) = -q\,(p-n+N_D^{+}-N_A^{-})
$$

Dirichlet contacts: $\varphi=0\,\text{V}$ at ohmic source & drain.
Gate bias $V_{\!g}$ is applied as a uniform shift $\Delta\varphi = V_{\!g}$ inside the channel region (a stand‑in for an oxide stack in this demo).

### 1.2 Carrier statistics (equilibrium)

$$
 n = n_i\,\exp\!\left(\frac{q\varphi}{kT}\right),\qquad p = n_i\,\exp\!\left(-\frac{q\varphi}{kT}\right)
$$

Evaluated every Newton step so charge and potential remain self‑consistent.

### 1.3 Mobility

1. **Concentration dependence (Caughey–Thomas)**

   $$
   \mu_{\text{lat}}(N)=\mu_{\min}+\frac{\mu_{\max}-\mu_{\min}}{1+\left(\dfrac{N}{N_{\text{ref}}}\right)^{\!\alpha}}
   $$
2. **High‑field velocity saturation (Kroemer)**

   $$
   \mu(E)=\frac{\mu_{\text{lat}}}{1+\mu_{\text{lat}}|E|/v_{\text{sat}}}\,,\qquad E=-\frac{d\varphi}{dx}
   $$

### 1.4 Current densities (drift–diffusion, used for G–R visualisation)

$$
J_n = q\mu_n n E - qD_n\,\frac{dn}{dx},\qquad
J_p = q\mu_p p E + qD_p\,\frac{dp}{dx}
$$

### 1.5 Generation & Recombination

* **Shockley–Read–Hall**

  $$
  R_{\text{SRH}} = \frac{np-n_i^{2}}{\tau_p(n+n_i)+\tau_n(p+n_i)}
  $$
* **Auger**

  $$
  R_{\text{Auger}} = \bigl(C_n n + C_p p\bigr)\,(np-n_i^{2})
  $$
* **Impact ionisation (Selberherr)**

  $$
  \alpha(E) = \alpha_0\,\exp\Bigl[-\bigl(E_{\text{crit}}/E\bigr)^{\!\beta}\Bigr],\qquad
  G_{\text{II}} = \alpha(E)\,\frac{|J_n|+|J_p|}{q}
  $$

The GUI plots the magnitude

$$
|G_{\text{II}}-R_{\text{SRH}}-R_{\text{Auger}}|\quad (\text{cm}^{-3}\,\text{s}^{-1})
$$

in log‑scale, revealing where avalanche could start.

> **Quantum stub** — `quantum.py` is ready to be swapped with a Schrödinger–Poisson routine that returns an extra charge term $\rho_Q(\varphi)$ for sub‑10‑nm channels.

---


