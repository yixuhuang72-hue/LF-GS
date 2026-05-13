#  LF-GS: 3D Gaussian Splatting for View Synthesis of Multi-View Light Field Images
[[Paper](https://ieeexplore.ieee.org/document/11152586)]

## 1. Installation
This project uses conda environment management. Install dependencies via:

```bash
# Create conda environment
conda env create -f environment.yml
conda activate gaussian-splatting

# Install submodule dependencies
pip install ./submodules/diff-gaussian-rasterization
pip install ./submodules/simple-knn
```

## 2. Data Preparation

Dataset Structure
```bash
data/
├── images/                    # Input 2D images
├── depth/                     # Depth maps
└── sparse/                    # Camera parameters and initial 3D point cloud data
    └── 0/               
        ├── cameras.bin        # Camera intrinsic and extrinsic parameters
        ├── depth_params.json  # Depth-related configuration parameters
        ├── images.bin         # Image metadata 
        └── points3D.bin       # Initial 3D point cloud data
```


(1) Point cloud initialization: Referring to COLMAP (https://github.com/colmap/colmap), we adopt the COLMAP approach for point cloud initialization.

(2) Depth Map Generation

● Depth Maps: Consistent with the paper, we arrange light field sub-aperture images into a pseudo-video sequence, and then use Video Depth Anything (https://github.com/DepthAnything/Video-Depth-Anything) to obtain depth maps.

Sample Data: Given the large volume of the original full dataset, the ./data directory only contains lightweight test data (central sub-aperture images only) for easy testing and validation.

## 3. Training

 Example training command:

```bash
python train.py -s <dataset_root> --eval -d depth
```

● -s : Dataset root path
● --eval : Enable validation testing
● -d depth : Enable depth-aware training mode

## 4. Evaluation
```bash
# Generate renderings
python render.py -m <model_output_path>

# Compute error metrics (PSNR/SSIM/LPIPS)
python metrics.py -m <model_output_path>
```

This repository is built upon the excellent work 3D Gaussian Splatting for Real-Time Radiance Field Rendering. We heavily utilized their codebase as the foundation for our modifications.
We sincerely thank the authors for their open-source contribution. For more details regarding the original method or issues related to the core implementation, please refer to the original repository.
If you use this code in your research, please explicitly cite the original paper:

```bash
@Article{kerbl3Dgaussians,
      author       = {Kerbl, Bernhard and Kopanas, Georgios and Leimk{\"u}hler, Thomas and Drettakis, George},
      title        = {3D Gaussian Splatting for Real-Time Radiance Field Rendering},
      journal      = {ACM Transactions on Graphics},
      number       = {4},
      volume       = {42},
      month        = {July},
      year         = {2023},
      url          = {[https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/]}
}
```
