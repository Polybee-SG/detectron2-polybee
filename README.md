<img src=".github/Detectron2-Logo-Horz.svg" width="300" >

Detectron2 is Facebook AI Research's next generation library
that provides state-of-the-art detection and segmentation algorithms.
It is the successor of
[Detectron](https://github.com/facebookresearch/Detectron/)
and [maskrcnn-benchmark](https://github.com/facebookresearch/maskrcnn-benchmark/).
It supports a number of computer vision research projects and production applications in Facebook.

<div align="center">
  <img src="https://user-images.githubusercontent.com/1381301/66535560-d3422200-eace-11e9-9123-5535d469db19.png"/>
</div>
<br>

## Learn More about Detectron2

* Includes new capabilities such as panoptic segmentation, Densepose, Cascade R-CNN, rotated bounding boxes, PointRend,
  DeepLab, ViTDet, MViTv2 etc.
* Used as a library to support building [research projects](projects/) on top of it.
* Models can be exported to TorchScript format or Caffe2 format for deployment.
* It [trains much faster](https://detectron2.readthedocs.io/notes/benchmarks.html).

See our [blog post](https://ai.meta.com/blog/-detectron2-a-pytorch-based-modular-object-detection-library-/)
to see more demos.
See this [interview](https://ai.meta.com/blog/detectron-everingham-prize/) to learn more about the stories behind detectron2.

## Installation

See [installation instructions](https://detectron2.readthedocs.io/tutorials/install.html).

## Building a Wheel

Requirements: PyTorch >= 1.8, CUDA toolkit (>= 13.0 recommended), and a C++ compiler.

The build will automatically use whatever PyTorch, CUDA, and C++ compiler are available in your local environment. The resulting wheel is tied to those versions and may not be compatible with a different environment.

We recommend using [Miniconda](https://docs.conda.io/en/latest/miniconda.html) to manage the Python environment and C++ compiler, and installing PyTorch from its official pip wheels (the `pytorch` conda channel no longer ships current builds, and there is no `pytorch-cuda=13.0` conda package):

```bash
conda create -n detectron2 python=3.10
conda activate detectron2
conda install -c conda-forge gxx
pip install torch==2.10.0 torchvision==0.25.0 --index-url https://download.pytorch.org/whl/cu130
```

Notes:

- **Python version:** set `python=3.10` to whichever version you want the wheel built for — the resulting wheel is tagged accordingly (e.g. `cp310`) and only installs on that Python version. PyTorch must publish a matching wheel for that version.
- **CUDA version:** pick the wheel index that matches your local CUDA toolkit, e.g. `cu130` for CUDA 13.0 (run `nvcc --version` to check).
- **Compiler:** `gxx` provides the C++ compiler nvcc needs to build the CUDA extension. Skip it only if you already have a compatible system `g++`.
- The `torch`/`torchvision` versions above are a known-good pairing built against CUDA 13.0.

**Build the wheel:**

```bash
python setup.py bdist_wheel
```

The wheel will be output to `dist/`. To force a CUDA build even if `torch.cuda.is_available()` returns false:

```bash
FORCE_CUDA=1 python setup.py bdist_wheel
```

**Verify CUDA extensions are included:**

```bash
unzip -l dist/detectron2-*.whl | grep _C
```

You should see `detectron2/_C.cpython-*.so` in the listing.

**Install from the wheel:**

```bash
pip install dist/detectron2-*.whl
```

**Publish to AWS CodeArtifact:**

```bash
pip install twine --index-url https://pypi.org/simple/

aws codeartifact login --tool twine \
  --domain <your-domain> \
  --domain-owner <your-account-id> \
  --repository <your-repository> \
  --region <your-region>

twine upload --repository codeartifact dist/detectron2-*.whl
```

**Install from AWS CodeArtifact:**

```bash
aws codeartifact login --tool pip \
  --domain <your-domain> \
  --domain-owner <your-account-id> \
  --repository <your-repository> \
  --region <your-region>

pip install detectron2
```

## Getting Started

See [Getting Started with Detectron2](https://detectron2.readthedocs.io/tutorials/getting_started.html),
and the [Colab Notebook](https://colab.research.google.com/drive/16jcaJoc6bCFAQ96jDe2HwtXj7BMD_-m5)
to learn about basic usage.

Learn more at our [documentation](https://detectron2.readthedocs.org).
And see [projects/](projects/) for some projects that are built on top of detectron2.

## Model Zoo and Baselines

We provide a large set of baseline results and trained models available for download in the [Detectron2 Model Zoo](MODEL_ZOO.md).

## License

Detectron2 is released under the [Apache 2.0 license](LICENSE).

## Citing Detectron2

If you use Detectron2 in your research or wish to refer to the baseline results published in the [Model Zoo](MODEL_ZOO.md), please use the following BibTeX entry.

```BibTeX
@misc{wu2019detectron2,
  author =       {Yuxin Wu and Alexander Kirillov and Francisco Massa and
                  Wan-Yen Lo and Ross Girshick},
  title =        {Detectron2},
  howpublished = {\url{https://github.com/facebookresearch/detectron2}},
  year =         {2019}
}
```
