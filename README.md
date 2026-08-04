# GMS MD-informed parametrization for excess chemical potentials and activity coefficients

MD-derived generalized multicomponent solution (GMS) model parameterization for molecularly informed excess chemical potentials and activity coefficients in multicomponent liquid mixtures. 

This repository contains data and analysis scripts for the manuscript:

**A generalized multicomponent solution model bridging molecular solvation structure and activity coefficients, with applications to electrical double-layer dynamics**

The scripts convert molecular dynamics (MD)-derived radial distribution functions (RDFs) into potentials of mean force (PMFs), coordination numbers (CNs), and generalized multicomponent solution (GMS) parameters used to compute excess chemical potentials and activity coefficients.

## Repository contents

```text
gms-md-chemical-potentials/
  data/
    LiPF6_EC_EMC/
      rdf/                       # Add LPF1M_EC-EMC_rdf.csv here
      processed/                 # Script outputs are written here
    TMABF4_EtOH_THF_MTC/
      rdf/                       # Add bulk_16MTC6ion_RDF.csv here
      processed/                 # Script outputs are written here
    figure_data/                 # Add numerical data behind manuscript figures
  notebooks/
    LiPF6_EC_EMC_parameterization.ipynb
    TMABF4_EtOH_THF_MTC_parameterization.ipynb
  requirements.txt
  environment.yml
  LICENSE
  CITATION.cff
```

## Input RDF format

The main input file should be a CSV file with:

- one column named `r` containing distances in angstroms;
- one RDF column for each pair, named as `SpeciesA-SpeciesB`, for example `Li+-O(c_EC)` or `N(TMA)-B(BF4)`.

The code checks both `A-B` and `B-A` column names when looking up pair RDFs.

## Quick start

1. Add RDF CSV files into the corresponding `data/*/rdf/` directories.
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Run the parameterization in the Jupyter notebooks. The outputs include:

- `rdf_shell_analysis.csv`: RDF peak, PMF minimum, cutoff, and CN for each center-neighbor pair;
- `gms_parameters.csv`: final GMS parameters for each pair;
- `affinity_matrix.csv`: symmetric affinity matrix `a_ij`;
- `delta_epsilon_kjmol.csv`: effective pair interaction free energies in kJ/mol;
- `coordination_size_Z.csv`: effective coordination size parameters.


## Citation

If you use this repository, please cite the associated manuscript.
