<div align="center">

<h1>Dual Visual–Textual Graph Construction for Open-Vocabulary Semantic Segmentation</h1>

</div>

## Abstract
> *Open-vocabulary semantic segmentation aims to assign pixel-level labels using arbitrary text queries, but existing CLIP-based methods often produce diffuse similarity maps and struggle with precise boundaries or small objects. Two-stage approaches—first generating class-agnostic mask proposals, then aligning them to text embeddings—improve shape fidelity but suffer from over-segmentation when using dense SAM proposals and from coarse boundaries when using patch-based spectral clustering. To address these limitations, we propose a novel graph-construction pipeline in which nodes correspond to SAM’s class-agnostic masks—providing fine boundary precision—and are augmented with textual nodes representing the target class embeddings. Edge weights capture both inter‐mask visual similarity and visual–textual affinity via cosine similarity in a joint embedding space, ensuring that clusters reflect full-object semantics rather than only discriminative parts. Our dual visual–textual graph promotes geometrically coherent, semantically complete mask proposals without the need for costly post-processing refinements. With the proposed method, state-of-the-art performance is attained over the latest approaches on several benchmarks*

## Dependencies and Installation


```
# git clone this repository
git clone https://github.com/DailyVy/DVGT.git
cd DVGT

# create new anaconda env
conda create -n DVGT python=3.10
conda activate DVGT

# install torch and dependencies
pip install -r docker/requirements.txt
```

## SAM

Download the SAM model as follows:

```bash
wget -P model https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth
```

## Datasets
We include the following dataset configurations in this repo: 
1) `With background class`: PASCAL VOC, PASCAL Context, ADE20k, and COCO-Stuff164k, 
2) `Without background class`: VOC20, Context59 (i.e., PASCAL VOC and PASCAL Context without the background category), and COCO-Object.

Please follow the [MMSeg data preparation document](https://github.com/open-mmlab/mmsegmentation/blob/main/docs/en/user_guides/2_dataset_prepare.md) to download and pre-process the datasets. 
The COCO-Object dataset can be converted from COCO-Stuff164k by executing the following command:

```
python datasets/cvt_coco_object.py PATH_TO_COCO_STUFF164K -o PATH_TO_COCO164K
```

## Quick Inference
```
python demo.py
```

To run inference with custom settings:
```
python demo.py --image path/to/your/image.jpg --classes animal landscape sky --output path/to/your/output.jpg --alpha 0.8
```


## Model evaluation
Please modify some settings in `configs/base_config.py` before running the evaluation.

Single-GPU:

```
python eval.py --config ./configs/cfg_DATASET.py --workdir YOUR_WORK_DIR
```

Multi-GPU:
```
bash ./dist_test.sh ./configs/cfg_DATASET.py
```

## Citation

If you find this project useful, please cite our [IEEE Access paper](https://ieeexplore.ieee.org/document/11359636):

```bibtex
@article{choi2026dual,
  author={Choi, Bigyeol and Jeon, Sangryul},
  journal={IEEE Access},
  title={Dual Visual--Textual Graph Construction for Open-Vocabulary Semantic Segmentation},
  year={2026},
  volume={14},
  pages={23656--23669},
  doi={10.1109/ACCESS.2026.3656497}
}
```

## License
The original DVGT code is released under the [MIT License](LICENSE). Third-party components and files derived from other projects remain subject to their respective copyright notices and license terms.


## Acknowledgement
This study is supported under the RIE2020 Industry Align- ment Fund – Industry Collaboration Projects (IAF-ICP) Funding Initiative, as well as cash and in-kind contribution from the industry partner(s).

This implementation is based on [OpenCLIP](https://github.com/mlfoundations/open_clip), [ProxyCLIP](https://github.com/mc-lan/ProxyCLIP), [SCESAME](https://github.com/ymgw55/SCESAME) and [PixelCLIP](https://github.com/cvlab-kaist/PixelCLIP/). Thanks for the awesome work.
