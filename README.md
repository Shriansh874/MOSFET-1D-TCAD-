# 1‑D MOSFET Simulator — Physics & Colab

A **single Google Colab notebook** that solves the key equations of a long‑channel MOSFET slice and lets you explore the results with sliders. 

---

## Governing Equations 

All variables use cgs units.  Temperature defaults to 300 K.

### 1.1 Poisson (Electrostatics)

$$
\frac{d}{dx}\!\Bigl(\varepsilon\,\frac{d\varphi}{dx}\Bigr)\;=\;-q\,[p\,-\,n\,+\,N_D^{+}\!-\!N_A^{-}]\qquad(1)
$$

* **Contacts:** Dirichlet φ = 0 V at source & drain.
* **Gate bias:** a uniform potential shift Δφ = V<sub>g</sub> applied to the channel region (oxide not yet included—simplifies the demo while still revealing inversion physics).

### 1.2 Carrier Statistics (Equilibrium)

$$
 n = n_i\,e^{\;q\varphi/kT}, \qquad p = n_i\,e^{-q\varphi/kT}\qquad(2)
$$

Boltzmann approximation; evaluated each Newton iteration so charge and potential remain self‑consistent.

### 1.3 Mobility

1. **Concentration dependence (Caughey–Thomas)**

   $$
   \mu_{lat}(N)=\mu_{min}+\frac{\mu_{max}-\mu_{min}}{1+(N/N_{ref})^{\alpha}}\qquad(3)
   $$
2. **High‑field velocity saturation (Kroemer)**

   $$
   \mu(E)=\frac{\mu_{lat}}{1+\mu_{lat}|E|/v_{sat}}\qquad(4)
   $$

The two effects combine to imitate bulk mobility roll‑off and velocity saturation without solving energy‑balance equations.

### 1.4 Generation & Recombination

* **Shockley–Read–Hall**

  $$
  R_{\mathrm{SRH}}=\frac{np-n_i^2}{\tau_p(n+n_i)+\tau_n(p+n_i)}\qquad(5)
  $$
* **Auger**

  $$
  R_{\mathrm{Auger}}=(C_n n + C_p p)(np-n_i^2)\qquad(6)
  $$
* **Impact Ionisation (Selberherr)**

  $$
  \alpha(E)=\alpha_0 e^{-(E_{crit}/E)^{\beta}},\quad G_{\mathrm{II}}=\alpha(E)\,\frac{|J_n|+|J_p|}{q}\qquad(7)
  $$

The GUI plots $|G_{\mathrm{II}}-R_{\mathrm{SRH}}-R_{\mathrm{Auger}}|$ so you can see where avalanche might begin.

> **Note:** `quantum.py` is a stub you can replace with a Schrödinger–Poisson charge routine.  Plug its output into Eq‑(1) for sub‑10‑nm channels.

---

