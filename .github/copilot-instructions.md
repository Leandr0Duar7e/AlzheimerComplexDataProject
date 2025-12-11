# Alzheimer Complex Data Project - AI Coding Agent Instructions

## Project Overview
This is a university research project for the "Complex Data in Health" course at UPM, focusing on Alzheimer's disease through multi-modal data analysis including transcriptomics (single-cell RNA-seq) and deep learning (PyTorch).

## Project Structure

### Core Components
1. **`scripts/`** - Main analysis directory with two sub-areas:
   - **Main scripts/** - Contains transcriptome analysis (`23Transcriptome.ipynb`) and medical terminology queries (`1_MedicalTerminologies.ipynb`)
   - **`scripts/Part_4/`** - PyTorch tutorials and deep learning exercises (7 notebooks covering datasets, tensors, training loops, and image processing)

2. **`Data/`** - Contains GEO dataset GSE300079 (single-cell RNA-seq data from mouse brain)
   - Series matrix files and raw data from 10x Genomics format
   - Gene lists (upregulated/downregulated genes)

### Environment Setup
- **Virtual environment location**: `scripts/.venv/`
- **Setup commands** (from project root):
  ```bash
  python3 -m venv scripts/.venv
  source scripts/.venv/bin/activate  # Linux/Mac
  # scripts/.venv/Scripts/activate  # Windows
  pip install -r scripts/requirements.txt
  ```

## Scientific Domain

### Biological Context
- **Dataset**: GSE300079 - Mouse brain single-cell RNA-seq studying APOE switching
- **Research question**: Gene expression changes when switching from APOE4 (risk allele) to APOE2 (protective allele)
- **Sample groups**:
  - Control: APOE4-expressing mice (GSM9053416_S9, GSM9053419_S12)
  - Treatment: APOE4→E2 switched mice (GSM9053410_S1, GSM9053411_S2, GSM9053412_S3)

### Data Processing Approach
- **Pseudo-bulking**: Aggregate single-cell counts per sample to create bulk profiles
- **10x Genomics format**: Data uses standard 10x structure (barcodes.tsv.gz, features.tsv.gz, matrix.mtx.gz)
- **Analysis pipeline**: Load with scanpy → pseudo-bulk → differential expression → visualization (volcano plots, heatmaps)

## Code Patterns & Conventions

### Notebook Organization
- **Part_4 tutorials follow standard structure**:
  1. Custom PyTorch Dataset
  2. MNIST Example
  3. Tensors
  4. Training loop
  5. Image tasks
  6. Data processing
  7. Image filters

### PyTorch Conventions
- Use `torch.utils.data.DataLoader` with batch_size=64 as default
- Image transforms via `torchvision.transforms.Compose()`
- Standard device pattern: `device = torch.device("cuda" if torch.cuda.is_available() else "cpu")`
- Training loops separate `train_loop()` and `test_loop()` functions
- Model classes inherit from `nn.Module` with `__init__` and `forward` methods

### Data Loading Patterns
- **Single-cell data**: Use `scanpy.read_10x_mtx()` for 10x Genomics data
- **Image data**: Standard torchvision datasets (MNIST, CIFAR10) with `ToTensor()` transform
- **Visualization**: matplotlib with `plt.figure(figsize=(10, 10))` for consistency

### Medical Terminology
- SPARQL queries to NCI Thesaurus endpoint: `https://shared.semantics.cancer.gov/sparql`
- Query pattern extracts: Preferred_Name, DEFINITION, Semantic_Type, UMLS_CUI for disease concepts

## Key Dependencies
- **Scientific computing**: numpy, pandas, matplotlib
- **Deep learning**: torch, torchvision
- **Bioinformatics**: scanpy (for single-cell analysis)
- **Semantic web**: SPARQLWrapper (for medical ontology queries)
- **Image processing**: PIL, cv2 (OpenCV)

## Critical Workflows

### Running Notebooks
- Jupyter notebooks are the primary development environment
- Execute cells sequentially as they build on prior state
- Part_4 notebooks are self-contained tutorials

### Single-cell Analysis Workflow
1. Load 10x data per sample with `sc.read_10x_mtx()`
2. Sum counts across cells (pseudo-bulking): `adata.X.sum(axis=0)`
3. Create expression matrix with samples as columns
4. Perform statistical analysis (t-tests for differential expression)
5. Visualize with volcano plots and save results

### Image Processing Pipeline
- Load images → Apply transforms (resize, normalize, augment) → Create DataLoader → Train model
- Common transforms: `ToTensor()`, `Normalize()`, `Resize()`, `GaussianBlur()`, `ColorJitter()`
- Filters: Use cv2 for advanced operations (Laplacian for edge detection)

## Important Notes
- **Data paths**: Relative paths assume execution from notebook directory
- **Output directories**: Results saved to `../Data/` from notebooks
- **Sample IDs**: GSM identifiers map to biological conditions (see metadata in 23Transcriptome.ipynb)
- **Windows environment**: Current workspace is Windows (OneDrive path), adjust shell commands accordingly
