# MiDi: Mixed Graph and 3D Denoising Diffusion for Molecule Generation

Clément Vignac, Nagham Osman, Laura Toni, Pascal Frossard


## Installation

- Create a new conda environment for python 3.9
- Install rdkit: `conda create -c conda-forge -n MoleculeDiffusion rdkit python=3.9`
- `conda activate MoleculeDiffusion`
- try the import `from rdkit import Chem`
- `conda install -c "nvidia/label/cuda-11.3.1" cuda-nvcc`     -- the precise version depends on your cuda version
- conda install pytorch 
- try the import `import torch`
  - `pip install wandb`
- `conda install pyg -c pyg`
- try the previous import + `from torch_geometric.data import Data`
- `pip install pytorch-lightning`
- try the previous imports + `import pytorch_lightning as pl`
- `pip install ray ray-lightning`
- try the previous imports + `import ray-lightning`
- `pip install -r requirements.txt`
- `pip install -e .`


## Datasets

  - QM9 should download automatically
  - For GEOM, download the data and put in `MiDi/data/geom/raw/`:
    - train: https://drive.switch.ch/index.php/s/UauSNgSMUPQdZ9v
    - validation: https://drive.switch.ch/index.php/s/YNW5UriYEeVCDnL
    - test: https://drive.switch.ch/index.php/s/GQW9ok7mPInPcIo
  
## Training:

First move inside the `src` folder (so that the outputs are saved at the right location):

Some examples:

QM9 without hydrogens on cpu

``` python3 main.py dataset=qm0 dataset.remove_h=True +experiment=qm9_no_h```

GEOM-DRUGS with hydrogens on 2 gpus

``` python3 main.py dataset=geom dataset.remove_h=False +experiment=geom_with_h general.gpus=2```


## Resuming a previous run

First, retrieve the absolute path of the checkpoint, it looks like
ABS_PATH=`/home/vignac/MiDi/outputs/2023-02-13/18-10-49-geomH/checkpoints/geomH_bigger/epoch=219.ckpt'`

Then run:

``` python3 main.py dataset=qm0 dataset.remove_h=True +experiment=qm9_no_h general.resume='ABS_PATH' ```


## Evaluation

Sampling on multiple gpu is not really handled, we recommand sampling on a single gpu.

Run:

``` python3 main.py dataset=qm0 dataset.remove_h=True +experiment=qm9_no_h general.test_only='ABS_PATH' ```


## Multi-gpu training

Ray-lightning is currently not well maintained, and multi-gpu training might fail. If you get a multi-gpu id error,
replace the file is which the error is raised by the content of `ddp_strategy.py` at the root of this folder

For me, this file was: `/home/vignac/.conda/envs/moldiffusion/lib/python3.9/site-packages/ray_lightning/ray_ddp.py`

We will try to remove this dependency in the coming weeks.