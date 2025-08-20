## Get Started

### Environment Setup
First, create and activate a conda environment, then install the requirements:

```bash
# Create and activate conda environment
conda create -n gnn-rag python=3.8
conda activate gnn-rag

# Install PyTorch (choose the command based on your CUDA version)
# For CUDA 11.0:
pip install torch==1.7.1+cu110 -f https://download.pytorch.org/whl/torch_stable.html
# OR for CPU only:
# pip install torch==1.7.1+cpu -f https://download.pytorch.org/whl/torch_stable.html

# Install base requirements
pip install numpy==1.19.5 tqdm==4.59.0

# Install transformers and its dependencies
pip install transformers==4.6.1 six>=1.14.0

# Install sentence-transformers for SBERT
pip install sentence-transformers
```

We have simple requirements in `requirements.txt`. You can always check if you can run the code immediately.

### Download Datasets and LM
The datasets as well as the pretrained LM (LMsr) are uploaded here: https://drive.google.com/drive/folders/1ifgVHQDnvFEunP9hmVYT07Y3rvcpIfQp?usp=sharing

To download and set up the datasets and LM, run:
```bash
# Install gdown if not already installed
pip install gdown

# Create data directory
mkdir -p data

# Download datasets and LM directly to the data directory
cd data
gdown https://drive.google.com/drive/folders/1ifgVHQDnvFEunP9hmVYT07Y3rvcpIfQp --folder
cd ..
```

The datasets and LM will be extracted to their corresponding folders in the `data` directory.

## Training
Please follow the guidelines and hyperparamters of the corresponding GNNs for training. See `scripts` on a training example.  

Otherwise, you can download released GNN models from here: https://drive.google.com/file/d/1p7eLSsSKkZQxB32mT5lMsthVP6R_3x1j/view

To download the pretrained GNN models, run:
```bash
# Install gdown if not already installed
pip install gdown

# Download the pretrained models
gdown 1p7eLSsSKkZQxB32mT5lMsthVP6R_3x1j -O pretrained_gnn_models.zip

# Extract the models
unzip pretrained_gnn_models.zip
```

## Evaluation

To evaluate them, copy the command from the above scripts, add the `--is_eval` argument, and `--load experiment` followed by the name of the corresponding `ckpt` model.

For example, for Webqsp run:
```
python main.py ReaRev --entity_dim 50 --num_epoch 200 --batch_size 8 --eval_every 2 --data_folder data/webqsp/ --lm sbert --num_iter 3 --num_ins 2 --num_gnn 3 --relation_word_emb True --load_experiment ReaRev_webqsp.ckpt --is_eval --name webqsp
```

The result is saved as a `.info` file. In order to use GNN-RAG, please move this file to the corresponding folder in `GNN-RAG/llm/results/gnn/` by renaming it to `test.info`.