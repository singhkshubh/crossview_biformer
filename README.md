# <div align="center">**CrossView Biformers**</div>

<div align="center"><img src="docs/assets/teaser.jpg" width="65%"></div>
<br>
In This reposisotory I have explored implementation of cross attention for Bird's Eye View Sematic Segmentation from multiple camera views.
and further	implemented Bi-Level Routing Attention to make the transformer architecture more efficient by reducing the number of parameters making the process efficient. Also Implemented a Novel masked query attention approach to enhance the accuracy further. In order to take advantage of the geometric relation of the BEV map and individual images.


## <div align="center">**Demos**</div>

<br>

<div align="center"><img src="docs/assets/predictions.gif" width="75%"/></div>
<div align="center">
<b>Map-view Segmentation:</b>
The model uses multi-view images to produce a map-view segmentation at 45 FPS
</div>
<br>

<div align="center"><img src="docs/assets/map.gif" width="40%"/></div>
<div align="center">
<b>Map Making:</b>
With vehicle pose, we can construct a map by fusing model predictions over time
</div>
<br>

<div align="center"><img src="docs/assets/attention.gif" width="75%"/></div>
<div align="center">
<b>Cross-view Attention:</b>
For a given map-view location, we show which image patches are being attended to
</div>
<br>

## <div align="center">**Installation**</div>

```bash
# Clone repo
git clone https://github.com/singhkshubh/crossview_biformer
cd crossview_biformer

# Setup conda environment
conda create -y --name cvt python=3.8

conda activate cvt
conda install -y pytorch torchvision cudatoolkit=11.3 -c pytorch

# Install dependencies
pip install -r requirements.txt
pip install -e .
```

## <div align="center">**Data**</div>

<div align="center"><img src="docs/assets/view_data.gif" width="75%"/></div>
<br>

Documentation:
* [Dataset setup](docs/dataset_setup.md)
* [Label generation](docs/label_generation.md) (optional)

<br/>

Download the original datasets and our generated map-view labels

| | Dataset | Labels |
| :-- | :-- | :-- |
| nuScenes | [keyframes + map expansion](https://www.nuscenes.org/nuscenes#download) (60 GB) | [cvt_labels_nuscenes.tar.gz](https://www.cs.utexas.edu/~bzhou/cvt/cvt_labels_nuscenes.tar.gz) (361 MB) |
| Argoverse 1.1 | [3D tracking](https://www.argoverse.org/av1.html#download-link) | coming soon™ |

<br/>

The structure of the extracted data should look like the following

```
/datasets/
├─ nuscenes/
│  ├─ v1.0-trainval/
│  ├─ v1.0-mini/
│  ├─ samples/
│  ├─ sweeps/
│  └─ maps/
│     ├─ basemap/
│     └─ expansion/
└─ cvt_labels_nuscenes/
   ├─ scene-0001/
   ├─ scene-0001.json
   ├─ ...
   ├─ scene-1000/
   └─ scene-1000.json
```

When everything is setup correctly, check out the dataset with

```bash
python3 scripts/view_data.py \
  data=nuscenes \
  data.dataset_dir=/media/datasets/nuscenes \
  data.labels_dir=/media/datasets/cvt_labels_nuscenes \
  data.version=v1.0-mini \
  visualization=nuscenes_viz \
  +split=val
```

# <div align="center">**Training**</div>

<div align="center">
<a href="https://www.pytorchlightning.ai">
<img src="https://raw.githubusercontent.com/PyTorchLightning/pytorch-lightning/master/docs/source/_static/images/logo.png" width="25%">
</a>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<a href="https://wandb.ai/site">
<img src="https://raw.githubusercontent.com/wandb/client/master/.github/wb-logo-lightbg.png" width="25%">
</a>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<a href="https://hydra.cc">
<img src="https://raw.githubusercontent.com/facebookresearch/hydra/master/website/static/img/Hydra-Readme-logo2.svg" width="15%">
</a>
</div>

<br>

An average job of 50k training iterations takes ~8 hours.  
Our models were trained using 4 GPU jobs, but also can be trained on single GPU.

To train a model,

```bash
python3 scripts/train.py \
  +experiment=cvt_nuscenes_vehicle
  data.dataset_dir=/media/datasets/nuscenes \
  data.labels_dir=/media/datasets/cvt_labels_nuscenes
```

For more information, see

* `config/config.yaml` - base config
* `config/model/cvt.yaml` - model architecture
* `config/experiment/cvt_nuscenes_vehicle.yaml` - additional overrides

## <div align="center">**Additional Information**</div>

### **Refrences**
* https://github.com/bradyz/cross_view_transformers
* https://github.com/rayleizhu/BiFormer
* https://github.com/wayveai/fiery




#   c r o s s v i e w _ b i f o r m 
 
 
