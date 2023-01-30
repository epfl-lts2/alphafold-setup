This is adapted from [this repo](https://github.com/kalininalab/alphafold_non_docker)

# Setup python environment

- `conda create -n alphafold python=3.8` (rename `alphafold` as needed)
- `conda activate alphafold`
- `conda install -y -c conda-forge openmm==7.5.1 cudnn==8.2.1.32 cudatoolkit==11.1.1 pdbfixer==1.7`
- `conda install -y -c nvidia cuda-nvcc==11.3` (this is needed as jax requires a functional `ptxas` binary)
- `conda install -y -c bioconda hmmer==3.3.2 hhsuite==3.3.0 kalign2==2.04`
- `pip install -r requirements.txt`
