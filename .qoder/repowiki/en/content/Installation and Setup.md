# Installation and Setup

<cite>
**Referenced Files in This Document**   
- [README.md](file://README.md)
- [setup.py](file://setup.py)
- [setup_ubuntu.sh](file://scripts/setup_ubuntu.sh)
- [setup_al2.sh](file://scripts/setup_al2.sh)
- [Dockerfile](file://Dockerfile)
- [docker/rockylinux-9.2/Dockerfile](file://docker/rockylinux-9.2/Dockerfile)
- [requirements/base.txt](file://requirements/base.txt)
- [requirements/extra-pdf-image.txt](file://requirements/extra-pdf-image.txt)
- [requirements/extra-docx.txt](file://requirements/extra-docx.txt)
- [requirements/extra-pptx.txt](file://requirements/extra-pptx.txt)
- [requirements/extra-xlsx.txt](file://requirements/extra-xlsx.txt)
- [requirements/extra-csv.txt](file://requirements/extra-csv.txt)
- [requirements/extra-epub.txt](file://requirements/extra-epub.txt)
- [requirements/extra-odt.txt](file://requirements/extra-odt.txt)
- [requirements/extra-markdown.txt](file://requirements/extra-markdown.txt)
- [requirements/extra-pandoc.txt](file://requirements/extra-pandoc.txt)
- [requirements/extra-paddleocr.txt](file://requirements/extra-paddleocr.txt)
- [scripts/install-pandoc.sh](file://scripts/install-pandoc.sh)
</cite>

## Table of Contents
1. [PyPI Installation with Extras](#pypi-installation-with-extras)
2. [System Dependencies](#system-dependencies)
3. [Ubuntu Setup](#ubuntu-setup)
4. [Amazon Linux 2 Setup](#amazon-linux-2-setup)
5. [Docker Installation](#docker-installation)
6. [Development Environment Setup](#development-environment-setup)
7. [Common Installation Issues](#common-installation-issues)
8. [Performance Considerations](#performance-considerations)

## PyPI Installation with Extras

The `unstructured` library supports installation via PyPI with various extras for different file types. The installation approach depends on the document types you need to process.

For basic text processing (plain text, HTML, XML, JSON, and emails) without additional dependencies:
```bash
pip install unstructured
```

To install support for all document types:
```bash
pip install "unstructured[all-docs]"
```

For specific document types, use the corresponding extras:

**Document-specific installations:**
- PDF and image processing: `pip install "unstructured[pdf,image]"`
- DOCX files: `pip install "unstructured[docx]"`
- PPTX files: `pip install "unstructured[pptx]"`
- XLSX files: `pip install "unstructured[xlsx]"`
- CSV/TSV files: `pip install "unstructured[csv]"`
- EPUB files: `pip install "unstructured[epub]"`
- ODT files: `pip install "unstructured[odt]"`
- Markdown files: `pip install "unstructured[md]"`
- RTF and other formats requiring pandoc: `pip install "unstructured[rtf]"`

The extras system allows you to install only the dependencies needed for your specific use case, reducing installation size and potential conflicts.

**Section sources**
- [README.md](file://README.md#L109-L111)
- [setup.py](file://setup.py#L59-L75)
- [requirements/base.txt](file://requirements/base.txt)
- [requirements/extra-pdf-image.txt](file://requirements/extra-pdf-image.txt)
- [requirements/extra-docx.txt](file://requirements/extra-docx.txt)
- [requirements/extra-pptx.txt](file://requirements/extra-pptx.txt)
- [requirements/extra-xlsx.txt](file://requirements/extra-xlsx.txt)
- [requirements/extra-csv.txt](file://requirements/extra-csv.txt)
- [requirements/extra-epub.txt](file://requirements/extra-epub.txt)
- [requirements/extra-odt.txt](file://requirements/extra-odt.txt)
- [requirements/extra-markdown.txt](file://requirements/extra-markdown.txt)
- [requirements/extra-pandoc.txt](file://requirements/extra-pandoc.txt)

## System Dependencies

The `unstructured` library requires several system-level dependencies for processing different document types. These must be installed separately from the Python package.

**Core system dependencies:**
- `libmagic-dev`: For filetype detection
- `poppler-utils`: For PDF and image processing
- `tesseract-ocr`: For OCR capabilities in images and PDFs
- `libreoffice`: For MS Office document processing
- `pandoc`: For EPUB, RTF, and OpenOffice document conversion

**Additional considerations:**
- For enhanced language support with Tesseract, install language packs (e.g., `tesseract-ocr-rus` for Russian)
- Pandoc version 2.14.2 or newer is required for RTF file processing
- The `libreoffice` dependency enables conversion of legacy Office formats
- `poppler-utils` provides essential PDF rendering capabilities

These dependencies can be installed using your system's package manager before installing the Python package.

**Section sources**
- [README.md](file://README.md#L114-L118)
- [setup_ubuntu.sh](file://scripts/setup_ubuntu.sh)
- [setup_al2.sh](file://scripts/setup_al2.sh)
- [scripts/install-pandoc.sh](file://scripts/install-pandoc.sh)

## Ubuntu Setup

For Ubuntu systems, use the provided setup script to install all necessary dependencies. The script automates the installation of system packages, Python environment, and document processing tools.

**Step-by-step Ubuntu installation:**

1. Download and run the setup script:
```bash
wget https://raw.githubusercontent.com/Unstructured-IO/unstructured/main/scripts/setup_ubuntu.sh
chmod +x setup_ubuntu.sh
./setup_ubuntu.sh $USER
```

2. The script will:
   - Update package lists and upgrade existing packages
   - Install essential build tools and utilities
   - Install Git for version control
   - Set up pyenv for Python version management
   - Install Python 3.10.4
   - Install OpenCV dependencies
   - Install poppler-utils for PDF processing
   - Install LibreOffice and pandoc for document conversion
   - Install Tesseract OCR with Russian language support
   - Install libmagic for filetype detection

3. After the script completes, install the Python package:
```bash
pip install "unstructured[all-docs]"
```

The setup script handles dependency installation in the correct order and configures the environment for optimal document processing performance.

**Section sources**
- [setup_ubuntu.sh](file://scripts/setup_ubuntu.sh)
- [README.md](file://README.md#L55-L130)

## Amazon Linux 2 Setup

For Amazon Linux 2 environments, use the dedicated setup script that accounts for the system's specific package management and library requirements.

**Amazon Linux 2 installation steps:**

1. Run the Amazon Linux 2 setup script:
```bash
wget https://raw.githubusercontent.com/Unstructured-IO/unstructured/main/scripts/setup_al2.sh
chmod +x setup_al2.sh
./setup_al2.sh $USER
```

2. Key differences from Ubuntu setup:
   - Uses `yum` instead of `apt` for package management
   - Installs a newer version of `sed` from source
   - Uses `mesa-libGL` instead of `libgl1` for OpenCV dependencies
   - Builds Tesseract OCR from source with all dependencies
   - Installs `file-devel` instead of `libmagic-dev`

3. The Tesseract installation process includes:
   - Installing required development libraries
   - Building and installing Leptonica (Tesseract dependency)
   - Installing autoconf-archive
   - Cloning and building Tesseract from GitHub
   - Installing Tesseract language data

4. After script completion, install the Python package:
```bash
pip install "unstructured[all-docs]"
```

The Amazon Linux 2 script ensures all native dependencies are properly compiled and linked for the specific environment.

**Section sources**
- [setup_al2.sh](file://scripts/setup_al2.sh)
- [README.md](file://README.md#L55-L130)

## Docker Installation

The `unstructured` library provides Docker images for containerized deployment, offering consistent environments across different platforms.

**Default Docker image:**

1. Pull the latest image:
```bash
docker pull downloads.unstructured.io/unstructured-io/unstructured:latest
```

2. Run the container:
```bash
docker run -dt --name unstructured downloads.unstructured.io/unstructured-io/unstructured:latest
docker exec -it unstructured bash
```

3. The default Dockerfile:
   - Uses wolfi-base as the base image
   - Installs font packages and Git
   - Copies requirements and source code
   - Installs Python dependencies
   - Downloads NLTK packages
   - Initializes machine learning models

**Rocky Linux 9.2 based image:**

1. Build from the Rocky Linux Dockerfile:
```bash
cd docker/rockylinux-9.2
docker build -t unstructured-rocky .
```

2. Key features of the Rocky Linux image:
   - Uses Rocky Linux 9.2 as the base
   - Installs development tools group
   - Uses dnf package manager
   - Pre-installs all Python dependencies
   - Downloads NLTK packages
   - Initializes partitioning models

3. Run the Rocky Linux container:
```bash
docker run -it unstructured-rocky /bin/bash
```

Both Docker images provide isolated environments with all dependencies pre-installed, ensuring consistent behavior across different host systems.

**Section sources**
- [Dockerfile](file://Dockerfile)
- [docker/rockylinux-9.2/Dockerfile](file://docker/rockylinux-9.2/Dockerfile)
- [README.md](file://README.md#L55-L91)

## Development Environment Setup

For developers contributing to the `unstructured` library, a comprehensive development environment is available.

**Local development setup:**

1. Prerequisites:
   - Install pyenv for Python version management
   - Install Python 3.10
   - Create and activate a virtual environment

```bash
pyenv install 3.10
pyenv virtualenv 3.10 unstructured
pyenv activate unstructured
```

2. Install dependencies:
```bash
make install
```

3. Optional components:
   - For image and PDF processing: `make install-local-inference`
   - Install pre-commit hooks: `pre-commit install`

4. Docker-based development:
```bash
make docker-start-dev
```

This starts a Docker container with the local repository mounted, allowing development without system compatibility issues.

**Test dependencies:**
- Install test requirements: `pip install -r requirements/test.txt`
- The test suite includes comprehensive coverage for all document types
- Development dependencies include linting and formatting tools

The development environment supports both local and containerized workflows, accommodating different developer preferences and system configurations.

**Section sources**
- [README.md](file://README.md#L132-L168)
- [requirements/dev.txt](file://requirements/dev.txt)
- [requirements/test.txt](file://requirements/test.txt)
- [Makefile](file://Makefile)

## Common Installation Issues

This section addresses frequent installation problems and their solutions.

**Missing system libraries:**
- **Issue**: "Command not found" for pdf2image, tesseract, or libreoffice
- **Solution**: Install the missing system dependencies using your package manager
- For Ubuntu: `sudo apt-get install poppler-utils tesseract-ocr libreoffice`
- For Amazon Linux 2: `sudo yum install poppler-utils tesseract libreoffice`

**OCR engine configuration:**
- **Issue**: Tesseract not detecting text in images
- **Solution**: Verify Tesseract installation and language data
- Check Tesseract version: `tesseract --version`
- Ensure language data is in the correct directory: `/usr/share/tesseract-ocr/4/tessdata/`
- Test Tesseract independently: `tesseract image.png stdout`

**Pandoc version issues:**
- **Issue**: RTF files not processing correctly
- **Solution**: Ensure pandoc version 2.14.2 or newer
- Use the provided installation script: `./scripts/install-pandoc.sh`
- Or install via pip: `pip install pypandoc`

**Python dependency conflicts:**
- **Issue**: Version conflicts between packages
- **Solution**: Use virtual environments to isolate dependencies
- Create a clean virtual environment and reinstall
- Use the constraints file to ensure compatible versions

**Docker build failures:**
- **Issue**: Base image (wolfi-base) update failures
- **Solution**: The base image is updated regularly; rebuild if encountering issues
- Use the Rocky Linux image as an alternative

These common issues can typically be resolved by verifying system dependencies and following the recommended installation procedures.

**Section sources**
- [README.md](file://README.md#L114-L118)
- [scripts/install-pandoc.sh](file://scripts/install-pandoc.sh)
- [setup_ubuntu.sh](file://scripts/setup_ubuntu.sh)
- [setup_al2.sh](file://scripts/setup_al2.sh)

## Performance Considerations

Deployment performance varies based on the environment and document types being processed.

**Resource requirements by document type:**
- **Text/HTML/XML**: Low memory, minimal CPU
- **PDF/Image with OCR**: High memory, significant CPU
- **Office documents**: Moderate memory, moderate CPU
- **Large documents**: Scale linearly with document size

**Optimization strategies:**
- **Ubuntu/Amazon Linux 2**: Native compilation provides optimal performance
- **Docker**: Slight overhead but consistent across environments
- **Rocky Linux image**: Optimized for enterprise environments

**Scaling recommendations:**
- For high-volume processing, use native installation on Ubuntu
- For consistent environments across teams, use Docker
- For production deployments, consider the Rocky Linux image
- Monitor memory usage when processing documents with OCR

**Performance tips:**
- Pre-install all required language packs for Tesseract
- Use SSD storage for temporary files
- Allocate sufficient memory for large document processing
- Consider parallel processing for batch jobs

The choice of installation method should balance performance requirements with deployment complexity and environment consistency needs.

**Section sources**
- [README.md](file://README.md)
- [setup_ubuntu.sh](file://scripts/setup_ubuntu.sh)
- [setup_al2.sh](file://scripts/setup_al2.sh)
- [Dockerfile](file://Dockerfile)
- [docker/rockylinux-9.2/Dockerfile](file://docker/rockylinux-9.2/Dockerfile)