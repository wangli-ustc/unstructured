# Advanced Features

<cite>
**Referenced Files in This Document**   
- [base.py](file://unstructured/chunking/base.py)
- [basic.py](file://unstructured/chunking/basic.py)
- [title.py](file://unstructured/chunking/title.py)
- [interfaces.py](file://unstructured/embed/interfaces.py)
- [openai.py](file://unstructured/embed/openai.py)
- [vertexai.py](file://unstructured/embed/vertexai.py)
- [translate.py](file://unstructured/cleaners/translate.py)
- [form_extraction.py](file://unstructured/partition/pdf_image/form_extraction.py)
- [elements.py](file://unstructured/documents/elements.py)
- [html_table.py](file://unstructured/common/html_table.py)
- [test_lang.py](file://test_unstructured/partition/common/test_lang.py)
- [test_table_structure.py](file://test_unstructured/metrics/test_table_structure.py)
- [table_structure.py](file://unstructured/metrics/table_structure.py)
</cite>

## Table of Contents
1. [Document Chunking Strategies](#document-chunking-strategies)
2. [Embedding Integration](#embedding-integration)
3. [Table Structure Inference and Preservation](#table-structure-inference-and-preservation)
4. [Form Extraction](#form-extraction)
5. [Multi-language Support](#multi-language-support)
6. [Integration Challenges and Performance Considerations](#integration-challenges-and-performance-considerations)

## Document Chunking Strategies

The document chunking system provides multiple strategies for breaking down documents into manageable pieces for LLM workflows. The core chunking functionality is implemented in the `unstructured/chunking` module, with different strategies available for various use cases.

The base chunking implementation provides fundamental behaviors that are present across all chunking strategies:
- Maximally filling each chunk with sequential elements
- Isolating oversized elements and dividing them through text-splitting
- Providing overlap functionality for context preservation

Two primary chunking strategies are available: basic chunking and title-based chunking.

**Basic Chunking** is the "plain-vanilla" approach that combines sequential elements into chunks while respecting text-length limits. This strategy is implemented in `basic.py` and uses the `chunk_elements` function which accepts parameters like `max_characters`, `new_after_n_chars`, and `overlap` to control chunking behavior.

**Title-based Chunking** uses title elements to identify semantic sections within documents. This strategy respects section boundaries, ensuring that chunks do not contain text from multiple sections. It's particularly useful for documents with clear hierarchical structure. The `chunk_by_title` function in `title.py` implements this strategy and adds parameters like `combine_text_under_n_chars` and `multipage_sections` to control how sections are handled.

Key chunking parameters include:
- `max_characters`: Hard maximum chunk length
- `new_after_n_chars`: Soft maximum that triggers new chunk creation
- `overlap`: Number of characters to overlap between chunks for context preservation
- `overlap_all`: Whether to apply overlap between normal chunks
- `combine_text_under_n_chars`: Combines small chunks when space allows

The chunking system handles special cases like tables differently from regular text elements. Tables can be split while maintaining synchronized text and HTML representations, ensuring that table structure is preserved across chunk boundaries.

**Section sources**
- [base.py](file://unstructured/chunking/base.py)
- [basic.py](file://unstructured/chunking/basic.py)
- [title.py](file://unstructured/chunking/title.py)

## Embedding Integration

The embedding system provides interfaces for integrating with various embedding providers, allowing documents to be converted into vector representations for retrieval and similarity operations.

The embedding architecture is built around an interface-based design defined in `interfaces.py`. The `BaseEmbeddingEncoder` abstract base class establishes the contract that all embedding providers must implement, including methods for initialization, document embedding, and query embedding.

Currently supported embedding providers include:
- OpenAI
- VertexAI
- MixedbreadAI
- OctoAI
- VoyageAI
- HuggingFace
- Bedrock

Each provider is implemented as a separate module in the `unstructured/embed` directory. The implementation follows a consistent pattern where each provider has:
- A configuration class that inherits from `EmbeddingConfig`
- An encoder class that inherits from `BaseEmbeddingEncoder`
- Provider-specific initialization and authentication mechanisms

For example, the OpenAI integration in `openai.py` uses the `langchain_openai` library to create an OpenAI embeddings client. It supports configuration of the API key and model name (defaulting to "text-embedding-ada-002"). The implementation handles embedding both documents (lists of elements) and individual queries.

Similarly, the VertexAI integration in `vertexai.py` uses `langchain_google_vertexai` and handles Google Cloud authentication by writing credentials to a temporary file and setting the `GOOGLE_APPLICATION_CREDENTIALS` environment variable.

The embedding system is designed to be extensible, allowing new providers to be added by implementing the `BaseEmbeddingEncoder` interface. This modular design enables users to switch between different embedding providers based on their requirements, cost considerations, or performance characteristics.

**Section sources**
- [interfaces.py](file://unstructured/embed/interfaces.py)
- [openai.py](file://unstructured/embed/openai.py)
- [vertexai.py](file://unstructured/embed/vertexai.py)

## Table Structure Inference and Preservation

The system provides robust capabilities for inferring and preserving table structure across different document formats. This is particularly important for maintaining the semantic meaning of tabular data when processing documents.

Table structure inference is implemented in the `unstructured/metrics/table_structure.py` module. The `image_or_pdf_to_dataframe` function converts images or PDFs containing tables into pandas DataFrames using the Table Transformer model from `unstructured_inference`. This function handles both image-based tables and PDFs by first converting PDFs to images and then applying OCR to extract table tokens.

The system preserves table structure through the `text_as_html` metadata field in Table elements. When a table is detected during document processing, its structure is captured as HTML and stored in this field. This allows the table to be reconstructed with its original formatting and structure, even when the table is split across multiple chunks.

The `html_table.py` module provides utilities for working with HTML table representations. The `HtmlTable` class can parse HTML table fragments, normalize them by removing attributes and noise elements like `<thead>` and `<tbody>`, and provide compact representations that maximize semantic content in limited space. This is particularly important for chunking, where space efficiency is critical.

When tables are split during chunking, the system ensures that text and HTML representations remain synchronized. For oversized tables, the `_TableChunker` class in `base.py` handles the splitting process, creating `TableChunk` elements that maintain both text and HTML representations of table segments.

The table evaluation system in `test_table_structure.py` provides metrics for assessing table structure detection accuracy, including table-level accuracy and element-level row and column index accuracy. This allows for quantitative evaluation of table parsing performance.

**Section sources**
- [html_table.py](file://unstructured/common/html_table.py)
- [table_structure.py](file://unstructured/metrics/table_structure.py)
- [test_table_structure.py](file://test_unstructured/metrics/test_table_structure.py)

## Form Extraction

Form extraction capabilities are designed to identify and extract key-value pairs from form-like structures in documents, particularly in PDFs and image documents.

The form extraction functionality is implemented in `unstructured/partition/pdf_image/form_extraction.py`. Currently, the `run_form_extraction` function is a placeholder that raises a `NotImplementedError`, indicating that form extraction is not yet available but the interface is defined.

The system defines a `FormKeysValues` type in `elements.py` to represent form data, with `FormKeyValuePair` defining the structure for key-value pairs including confidence scores. This suggests a design where form fields are identified with associated confidence levels, allowing downstream applications to handle uncertain extractions appropriately.

The form extraction system is designed to work with the existing element structure, where form fields would be represented as special types of elements with metadata indicating their role as form keys or values. The `key_value_pairs` field in `ElementMetadata` is specifically designed to store form data extracted from documents.

Future implementation of form extraction would likely leverage machine learning models trained on form data to identify field boundaries and relationships between keys and values. The system architecture appears to be designed to support various form extraction models while providing a consistent interface for accessing the extracted data.

**Section sources**
- [form_extraction.py](file://unstructured/partition/pdf_image/form_extraction.py)
- [elements.py](file://unstructured/documents/elements.py)

## Multi-language Support

The system provides comprehensive multi-language support for document processing, including language detection, text classification, and translation capabilities.

Language detection is implemented in the `detect_languages` function, which can automatically detect languages in text or accept user-specified languages. The system supports both ISO 639-2 and ISO 639-1 language codes, converting between them as needed. When multiple languages are detected in a single text, the system returns all detected languages.

The language detection system is integrated with OCR processing through the `prepare_languages_for_tesseract` function, which converts language codes to the format expected by Tesseract OCR. This function handles multiple languages by joining them with plus signs (e.g., "eng+spa") and includes support for language variants (e.g., "chi_sim+chi_sim_vert+chi_tra+chi_tra_vert" for Chinese).

Text classification functions like `is_possible_narrative_text` and `is_possible_title` are language-aware and can handle multi-language text. These functions use language-specific rules to determine text type, improving accuracy for non-English content.

Translation capabilities are provided through the `translate.py` module, which uses the Helsinki-NLP/opus-mt models from Hugging Face Transformers. The `translate_text` function can translate text between languages, with automatic language detection when the source language is not specified. The system normalizes Chinese language codes (zh-cn, zh-tw, zh-hk) to "zh" to handle different Chinese variants with a single model.

The multi-language system is designed to handle documents containing multiple languages, with metadata fields like `languages` in `ElementMetadata` storing all detected languages for each element. This allows downstream applications to handle multi-lingual content appropriately, such as applying language-specific processing rules or routing content to appropriate translation services.

**Section sources**
- [translate.py](file://unstructured/cleaners/translate.py)
- [elements.py](file://unstructured/documents/elements.py)
- [test_lang.py](file://test_unstructured/partition/common/test_lang.py)

## Integration Challenges and Performance Considerations

Implementing advanced document processing features presents several integration challenges and performance considerations that must be addressed for optimal system operation.

**Chunking Performance**: The chunking process must balance thoroughness with efficiency. Large documents with complex structures can generate many elements, making the chunking process computationally intensive. The system addresses this by using lazy evaluation and generators where possible, minimizing memory usage. However, users should be aware that documents with many small elements may result in a large number of chunks, potentially impacting downstream processing performance.

**Embedding Latency**: External embedding services introduce network latency and potential rate limiting. The system should implement caching mechanisms to avoid re-embedding identical content and should handle API rate limits gracefully. For high-throughput applications, consider using local embedding models or batching requests to minimize round-trip times.

**Table Processing Complexity**: Table structure inference is computationally expensive, particularly for image-based tables that require both OCR and structure detection. The Table Transformer model requires significant GPU resources for optimal performance. For CPU-only environments, consider using simpler table detection methods or preprocessing documents to extract tables before processing.

**Form Extraction Limitations**: As form extraction is not yet implemented, this represents a gap in functionality for users needing to process forms. The placeholder implementation suggests that when implemented, form extraction will likely be resource-intensive, requiring specialized models trained on form data.

**Multi-language Processing Overhead**: Language detection and translation add processing overhead. Automatic language detection requires analyzing text content, while translation involves calling external services or running large ML models. For documents known to be in a specific language, explicitly specifying the language can improve performance by skipping detection.

**Memory Management**: The system should be monitored for memory usage, particularly when processing large documents or batches of documents. Elements and their metadata can consume significant memory, especially when including original elements in chunk metadata. Consider processing documents in smaller batches or using streaming processing when memory is constrained.

**Error Handling**: Integration with external services (embedding providers, translation services) requires robust error handling for network failures, authentication issues, and service outages. Implement retry logic with exponential backoff and provide clear error messages to aid troubleshooting.

**Scalability**: For large-scale document processing, consider distributed processing architectures where different documents or batches can be processed in parallel. The modular design of the system supports this approach, allowing different components to be scaled independently based on workload characteristics.

**Configuration Management**: With multiple advanced features, proper configuration management is essential. Users should carefully tune parameters like chunk sizes, overlap, and language settings based on their specific use cases and document characteristics to achieve optimal results.

**Section sources**
- [base.py](file://unstructured/chunking/base.py)
- [openai.py](file://unstructured/embed/openai.py)
- [translate.py](file://unstructured/cleaners/translate.py)
- [table_structure.py](file://unstructured/metrics/table_structure.py)