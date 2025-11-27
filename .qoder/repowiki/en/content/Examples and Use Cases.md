# Examples and Use Cases

<cite>
**Referenced Files in This Document**   
- [partition/auto.py](file://unstructured/partition/auto.py)
- [partition/pdf.py](file://unstructured/partition/pdf.py)
- [partition/text.py](file://unstructured/partition/text.py)
- [documents/elements.py](file://unstructured/documents/elements.py)
- [example-docs/fake-text.txt](file://example-docs/fake-text.txt)
- [example-docs/fake-email.txt](file://example-docs/fake-email.txt)
- [example-docs/simple-table.md](file://example-docs/simple-table.md)
- [example-docs/book-war-and-peace-1p.txt](file://example-docs/book-war-and-peace-1p.txt)
- [example-docs/language-docs/UDHR_first_article_all.txt](file://example-docs/language-docs/UDHR_first_article_all.txt)
- [example-docs/example-10k-1p.html](file://example-docs/example-10k-1p.html)
- [scripts/convert/elements_json_to_format.py](file://scripts/convert/elements_json_to_format.py)
- [scripts/performance/run_partition.py](file://scripts/performance/run_partition.py)
</cite>

## Table of Contents
1. [Introduction](#introduction)
2. [LLM Preprocessing Workflows](#llm-preprocessing-workflows)
3. [Document Analysis Applications](#document-analysis-applications)
4. [Content Migration Scenarios](#content-migration-scenarios)
5. [Industry-Specific Use Cases](#industry-specific-use-cases)
6. [Performance Considerations](#performance-considerations)
7. [Conclusion](#conclusion)

## Introduction
The unstructured library provides comprehensive tools for preprocessing unstructured data for various applications including LLM workflows, document analysis, and content migration. This document showcases practical examples and use cases demonstrating the library's capabilities across different document types and industry scenarios. The library supports a wide range of file formats including PDFs, text documents, emails, HTML, and various office document formats, making it versatile for diverse data processing needs.

The core functionality revolves around the partitioning process, which extracts and structures content from documents into meaningful elements. These elements can then be used for downstream applications such as information retrieval, content analysis, and data migration. The library's modular design allows for flexible integration into existing workflows and supports both simple and complex document processing requirements.

**Section sources**
- [partition/auto.py](file://unstructured/partition/auto.py#L1-L384)
- [README.md](file://README.md#L1-L256)

## LLM Preprocessing Workflows

### Text Document Processing
For LLM preprocessing, the unstructured library excels at processing plain text documents. Using the `partition_text` function, the library can parse text files and identify different content elements such as titles, paragraphs, lists, and addresses. This structured output is ideal for preparing training data or creating knowledge bases for LLMs.

```python
from unstructured.partition.text import partition_text

elements = partition_text(filename="example-docs/fake-text.txt")
print("\n\n".join([str(el) for el in elements]))
```

This workflow processes the sample text document and identifies elements like the address "Doylestown, PA 18901" and bulleted list items. The output elements contain metadata including the source filename and text content, making them ready for vectorization and ingestion into LLM applications.

### Email Processing for LLM Training
Emails are a common source of training data for LLMs, particularly for conversational AI applications. The library can process both plain text and HTML email formats, extracting the relevant content while preserving the structure.

```python
from unstructured.partition.email import partition_email

elements = partition_email(filename="example-docs/fake-email.txt")
```

The partitioning process handles MIME-encoded emails, extracting text from both plain text and HTML parts while maintaining the semantic structure. This is particularly useful for creating dialogue datasets where the conversational flow needs to be preserved.

### Multilingual Content Processing
The library supports processing multilingual content, which is essential for training LLMs that need to understand and generate text in multiple languages. The sample document `UDHR_first_article_all.txt` contains the first article of the Universal Declaration of Human Rights in over 300 languages.

```python
from unstructured.partition.text import partition_text

elements = partition_text(filename="example-docs/language-docs/UDHR_first_article_all.txt")
```

This capability allows organizations to build multilingual knowledge bases and train LLMs on diverse linguistic data, improving their performance across different language contexts.

**Section sources**
- [partition/text.py](file://unstructured/partition/text.py#L1-L217)
- [example-docs/fake-text.txt](file://example-docs/fake-text.txt#L1-L10)
- [example-docs/fake-email.txt](file://example-docs/fake-email.txt#L1-L25)
- [example-docs/language-docs/UDHR_first_article_all.txt](file://example-docs/language-docs/UDHR_first_article_all.txt#L1-L800)

## Document Analysis Applications

### Financial Report Analysis
Financial reports often contain complex layouts with tables, charts, and narrative text. The unstructured library can process these documents and extract structured data for analysis. For financial statements like the sample 10-K filing, the library can identify key sections and extract tabular data.

```python
from unstructured.partition.pdf import partition_pdf

elements = partition_pdf(
    filename="example-docs/example-10k-1p.html",
    strategy="hi_res",
    infer_table_structure=True
)
```

The `infer_table_structure` parameter enables the extraction of table data in HTML format, preserving the row and column structure. This is particularly valuable for financial analysts who need to extract and analyze numerical data from annual reports, quarterly filings, and other financial documents.

### Scientific Paper Processing
Scientific papers present unique challenges with their complex layouts, mathematical formulas, and reference sections. The library's high-resolution partitioning strategy uses layout detection models to identify different document elements accurately.

```python
elements = partition_pdf(
    filename="example-docs/layout-parser-paper.pdf",
    strategy="hi_res",
    languages=["eng"]
)
```

This approach can identify and separate abstracts, section headings, figures, captions, and references, creating a structured representation of the paper that can be used for literature reviews, citation analysis, or building scientific knowledge bases.

### Table Extraction and Analysis
Tables are a critical component of many documents, and the unstructured library provides robust table extraction capabilities. The library can convert tables into structured formats that can be easily analyzed or loaded into databases.

```python
from unstructured.partition.md import partition_md

elements = partition_md(
    filename="example-docs/simple-table.md",
    infer_table_structure=True
)
```

For the simple markdown table in the example, the library extracts the table structure and can represent it in HTML format, making it suitable for web-based applications or further processing in data analysis pipelines.

**Section sources**
- [partition/pdf.py](file://unstructured/partition/pdf.py#L1-L800)
- [partition/md.py](file://unstructured/partition/md.py#L1-L100)
- [example-docs/example-10k-1p.html](file://example-docs/example-10k-1p.html#L1-L50)
- [example-docs/simple-table.md](file://example-docs/simple-table.md#L1-L4)

## Content Migration Scenarios

### Book Digitization
Migrating printed books to digital formats requires careful handling of chapter breaks, section headings, and pagination. The library can process lengthy texts like "War and Peace" and maintain the document structure during conversion.

```python
elements = partition_text(filename="example-docs/book-war-and-peace-1p.txt")
```

This workflow preserves chapter headings and paragraph structure, making it suitable for creating e-books or digital archives. The extracted elements can be reassembled into various digital formats while maintaining the original document's organization.

### Website Content Migration
Migrating content from websites to other platforms often involves extracting text from HTML while preserving the semantic structure. The library can process HTML documents and identify different content elements.

```python
from unstructured.partition.html import partition_html

elements = partition_html(filename="example-docs/ideas-page.html")
```

This approach is useful for content management system migrations, where HTML content needs to be extracted and reformatted for a new platform while maintaining the original structure and hierarchy.

### Document Format Conversion
The library supports converting documents between various formats, which is essential for content migration projects. Using the provided conversion scripts, users can transform structured elements into different output formats.

```python
# Convert JSON elements to HTML
python scripts/convert/elements_json_to_format.py \
  --format html \
  --outdir output_dir \
  input_elements.json
```

This capability enables organizations to modernize their document repositories, converting legacy formats to modern, accessible formats while preserving the content structure and metadata.

**Section sources**
- [partition/html.py](file://unstructured/partition/html.py#L1-L100)
- [scripts/convert/elements_json_to_format.py](file://scripts/convert/elements_json_to_format.py#L1-L100)
- [example-docs/book-war-and-peace-1p.txt](file://example-docs/book-war-and-peace-1p.txt#L1-L63)
- [example-docs/ideas-page.html](file://example-docs/ideas-page.html#L1-L20)

## Industry-Specific Use Cases

### Legal Document Processing
Legal documents often contain complex formatting, footnotes, and cross-references. The unstructured library can process legal contracts, court filings, and regulatory documents, extracting key clauses and provisions.

```python
elements = partition_pdf(
    filename="legal_document.pdf",
    strategy="hi_res",
    pdf_infer_table_structure=True
)
```

This capability supports legal technology applications such as contract analysis, due diligence, and compliance monitoring, where extracting specific provisions from lengthy documents is critical.

### Medical Record Analysis
Medical records contain sensitive information in structured and unstructured formats. The library can process clinical notes, lab reports, and patient histories, extracting relevant information while maintaining patient privacy.

```python
elements = partition_text(
    filename="medical_record.txt",
    paragraph_grouper=None
)
```

Healthcare organizations can use this capability for clinical decision support, research, and quality improvement initiatives, transforming unstructured clinical text into structured data for analysis.

### Scientific Research Data Extraction
Research papers and technical reports often contain valuable data in tables and figures. The library's ability to extract and structure this information supports data mining and knowledge discovery in scientific domains.

```python
elements = partition_pdf(
    filename="research_paper.pdf",
    strategy="hi_res",
    extract_images_in_pdf=True,
    infer_table_structure=True
)
```

This use case enables researchers to build comprehensive literature databases, perform meta-analyses, and identify trends across multiple studies by extracting structured data from unstructured sources.

**Section sources**
- [partition/pdf.py](file://unstructured/partition/pdf.py#L126-L249)
- [partition/text.py](file://unstructured/partition/text.py#L40-L51)

## Performance Considerations

### Processing Large Documents
When processing large documents like books or extensive reports, performance optimization becomes critical. The library provides several strategies for handling large files efficiently.

```python
# Use fast strategy for text-extractable PDFs
elements = partition_pdf(
    filename="large_document.pdf",
    strategy="fast"
)
```

The "fast" strategy extracts text directly from PDFs when the text is extractable, significantly reducing processing time compared to OCR-based methods. For documents where text extraction is not possible, the "hi_res" strategy with layout detection provides more accurate results but requires more computational resources.

### Batch Processing Workflows
For processing multiple documents, batch processing workflows can optimize resource utilization and throughput.

```python
# Process multiple files
import os
from unstructured.partition.auto import partition

documents_dir = "path/to/documents"
all_elements = []

for filename in os.listdir(documents_dir):
    file_path = os.path.join(documents_dir, filename)
    elements = partition(filename=file_path)
    all_elements.extend(elements)
```

This approach allows organizations to process document collections efficiently, making it suitable for enterprise-scale content processing operations.

### Resource Management
The library's performance can be tuned based on available system resources. For environments with limited memory, processing documents in smaller batches or using simpler partitioning strategies can prevent resource exhaustion.

```python
# Monitor performance
import time
from scripts.performance.run_partition import run_partition

start_time = time.time()
result = run_partition("document.pdf", "hi_res")
processing_time = time.time() - start_time
```

Understanding the performance characteristics of different partitioning strategies helps organizations plan their infrastructure and optimize processing workflows for their specific use cases and scale requirements.

**Section sources**
- [scripts/performance/run_partition.py](file://scripts/performance/run_partition.py#L1-L20)
- [partition/pdf.py](file://unstructured/partition/pdf.py#L126-L249)

## Conclusion
The unstructured library provides a comprehensive solution for processing diverse document types across various applications. From LLM preprocessing to industry-specific document analysis, the library's capabilities support a wide range of use cases. By transforming unstructured content into structured elements, the library enables organizations to unlock the value in their document repositories and integrate this information into modern data workflows. The examples and use cases demonstrated in this document illustrate the library's versatility and power in handling real-world document processing challenges.