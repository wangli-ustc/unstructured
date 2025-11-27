# Getting Started

<cite>
**Referenced Files in This Document**   
- [auto.py](file://unstructured/partition/auto.py)
- [elements.py](file://unstructured/documents/elements.py)
- [README.md](file://README.md)
- [constants.py](file://unstructured/partition/utils/constants.py)
</cite>

## Table of Contents
1. [Introduction](#introduction)
2. [Installation and Setup](#installation-and-setup)
3. [Main Entry Point: partition.auto.partition()](#main-entry-point-partitionautopartition)
4. [Processing Different File Types](#processing-different-file-types)
5. [Handling Output Elements](#handling-output-elements)
6. [Command Line Interface](#command-line-interface)
7. [Common Patterns and Best Practices](#common-patterns-and-best-practices)
8. [Troubleshooting Common Issues](#troubleshooting-common-issues)
9. [Performance Tips](#performance-tips)

## Introduction

The unstructured library provides a powerful toolkit for processing various document formats and extracting meaningful content. This guide will help you get started with the library, focusing on practical usage examples and best practices for beginners.

The library's core functionality revolves around document partitioning - breaking down documents into their constituent elements like text, tables, headers, and other semantic components. The main entry point for this functionality is the `partition.auto.partition()` function, which automatically detects file types and routes them to appropriate processing functions.

This guide will walk you through the basics of using the unstructured library, demonstrating how to process sample documents from the example-docs directory, handle the output elements, and extract meaningful content.

**Section sources**
- [README.md](file://README.md#L46-L256)

## Installation and Setup

To get started with the unstructured library, you'll need to install it along with any required dependencies for the document types you plan to process.

For basic text processing, you can install the core library:
```bash
pip install unstructured
```

To support all document types, including PDFs, images, and Office documents:
```bash
pip install "unstructured[all-docs]"
```

For specific document types, you can install targeted extras:
```bash
pip install "unstructured[docx,pptx]"
```

Additionally, you may need to install system dependencies depending on your document types:
- `libmagic-dev` for filetype detection
- `poppler-utils` for PDF and image processing
- `tesseract-ocr` for OCR capabilities
- `libreoffice` for MS Office documents
- `pandoc` for EPUB, RTF, and Open Office documents

**Section sources**
- [README.md](file://README.md#L109-L119)

## Main Entry Point: partition.auto.partition()

The primary entry point for the unstructured library is the `partition.auto.partition()` function. This function automatically detects the file type and routes the document to the appropriate partitioning function.

```python
from unstructured.partition.auto import partition

# Process a document by filename
elements = partition(filename="example-docs/fake-text.txt")

# Process a document from a file object
with open("example-docs/fake-text.txt", "rb") as f:
    elements = partition(file=f)
```

The `partition()` function accepts several parameters:

**Core Parameters:**
- `filename`: Path to the file to be processed
- `file`: File-like object in binary mode
- `url`: URL of a remote document
- `encoding`: Character encoding (default: "utf-8")

**Processing Strategy Parameters:**
- `strategy`: Processing strategy (auto, fast, ocr_only, hi_res)
- `languages`: List of languages present in the document
- `detect_language_per_element`: Detect language for each element individually

**PDF/Image Specific Parameters:**
- `pdf_infer_table_structure`: Extract table structure from PDFs
- `extract_images_in_pdf`: Extract images from PDFs
- `hi_res_model_name`: Layout detection model for hi_res strategy

The function returns a list of Element objects, each representing a semantic component of the document.

**Section sources**
- [auto.py](file://unstructured/partition/auto.py#L30-L139)

## Processing Different File Types

The unstructured library supports a wide variety of file types. Let's explore how to process different document types using the example documents provided.

### Text Files
```python
from unstructured.partition.auto import partition

# Process a plain text file
elements = partition(filename="example-docs/fake-text.txt")
print(f"Extracted {len(elements)} elements")
for element in elements:
    print(f"Type: {element.type}, Text: {element.text}")
```

### Email Files
```python
# Process an email file
elements = partition(filename="example-docs/fake-email.txt")
for element in elements:
    if hasattr(element, 'text'):
        print(element.text)
```

### HTML Files
```python
# Process an HTML file
elements = partition(filename="example-docs/example-10k-1p.html")
# Extract just the narrative text
narrative_elements = [el for el in elements if el.type == "NarrativeText"]
for element in narrative_elements:
    print(element.text)
```

### JSON Files
```python
# Process a JSON file containing unstructured output
elements = partition(filename="example-docs/simple.json")
for element in elements:
    print(f"{element.type}: {element.text}")
```

The library automatically detects the file type using libmagic and routes it to the appropriate partitioning function, making it easy to process multiple document types with a consistent API.

**Section sources**
- [auto.py](file://unstructured/partition/auto.py#L210-L293)
- [README.md](file://README.md#L193-L234)

## Handling Output Elements

When you partition a document, the library returns a list of Element objects. Each element contains both text content and metadata about its properties.

### Element Structure
Each Element object has the following key attributes:
- `text`: The extracted text content
- `type`: The semantic type (Title, NarrativeText, ListItem, etc.)
- `metadata`: Additional information about the element

```python
from unstructured.partition.auto import partition

elements = partition(filename="example-docs/fake-text.txt")

for element in elements:
    print(f"Element Type: {element.type}")
    print(f"Text: {element.text}")
    print(f"Metadata: {element.metadata.to_dict()}")
    print("---")
```

### Accessing Metadata
The metadata contains valuable information about each element:

```python
element = elements[0]

# Basic file information
print(f"Filename: {element.metadata.filename}")
print(f"Filetype: {element.metadata.filetype}")
print(f"Last modified: {element.metadata.last_modified}")

# Document structure
print(f"Page number: {element.metadata.page_number}")
print(f"Parent ID: {element.metadata.parent_id}")

# Content-specific metadata
if element.metadata.text_as_html:
    print(f"HTML representation: {element.metadata.text_as_html}")
```

### Element Types
The library categorizes elements into different types:
- `Title`: Document or section titles
- `NarrativeText`: Paragraphs of text
- `ListItem`: Items in a list
- `Address`: Address information
- `Table`: Tabular data
- `Image`: Image elements
- `UncategorizedText`: Text that doesn't fit other categories

You can filter elements by type:
```python
# Get all titles
titles = [el for el in elements if el.type == "Title"]

# Get all narrative text
narrative_text = [el for el in elements if el.type == "NarrativeText"]

# Get all lists
list_items = [el for el in elements if el.type == "ListItem"]
```

**Section sources**
- [elements.py](file://unstructured/documents/elements.py#L149-L300)

## Command Line Interface

In addition to the Python API, the unstructured library provides a command-line interface for processing documents.

### Basic Usage
```bash
# Process a single file
unstructured partition --filename example-docs/fake-text.txt

# Process multiple files
unstructured partition --filename example-docs/fake-text.txt --filename example-docs/fake-email.txt

# Process all files in a directory
unstructured partition --directory example-docs/
```

### Output Formats
```bash
# Output as JSON
unstructured partition --filename example-docs/fake-text.txt --output-format json

# Output as text
unstructured partition --filename example-docs/fake-text.txt --output-format text

# Save to file
unstructured partition --filename example-docs/fake-text.txt --output example-output.json
```

### Processing Options
```bash
# Specify processing strategy
unstructured partition --filename example-docs/example-10k-1p.html --strategy hi_res

# Specify language
unstructured partition --filename example-docs/fake-text.txt --languages eng

# Process URL
unstructured partition --url https://example.com/document.pdf
```

The CLI provides a convenient way to process documents without writing Python code, making it ideal for quick experiments and batch processing.

**Section sources**
- [README.md](file://README.md#L55-L93)

## Common Patterns and Best Practices

### Extracting Clean Text
```python
from unstructured.partition.auto import partition

def extract_clean_text(filename):
    """Extract clean text from a document."""
    elements = partition(filename=filename)
    return "\n\n".join([str(el) for el in elements])

text = extract_clean_text("example-docs/fake-text.txt")
print(text)
```

### Processing Multiple Files
```python
import os
from unstructured.partition.auto import partition

def process_directory(directory_path):
    """Process all files in a directory."""
    results = {}
    for filename in os.listdir(directory_path):
        file_path = os.path.join(directory_path, filename)
        if os.path.isfile(file_path):
            try:
                elements = partition(filename=file_path)
                results[filename] = elements
            except Exception as e:
                print(f"Error processing {filename}: {e}")
    return results

# Process all example documents
results = process_directory("example-docs")
```

### Filtering and Transforming Elements
```python
from unstructured.partition.auto import partition

def get_narrative_content(filename):
    """Extract only narrative content from a document."""
    elements = partition(filename=filename)
    
    # Filter for narrative text and titles
    content_elements = [
        el for el in elements 
        if el.type in ["NarrativeText", "Title"]
    ]
    
    # Combine text with section headers
    content = []
    for element in content_elements:
        if element.type == "Title":
            content.append(f"\n## {element.text}\n")
        else:
            content.append(element.text)
    
    return "\n".join(content)

narrative_content = get_narrative_content("example-docs/example-10k-1p.html")
```

### Working with Tables
```python
def extract_tables(filename):
    """Extract tables and their HTML representation."""
    elements = partition(filename=filename)
    tables = [el for el in elements if el.type == "Table"]
    
    table_data = []
    for table in tables:
        table_info = {
            "text": table.text,
            "html": table.metadata.text_as_html,
            "page": table.metadata.page_number
        }
        table_data.append(table_info)
    
    return table_data
```

These patterns demonstrate common use cases and provide a foundation for building more complex document processing workflows.

**Section sources**
- [auto.py](file://unstructured/partition/auto.py#L291-L293)
- [elements.py](file://unstructured/documents/elements.py#L149-L300)

## Troubleshooting Common Issues

### File Path Errors
```python
from unstructured.partition.auto import partition
import os

# Check if file exists
filename = "example-docs/fake-text.txt"
if not os.path.exists(filename):
    print(f"File not found: {filename}")
else:
    elements = partition(filename=filename)
```

### Unsupported File Formats
```python
from unstructured.partition.auto import partition
from unstructured.documents.elements import UnsupportedFileFormatError

try:
    elements = partition(filename="example-docs/unsupported/factbook.xsl")
except UnsupportedFileFormatError as e:
    print(f"Unsupported file format: {e}")
except Exception as e:
    print(f"Error processing file: {e}")
```

### Missing Dependencies
If you encounter import errors, you may need to install additional dependencies:

```python
# For PDF processing
pip install "unstructured[pdf]"

# For image processing
pip install "unstructured[images]"

# For Office documents
pip install "unstructured[docx,pptx,xlsx]"
```

### Handling Large Files
For very large documents, consider processing in chunks or using memory-efficient approaches:

```python
def process_large_file(filename, chunk_size=1000):
    """Process a large file in chunks."""
    elements = partition(filename=filename)
    
    # Process elements in chunks
    for i in range(0, len(elements), chunk_size):
        chunk = elements[i:i + chunk_size]
        # Process chunk
        yield chunk
```

### Debugging Tips
1. Verify file paths are correct
2. Check that required system dependencies are installed
3. Use the `--verbose` flag with the CLI for detailed output
4. Start with simple documents before processing complex ones
5. Check the library's logging output for warnings and errors

**Section sources**
- [auto.py](file://unstructured/partition/auto.py#L356-L359)
- [README.md](file://README.md#L241-L243)

## Performance Tips

### Choose the Right Strategy
The unstructured library offers different processing strategies with varying performance characteristics:

```python
from unstructured.partition.auto import partition
from unstructured.partition.utils.constants import PartitionStrategy

# Fast strategy - quick but less accurate
elements = partition(filename="document.pdf", strategy=PartitionStrategy.FAST)

# Hi-res strategy - slower but more accurate
elements = partition(filename="document.pdf", strategy=PartitionStrategy.HI_RES)

# Auto strategy - balances speed and accuracy
elements = partition(filename="document.pdf", strategy=PartitionStrategy.AUTO)
```

### Optimize for Your Use Case
```python
# For text extraction only, disable table inference
elements = partition(
    filename="document.pdf", 
    strategy="fast",
    skip_infer_table_types=["pdf"]
)

# For multilingual documents, specify languages
elements = partition(
    filename="document.pdf",
    languages=["eng", "spa"]
)
```

### Batch Processing
```python
def process_files_concurrently(filenames):
    """Process multiple files concurrently."""
    from concurrent.futures import ThreadPoolExecutor
    from unstructured.partition.auto import partition
    
    def process_single_file(filename):
        try:
            return partition(filename=filename)
        except Exception as e:
            return f"Error processing {filename}: {e}"
    
    with ThreadPoolExecutor(max_workers=4) as executor:
        results = list(executor.map(process_single_file, filenames))
    
    return results
```

### Memory Management
```python
# For large documents, process and save incrementally
def process_and_save(filename, output_file):
    """Process and save results incrementally."""
    elements = partition(filename=filename)
    
    with open(output_file, 'w') as f:
        for element in elements:
            f.write(f"{element.type}: {element.text}\n")
```

### Caching Results
```python
import json
import hashlib

def cached_partition(filename, cache_dir="cache"):
    """Cache partition results to avoid reprocessing."""
    # Create cache directory
    os.makedirs(cache_dir, exist_ok=True)
    
    # Generate cache key
    cache_key = hashlib.md5(filename.encode()).hexdigest()
    cache_file = os.path.join(cache_dir, f"{cache_key}.json")
    
    # Check if cached result exists
    if os.path.exists(cache_file):
        with open(cache_file, 'r') as f:
            return json.load(f)
    
    # Process and cache result
    elements = partition(filename=filename)
    with open(cache_file, 'w') as f:
        json.dump([el.to_dict() for el in elements], f)
    
    return elements
```

These performance tips will help you optimize your document processing workflows for speed and efficiency.

**Section sources**
- [auto.py](file://unstructured/partition/auto.py#L40-L45)
- [constants.py](file://unstructured/partition/utils/constants.py#L17-L21)