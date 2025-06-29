# 1‑D MOSFET Simulator — Physics & Colab

This repository accompanies a **Google Colab notebook** that solves the fundamental 1‑D MOSFET equations and lets you explore their behaviour with IPyWidgets sliders.

Below you’ll find only what matters:

* **The exact equations** (rendered via GitHub’s built‑in \`\`\`math fences).
* **How to run the notebook in Colab.**

---

## 1 Equations Implemented 🧮

*(cgs units; default T = 300 K)*

### 1.1 Poisson (electrostatics)

```math
\frac{d}{dx}\left(\varepsilon\,\frac{d\varphi}{dx}\right) 
  = -q\,(p - n + N_D^{+} - N_A^{-})
```

*Ohmic contacts:* φ = 0 V at source & drain.
*Gate bias:* adds a uniform shift Δφ = V<sub>g</sub> inside the channel region (oxide omitted for brevity).

### 1.2 Carrier statistics (equilibrium Boltzmann)

```math
n = n_i \exp\!\left(\frac{q\varphi}{kT}\right),
\qquad
p = n_i \exp\!\left(-\frac{q\varphi}{kT}\right)
```

(Re‑evaluated every Newton iteration).

### 1.3 Mobility

**Concentration dependence (Caughey–Thomas)**

```math
\mu_{lat}(N) = \mu_{\min} 
 + \frac{\mu_{\max}-\mu_{\min}}{1 + \left( N / N_{\text{ref}} \right)^{\alpha}}
```

**Velocity saturation (Kroemer)**

```math
\mu(E) = \frac{\mu_{lat}}{1 + \mu_{lat} |E| / v_{sat}},
\qquad E = -\frac{d\varphi}{dx}
```

### 1.4 Current densities (for G–R plots)

```math
J_n = q\mu_n n E - q D_n \frac{dn}{dx}
\qquad
J_p = q\mu_p p E + q D_p \frac{dp}{dx}
```

### 1.5 Generation & Recombination

**Shockley–Read–Hall**

```math
R_{\text{SRH}} = \frac{np - n_i^{2}}{\tau_p (n + n_i) + \tau_n (p + n_i)}
```

**Auger**

```math
R_{\text{Auger}} = (C_n n + C_p p)\,(np - n_i^{2})
```

**Impact ionisation (Selberherr)**

```math
\alpha(E) = \alpha_0 \exp\!\Bigl[-(E_{crit}/E)^{\beta}\Bigr]
\qquad
G_{\text{II}} = \alpha(E) \frac{|J_n| + |J_p|}{q}
```

The GUI plots the magnitude `|G_II − R_SRH − R_Auger|` on a log axis, revealing prospective avalanche spots.

> **Quantum stub** – `quantum.py` is a placeholder for Schrödinger–Poisson charge.  Replace it and feed the extra charge into Poisson if you need sub‑10 nm accuracy.

---

## 2 Run in Google Colab 🚀

1. **Open** the notebook (swap in your repo path):

   ```text
   https://colab.research.google.com/github/<USER>/<REPO>/blob/main/MOSFET_1D_GUI.ipynb
   ```
2. Click **Runtime ▸ Run all**. The notebook will

   * install `ipywidgets` (first run only),
   * write each Python module via `%%writefile`,
   * import `mosfet_gui` → launches the slider panel.
3. **Play with the sliders**

   * **Gate bias Vg** – φ(x) bends, inversion appears.
   * **Doping N<sub>A</sub>, N<sub>D</sub>** – mobility drops in heavily doped regions; G–R shifts.
   * **Temperature** – SRH flips sign as n<sub>i</sub> rises.
   * **Length L** – shows classic short‑channel electrostatics.

If Colab prompts *“Restart runtime after pip install”* simply accept, then select **Run all** again.

Enjoy exploring MOSFET physics!  Open an issue if you hit a snag or have ideas.
