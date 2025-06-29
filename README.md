**MOSFET‑1D TCAD Playground**

A lightweight, self‑contained Python/Colab project that solves 1‑D Poisson and drift–diffusion equations for a long‑channel MOSFET slice, then visualises potential, carrier densities, field, mobility and generation/recombination rates with an interactive IPyWidgets GUI.

---

**Table of Contents**

1. [Features](#features)
2. [Project Structure](#structure)
3. [Quick Start – Google Colab](#colab)
4. [Running Locally](#local)
5. [How It Works](#physics)
6. [Extending the Simulator](#extend)
7. [Contributing](#contributing)
8. [License](#license)

<a id="features"></a>

### **Features**

* Pure‑Python (NumPy + SciPy) – no proprietary TCAD software required.
* **Interactive GUI** with sliders for gate bias, doping, device length and temperature. Plots update in real‑time.
* Semiconductor physics already included:

  * Poisson + drift–diffusion under Boltzmann statistics.
  * Concentration‑ & field‑dependent electron/hole mobility (Caughey‑Thomas + velocity saturation).
  * SRH & Auger recombination.
  * Impact‑ionisation generation (Selberherr model).
* Modular codebase – each physical effect lives in its own module for easy replacement or extension (e.g. Schrödinger–Poisson, hydrodynamic transport).
* Runs out‑of‑the‑box in **Google Colab** *(free GPU/CPU back‑end, no local installs)* or any Python ≥ 3.9 environment.

<a id="structure"></a>

### **Project Structure**

```
.
├── mosfet_gui.py           # launches the IPyWidgets GUI
├── mesh.py                 # 1‑D grid generator
├── poisson.py              # nonlinear Poisson solver (Newton + sparse FD)
├── drift_diffusion.py      # carrier post‑processing, mobility, G‑R
├── quantum.py              # placeholder for Schrödinger–Poisson (optional)
├── utils/
│   └── constants.py        # physical constants
└── models/
    ├── mobility.py         # μ(N,E) models
    ├── recombination.py    # SRH & Auger
    └── impact_ion.py       # impact‑ionisation α(E)
```

The repository also contains **`MOSFET_1D_GUI.ipynb`**, a Colab‑ready notebook that writes these files into the Colab filesystem and launches the GUI.

<a id="colab"></a>

### **Quick Start – Google Colab**

1. Open the notebook in Colab: [https://colab.research.google.com/github/your‑username/your‑repo/blob/main/MOSFET\_1D\_GUI.ipynb](https://colab.research.google.com/github/your‑username/your‑repo/blob/main/MOSFET_1D_GUI.ipynb)
2. Run **all cells** ⟶ Colab will:

   * install `ipywidgets` and enable the widget manager,
   * write each `%%writefile` cell to disk (creating the project modules),
   * import `mosfet_gui`, which pops up the slider panel.
3. Drag the sliders – observe how φ(x), carrier densities and other curves react to your parameter choices.

> **Tip:** Colab may prompt you to “Restart runtime after pip install”. Accept, then re‑run the notebook cells.

<a id="local"></a>

### **Running Locally**

```bash
# 1. Clone repository & enter
$ git clone https://github.com/your‑username/mosfet‑1d‑tcad.git
$ cd mosfet‑1d‑tcad

# 2. Create virtual environment (optional but recommended)
$ python3 -m venv venv && source venv/bin/activate

# 3. Install requirements
$ pip install -r requirements.txt

# 4. Launch Jupyter
$ jupyter notebook MOSFET_1D_GUI.ipynb
```

*Requirements:* Python 3.9+, NumPy ≥ 1.24, SciPy ≥ 1.11, Matplotlib, IPyWidgets.

<a id="physics"></a>

### **How It Works – Physics Pipeline**

1. **Mesh Generation** – uniform grid (default 1 µm / 601 nodes) via `mesh.py`.
2. **Doping Profile** – `mosfet_gui.py` builds an n+|p‑body|n+ slice.
3. **Poisson** – `poisson.py` solves ε∇·∇φ = ‑q(p–n+Ndop) with Newton iteration.
4. **Carrier Post‑Processing** – `drift_diffusion.py` computes:

   * Boltzmann n(x), p(x)
   * E = ‑∂φ/∂x
   * μₙ/μₚ(E,N) via `models/mobility.py`
   * R\_SRH, R\_Auger, G\_impact
5. **Plotting** – GUI shows φ & E, carrier/doping log‑scale, μ profiles & |G‑R|.

<a id="extend"></a>

### **Extending the Simulator**

| Extension                             | Where to start                                                                             |
| ------------------------------------- | ------------------------------------------------------------------------------------------ |
| Schrödinger–Poisson quantum inversion | Fill in `quantum.py`; add quantum charge to Poisson residual.                              |
| 2‑D short‑channel effects             | Swap FD for an FEM library (FEniCSx, scikit‑fem).                                          |
| Hydrodynamic / ballistic transport    | Replace Boltzmann drift‑diffusion in `drift_diffusion.py` with HD equations or BTE solver. |
| Thermal self‑heating                  | Add heat‑diffusion PDE & couple temperature‑dependent μ, α.                                |

<a id="contributing"></a>

### **Contributing**

Pull requests are welcome! Please open an issue for major feature discussions.  Follow PEP‑8, add docstrings & at least one minimal test in `tests/`.

<a id="license"></a>

### **License**

This project is released under the **MIT License** – see `LICENSE` for details.
