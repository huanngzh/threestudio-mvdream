# threestudio-mvdream
| MVDream | DMTet Refine |
| - | - |
| ![mvdream](https://github.com/huanngzh/threestudio-mvdream/assets/78398294/74246b6a-7c7a-40d5-9d0c-eb5fc79535fe) | ![mvdream_refined](https://github.com/huanngzh/threestudio-mvdream/assets/78398294/1b919fd4-39ca-44e0-abe7-e86a51872525) |

The MVDream extension for threestudio. The original implementation can be found at https://github.com/bytedance/MVDream-threestudio. We thank them for their contribution to the 3D generation community. To use it, please install [threestudio](https://github.com/threestudio-project/threestudio) first and then install this extension in threestudio `custom` directory.

# Installation
```
cd custom
git clone https://github.com/DSaurus/threestudio-mvdream.git
cd threestudio-mvdream

# First install xformers (https://github.com/facebookresearch/xformers#installing-xformers)
# cuda 11.8 version
pip3 install -U xformers --index-url https://download.pytorch.org/whl/cu118
# cuda 12.1 version
# pip3 install -U xformers --index-url https://download.pytorch.org/whl/cu121

# Then install other dependencies
pip install -r requirements.txt
```

# Quick Start
```bash
# MVDream without shading (memory efficient)
python launch.py --config custom/threestudio-mvdream/configs/mvdream-sd21.yaml --train --gpu 0 system.prompt_processor.prompt="an astronaut riding a horse"

# MVDream with shading (used in paper)
python launch.py --config custom/threestudio-mvdream/configs/mvdream-sd21-shading.yaml --train --gpu 0 system.prompt_processor.prompt="an astronaut riding a horse"

# MVDream with DMTet refine
python launch.py --config custom/threestudio-mvdream/configs/mvdream-sd21-refine.yaml --train --gpu 0 system.prompt_processor.prompt="an astronaut riding a horse" system.geometry_convert_from=outputs/mvdream-sd21-rescale0.5-shading/an_astronaut_riding_a_horse@20231226-141418/ckpts/last.ckpt
# !!! Change geometry_convert_from to your ckpt path
# Try to set isosurface_resolution to 256 and generate 256_tets.npz using load/tets/generate_tets.py for better refinement.
```

# Resume from checkpoints
```
# resume training from the last checkpoint, you may replace last.ckpt with any other checkpoints
python launch.py --config path/to/trial/dir/configs/parsed.yaml --train --gpu 0 resume=path/to/trial/dir/ckpts/last.ckpt
# if the training has completed, you can still continue training for a longer time by setting trainer.max_steps
python launch.py --config path/to/trial/dir/configs/parsed.yaml --train --gpu 0 resume=path/to/trial/dir/ckpts/last.ckpt trainer.max_steps=20000
# you can also perform testing using resumed checkpoints
python launch.py --config path/to/trial/dir/configs/parsed.yaml --test --gpu 0 resume=path/to/trial/dir/ckpts/last.ckpt
# note that the above commands use parsed configuration files from previous trials
# which will continue using the same trial directory
# if you want to save to a new trial directory, replace parsed.yaml with raw.yaml in the command

# only load weights from saved checkpoint but dont resume training (i.e. dont load optimizer state):
python launch.py --config path/to/trial/dir/configs/parsed.yaml --train --gpu 0 system.weights=path/to/trial/dir/ckpts/last.ckpt
```

# Citing

If you find MVDream helpful, please consider citing:

```
@article{shi2023MVDream,
  author = {Shi, Yichun and Wang, Peng and Ye, Jianglong and Mai, Long and Li, Kejie and Yang, Xiao},
  title = {MVDream: Multi-view Diffusion for 3D Generation},
  journal = {arXiv:2308.16512},
  year = {2023},
}
```
