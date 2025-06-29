# 1‑D MOSFET Simulator — Physics & Colab

This repo contains a single **Google Colab notebook** that numerically solves the classical 1‑D MOSFET equations and lets you explore the results with live sliders.  

---

## 1 Equations Implemented 

*(all quantities in cgs units; default temperature = 300 K)*

|  Label                             | Equation                                                                                                                              |      |                         |      |          |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- | ---- | ----------------------- | ---- | -------- |
| **Poisson**                        | $\frac{d}{dx}\left(\varepsilon\,\frac{d\varphi}{dx}\right)\;=\;-q\bigl(p-n+N_D^{+}-N_A^{-}\bigr)$                                     |      |                         |      |          |
| **Boltzmann carriers**             | $n = n_i\,\exp\!\left(\tfrac{q\varphi}{kT}\right),\qquad p = n_i\,\exp\!\left(-\tfrac{q\varphi}{kT}\right)$                           |      |                         |      |          |
| **Electric field**                 | $E = -\frac{d\varphi}{dx}$                                                                                                            |      |                         |      |          |
| **Caughey–Thomas mobility**        | $\mu_{\mathrm{lat}}(N)=\mu_{\min}+\frac{\mu_{\max}-\mu_{\min}}{1+\left(\tfrac{N}{N_{\mathrm{ref}}}\right)^{\!\alpha}}$                |      |                         |      |          |
| **Velocity‑sat correction**        | \$\$\mu(E)=\frac{\mu\_{\mathrm{lat}}}{1+\mu\_{\mathrm{lat}}                                                                           | E    | /v\_{\mathrm{sat}}}\$\$ |      |          |
| **Current (drift–diffusion)**      | $J_n = q\mu_n n E - qD_n\frac{dn}{dx},\qquad J_p = q\mu_p p E + qD_p\frac{dp}{dx}$                                                    |      |                         |      |          |
| **Shockley–Read–Hall**             | $R_{\mathrm{SRH}} = \frac{np-n_i^{2}}{\tau_p(n+n_i)+\tau_n(p+n_i)}$                                                                   |      |                         |      |          |
| **Auger recombination**            | $R_{\mathrm{Auger}} = (C_n n + C_p p)\,(np-n_i^{2})$                                                                                  |      |                         |      |          |
| **Impact‑ionisation (Selberherr)** | $\alpha(E)=\alpha_0\,\exp\!\left[-\left(\tfrac{E_{\mathrm{crit}}}{E}\right)^{\!\beta}\right]$ \$\$G\_{\mathrm{II}} = \alpha(E),\frac{ | J\_n | +                       | J\_p | }{q}\$\$ |
| **Net G–R (plotted)**              | $G_{\mathrm{net}} = G_{\mathrm{II}}-R_{\mathrm{SRH}}-R_{\mathrm{Auger}}$                                                              |      |                         |      |          |

**Boundary conditions**
*Dirichlet* φ = 0 V at ohmic source and drain.  Gate bias V<sub>g</sub> is applied as a uniform shift Δφ = V<sub>g</sub> across the channel region (a stand‑in for an oxide stack so the demo stays compact).

> **Quantum stub:** `quantum.py` is ready to be replaced with a Schrödinger–Poisson routine that supplies an extra charge term $\rho_{Q}(\varphi)$ for sub‑10‑nm channels.

---

