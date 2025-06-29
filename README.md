# 1‑D MOSFET Simulator — Physics & Colab

A **Google Colab notebook** that lets you explore MOSFET device physics by solving the governing PDEs in real time with IPyWidgets sliders.

This README involves:

* **Equations which are in the solver** (rendered in GitHub‑friendly \`math\` blocks).
* **How to run the notebook in Colab**.

---

## 1 Equations Implemented 

*(cgs units; default T = 300 K)*

### 1.1 Electrostatics

**Poisson (silicon regions)**

```math
\varepsilon_{\text{Si}} \frac{d^{2}\varphi}{dx^{2}}
   = -q\bigl( p - n + N_D^{+} - N_A^{-} \bigr)
```

**Laplace (charge‑free regions e.g. gate oxide placeholder)**

```math
\varepsilon_{\text{ox}} \frac{d^{2}\varphi}{dx^{2}} = 0
```

Because the demo collapses the oxide into a boundary condition, Laplace is implicitly satisfied there; listing it clarifies that Poisson reduces to Laplace wherever $\rho = 0$.

*Boundary conditions*

* Dirichlet φ = 0 V at ohmic source & drain contacts.
* Gate bias V<sub>g</sub> enters the silicon channel as a uniform shift Δφ (simplest possible MOS capacitor view).  When you extend to an explicit oxide, solve Laplace in that region and couple interface charge by continuity of displacement field.

### 1.2 Carrier Statistics (Equilibrium)

```math
n = n_i\,\exp\!\left(\frac{q\varphi}{kT}\right),
\qquad
p = n_i\,\exp\!\left(-\frac{q\varphi}{kT}\right)
```

### 1.3 Mobility

**Doping dependence (Caughey–Thomas)**

```math
\mu_{lat}(N) = \mu_{\min}
  + \frac{\mu_{\max}-\mu_{\min}}
         {1 + \bigl( N / N_{\text{ref}} \bigr)^{\alpha}}
```

**High‑field velocity saturation (Kroemer)**

```math
\mu(E) = \frac{\mu_{lat}}
              {1 + \mu_{lat} |E| / v_{sat}},
\qquad E = -\frac{d\varphi}{dx}
```

### 1.4 Current Densities (for G–R calculation)

```math
J_n =  q\mu_n n E \;\; - q D_n \frac{dn}{dx}
\qquad
J_p =  q\mu_p p E \;\; + q D_p \frac{dp}{dx}
```

### 1.5 Generation & Recombination

**Shockley–Read–Hall**

```math
R_{\text{SRH}} = \frac{np - n_i^{2}}
                      {\tau_p (n + n_i) + \tau_n (p + n_i)}
```

**Auger**

```math
R_{\text{Auger}} = (C_n n + C_p p)\,(np - n_i^{2})
```

**Impact Ionisation (Selberherr)**

```math
\alpha(E) = \alpha_0 \exp\!\Bigl[-(E_{crit}/E)^{\beta}\Bigr]
\qquad
G_{\text{II}} = \alpha(E) \, \frac{|J_n| + |J_p|}{q}
```

The GUI plots the log‑magnitude of

```math
|G_{\text{II}}\; -\; R_{\text{SRH}}\; -\; R_{\text{Auger}}|
```

so you can spot potential avalanche zones.

---

## 2 Running the Notebook in Colab 

1. * **V<sub>g</sub>** – bends φ, forms inversion layer.
   * **N<sub>A</sub>, N<sub>D</sub>** – shows mobility roll‑off and G–R shifts.
   * **Temperature** – SRH sign reversal as n<sub>i</sub> rises.
   * **Length L** – observe short‑channel electrostatics.

If Colab prompts *“Restart runtime after pip install”* accept, then click **Run all** again.

Enjoy experimenting, and file an issue if you hit a snag!

