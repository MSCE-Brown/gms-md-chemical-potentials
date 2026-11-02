# GMS MD-informed parametrization for excess chemical potentials and activity coefficients

This repository contains data and analysis scripts for the manuscript:

**A generalized multicomponent solution model bridging molecular solvation structure and activity coefficients, with applications to electrical double-layer dynamics**

The scripts convert molecular dynamics (MD)-derived radial distribution functions (RDFs) into potentials of mean force (PMFs), coordination numbers (CNs), and generalized multicomponent solution (GMS) parameters used to compute excess chemical potentials and activity coefficients.

## Repository contents

```text
gms-md-chemical-potentials/
  data/
    LiPF6_EC_EMC/
      comsol_results/
      rdf/                         # LPF1M_EC-EMC_rdf.csv
      processed/                   # Parameterization outputs
    TMABF4_EtOH_THF_MTC/
      comsol_results/
      rdf/                         # bulk_16MTC6ion_RDF.csv
      processed/                   # Parameterization outputs
    LiFSI_single_solvent/
      RDF_all_pairs/
        <system>/                  # Paired CMD-rdf_*.xvg and CMD-cn_*.xvg files
      reduction_potential_exp_Li.txt
    figure_data/                   # Numerical data underlying manuscript figures
  notebooks/
    LiPF6_EC_EMC_parameterization.ipynb
    TMABF4_EtOH_THF_MTC_parameterization.ipynb
    LiFSI_single_solvent_GMS.ipynb
  results/
    LiFSI_single_solvent/           # Created when optional exports are enabled
  requirements.txt
  environment.yml
  LICENSE
  CITATION.cff
```

## GMS parameters

**The definitions, parameterization settings, and numerical values of the affinity parameters $a_{ij}$ are provided in the notebooks.** Each system's notebook contains the relevant species ordering, pair definitions, structural analysis, and final parameter matrices. Consult these cells for the parameters used in that system.

The principal model inputs are:

| Parameter | Meaning |
| --- | --- |
| $X_i$ | Bulk mole fraction of species $i$ |
| $a_{ij}$ | Dimensionless affinity governing local neighbor selection |
| $\Delta\epsilon_{ij}$ | Effective pair interaction free-energy parameter, related to the affinity by $a_{ij}=\exp[-\Delta\epsilon_{ij}/(RT)]$ in molar units |
| $Z_i$ | Effective coordination number for central species $i$ |

## Input data

### LiPF6/EC/EMC and TMABF4/EtOH/THF/MTC

The RDF input is a CSV file containing:

- a column named `r`, with distances in angstroms;
- one RDF column per pair, named `SpeciesA-SpeciesB`, for example `Li+-O(c_EC)` or `N(TMA)-B(BF4)`.

The pair lookup checks both `A-B` and `B-A` column names. Species definitions and system-specific analysis settings are provided in the corresponding notebooks.

### LiFSI in single solvents

The bulk LiFSI notebook reads paired GROMACS XVG files using:

```python
DATA_GLOB = "../data/LiFSI_single_solvent/RDF_all_pairs/*/*.xvg"
EXPERIMENT_FILE = Path("../data/LiFSI_single_solvent/reduction_potential_exp_Li.txt")
```

Each `CMD-rdf_*.xvg` file is paired with the corresponding `CMD-cn_*.xvg` file in the same directory. Files contain two numeric columns: radius in **nm**, followed by the RDF or cumulative CN. The notebook converts radii to angstroms and ignores GROMACS metadata lines beginning with `#` or `@`.

Each system requires six pair types: Li–FSI, Li–solvent, Li–Li, FSI–solvent, FSI–FSI, and solvent–solvent. Filenames encode the solvent and Li:solvent ratio; an example is:

```text
CMD-rdf_Li_FSI_case1-Li-FSI-DME_example-1-4.0.xvg
CMD-cn_Li_FSI_case1-Li-FSI-DME_example-1-4.0.xvg
```

The experimental file is whitespace-delimited. Its first column is the solvent name, the second is the solvent/Li ratio, and the last is the CV-average potential in volts. At least four columns are expected; lines beginning with `#` are treated as comments.

## Quick start

1. Add RDF CSV files into the corresponding `data/*/rdf/` directories.
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Start Jupyter with `notebooks/` as the working directory, open the relevant notebook, and run its cells in order. Review the configuration and RDF/CN diagnostic plots before interpreting the thermodynamic results.

### Multicomponent parameterization notebooks

The LiPF6/EC/EMC and TMABF4/EtOH/THF/MTC workflows provide:

- `rdf_shell_analysis.csv`: selected RDF peaks, PMF minima, shell cutoffs, and CNs;
- `COMSOL_GMS_Parameters.txt`: model parameters for use in the COMSOL transport model;
- displayed affinity matrices $a_{ij}$, effective pair interaction parameters $\Delta\epsilon_{ij}$, and coordination parameters $Z_i$.

### Bulk LiFSI notebook

`LiFSI_single_solvent_GMS.ipynb` uses **DME, EC, FEC, and PC** by default. It includes:

- automatic peak selection within configured windows and first-minimum CN cutoffs, with explicit manual overrides;
- six-panel RDF/CN visual checks for each solvent;
- full mixing and excess chemical potentials for Li+, FSI−, and solvent;
- frozen-parameter extrapolation checks for all three species;
- a four-panel comparison of relative Li+ chemical potentials with experimental CV-average potential shifts;
- matching model-only panels for FSI− and solvent;
- numerical thermodynamic consistency checks and optional figure/table exports.

The Li+ comparison uses solvent-specific matched reference compositions. For display, EC experimental points outside the simulation range are excluded, and DME model points above the maximum experimental Li mole fraction are excluded. These display filters preserve the existing reference values.

Exports are disabled by default. Set `EXPORT_FIGURES` and/or `EXPORT_TABLES` to `True` to write outputs to `results/LiFSI_single_solvent/`.

## Continuum model setup

The parameterization notebooks provide molecularly informed inputs for the continuum calculations. See the manuscript's Supporting Information for the governing equations, COMSOL setup, and simulation conditions.



## Citation

If you use this repository, please cite the associated manuscript.
