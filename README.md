This is adapted from [this repo](https://github.com/kalininalab/alphafold_non_docker)

# Setup python environment

- `conda create -n alphafold python=3.8` (rename `alphafold` as needed)
- `conda activate alphafold`
- `conda install -y -c conda-forge openmm==7.5.1 cudnn==8.2.1.32 cudatoolkit==11.1.1 pdbfixer==1.7`
- `conda install -y -c nvidia cuda-nvcc==11.3.58` (this is needed as jax requires a functional `ptxas` binary)
- `conda install -y -c bioconda hmmer==3.3.2 hhsuite==3.3.0 kalign2==2.04`
- `pip install -r requirements.txt`
- `pip install jaxlib==0.3.25+cuda11.cudnn82 -f https://storage.googleapis.com/jax-releases/jax_cuda_releases.html`

The environment is almost ready, *but* a file needs to be patched. Assuming your base conda directory is in `~/miniconda3`, run the following:
- `cd ~/miniconda3/envs/alphafold/lib/python3.8/site-packages`
- `wget https://raw.githubusercontent.com/epfl-lts2/alphafold-setup/master/openmm.patch`
- `patch -p0 < openmm.patch`


# Download files
- `mdkir /mnt/scratch.../workdir` (adapt to your needs)
- `cd /mnt/scratch.../workdir`
- `wget -q https://github.com/deepmind/alphafold/archive/refs/tags/v2.3.1.tar.gz && tar -xzf v2.3.1.tar.gz`
- `cd alphafold-2.3.1`
- `wget -q -P ./alphafold/common/ https://git.scicore.unibas.ch/schwede/openstructure/-/raw/7102c63615b64735c4941278d92b554ec94415f8/modules/mol/alg/src/stereo_chemical_props.txt`
- `wget -q https://raw.githubusercontent.com/epfl-lts2/alphafold-setup/master/run_alphafold.sh`
- `chmod +x run_alphafold.sh`

# Run alphafold
- Create a fasta query, e.g.
```
>pdb|3h7p|A
MQIFVKTLTGKTITLEVEPSDTIENVKAKIQDKEGIPPDQQRLIFAGKQLEDGRTLSDYNIQRESTLHLVLRLRGG
```
- Run alphafold (the necessary databases have been already downladed and processed in `/mnt/scratch/alphafold`):

`./run_alphafold.sh -d /mnt/scratch/alphafold -o test2 -c reduced_dbs -f [fasta input] -t 2020-05-14 -a [gpu id] -n [number of cpus]`

e.g.:

`./run_alphafold.sh -d /mnt/scratch/alphafold -o test2 -c reduced_dbs -f ./ubi.fasta -t 2020-05-14 -a 1 -n 16`

