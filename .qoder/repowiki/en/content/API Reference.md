# API Reference

<cite>
**Referenced Files in This Document**   
- [__init__.py](file://unstructured/__init__.py)
- [partition/api.py](file://unstructured/partition/api.py)
- [documents/elements.py](file://unstructured/documents/elements.py)
- [chunking/__init__.py](file://unstructured/chunking/__init__.py)
- [chunking/basic.py](file://unstructured/chunking/basic.py)
- [chunking/dispatch.py](file://unstructured/chunking/dispatch.py)
- [chunking/base.py](file://unstructured/chunking/base.py)
- [embed/__init__.py](file://unstructured/embed/__init__.py)
- [errors.py](file://unstructured/errors.py)
- [partition/text.py](file://unstructured/partition/text.py)
- [partition/pdf.py](file://unstructured/partition/pdf.py)
- [staging/base.py](file://unstructured/staging/base.py)
- [documents/mappings.py](file://unstructured/documents/mappings.py)
- [logger.py](file://unstructured/logger.py)
</cite>

## Table of Contents
1. [Introduction](#introduction)
2. [Module Structure and Import Paths](#module-structure-and-import-paths)
3. [Element Classes](#element-classes)
4. [Partitioning Functions](#partitioning-functions)
5. [Chunking Utilities](#chunking-utilities)
6. [Embedding Interfaces](#embedding-interfaces)
7. [Error Handling](#error-handling)
8. [Utility Functions](#utility-functions)
9. [Versioning and Backward Compatibility](#versioning-and-backward-compatibility)
10. [Performance Recommendations](#performance-recommendations)

## Introduction
The unstructured library provides a comprehensive API for processing and analyzing unstructured documents such as PDFs, text files, and images. This API reference documents all public modules, classes, functions, parameters, return values, and exceptions. The library focuses on three main areas: document partitioning, element classification, and text chunking. It supports various document formats and provides utilities for metadata extraction, coordinate system management, and content transformation. The API is designed to be extensible and configurable, allowing users to customize processing strategies based on their specific requirements.

## Module Structure and Import Paths
The unstructured library follows a modular structure with clearly defined import paths for different functionality areas. The main modules are organized under the `unstructured` package, with submodules for specific capabilities.

```mermaid
graph TD
unstructured[unstructured] --> partition[partition]
unstructured --> documents[documents]
unstructured --> chunking[chunking]
unstructured --> embed[embed]
unstructured --> file_utils[file_utils]
unstructured --> cleaners[cleaners]
unstructured --> nlp[nlp]
unstructured --> staging[staging]
unstructured --> utils[utils]
partition --> api[api]
partition --> text[text]
partition --> pdf[pdf]
documents --> elements[elements]
documents --> coordinates[coordinates]
chunking --> basic[basic]
chunking --> dispatch[dispatch]
chunking --> base[base]
embed --> __init__[__init__]
embed --> openai[openai]
embed --> huggingface[huggingface]
```

**Diagram sources** 
- [__init__.py](file://unstructured/__init__.py)
- [partition/__init__.py](file://unstructured/partition/__init__.py)
- [documents/__init__.py](file://unstructured/documents/__init__.py)
- [chunking/__init__.py](file://unstructured/chunking/__init__.py)
- [embed/__init__.py](file://unstructured/embed/__init__.py)

## Element Classes
The unstructured library defines a hierarchy of element classes that represent different types of content extracted from documents. These classes inherit from the base `Element` class and provide specific functionality for different content types.

### Element Base Class
The `Element` class is the base class for all document elements. It provides common functionality for element identification, metadata management, and coordinate system conversion.

```mermaid
classDiagram
class Element {
+str text
+str category
+str id
+ElementMetadata metadata
+__init__(element_id : Optional[str], coordinates : Optional[tuple[tuple[float, float], ...]], coordinate_system : Optional[CoordinateSystem], metadata : Optional[ElementMetadata], detection_origin : Optional[str])
+convert_coordinates_to_new_system(new_system : CoordinateSystem, in_place : bool = True) -> Optional[Points]
+id_to_hash(sequence_number : int) -> str
+to_dict() -> dict[str, Any]
}
class Text {
+__init__(text : str, element_id : Optional[str] = None, coordinates : Optional[tuple[tuple[float, float], ...]] = None, coordinate_system : Optional[CoordinateSystem] = None, metadata : Optional[ElementMetadata] = None, detection_origin : Optional[str] = None)
}
class Title {
+__init__(text : str, element_id : Optional[str] = None, coordinates : Optional[tuple[tuple[float, float], ...]] = None, coordinate_system : Optional[CoordinateSystem] = None, metadata : Optional[ElementMetadata] = None, detection_origin : Optional[str] = None)
}
class NarrativeText {
+__init__(text : str, element_id : Optional[str] = None, coordinates : Optional[tuple[tuple[float, float], ...]] = None, coordinate_system : Optional[CoordinateSystem] = None, metadata : Optional[ElementMetadata] = None, detection_origin : Optional[str] = None)
}
class Table {
+__init__(text : str, element_id : Optional[str] = None, coordinates : Optional[tuple[tuple[float, float], ...]] = None, coordinate_system : Optional[CoordinateSystem] = None, metadata : Optional[ElementMetadata] = None, detection_origin : Optional[str] = None)
}
class Image {
+__init__(text : str, element_id : Optional[str] = None, coordinates : Optional[tuple[tuple[float, float], ...]] = None, coordinate_system : Optional[CoordinateSystem] = None, metadata : Optional[ElementMetadata] = None, detection_origin : Optional[str] = None)
}
class CheckBox {
+bool checked
+__init__(element_id : Optional[str] = None, coordinates : Optional[tuple[tuple[float, float], ...]] = None, coordinate_system : Optional[CoordinateSystem] = None, checked : bool = False, metadata : Optional[ElementMetadata] = None, detection_origin : Optional[str] = None)
}
Element <|-- Text
Element <|-- Title
Element <|-- NarrativeText
Element <|-- Table
Element <|-- Image
Element <|-- CheckBox
```

**Diagram sources** 
- [documents/elements.py](file://unstructured/documents/elements.py#L662-L800)

### Element Metadata
The `ElementMetadata` class provides a flexible system for storing metadata associated with document elements. It supports various metadata fields and consolidation strategies for chunking operations.

```mermaid
classDiagram
class ElementMetadata {
+Optional[str] attached_to_filename
+Optional[list[str]] bcc_recipient
+Optional[int] category_depth
+Optional[list[str]] cc_recipient
+Optional[CoordinatesMetadata] coordinates
+Optional[DataSourceMetadata] data_source
+Optional[float] detection_class_prob
+Optional[str] detection_origin
+Optional[list[str]] emphasized_text_contents
+Optional[list[str]] emphasized_text_tags
+Optional[str] file_directory
+Optional[str] filename
+Optional[str] filetype
+Optional[str] header_footer_type
+Optional[str] image_base64
+Optional[str] image_mime_type
+Optional[str] image_url
+Optional[str] image_path
+Optional[bool] is_continuation
+Optional[list[str]] languages
+Optional[str] last_modified
+Optional[list[str]] link_texts
+Optional[list[str]] link_urls
+Optional[list[int]] link_start_indexes
+Optional[list[Link]] links
+Optional[str] email_message_id
+Optional[list[Element]] orig_elements
+Optional[str] page_name
+Optional[int] page_number
+Optional[str] parent_id
+Optional[list[str]] sent_from
+Optional[list[str]] sent_to
+Optional[str] signature
+Optional[str] subject
+Optional[str] text_as_html
+Optional[dict[str, str | int]] table_as_cells
+Optional[str] url
+Optional[list[FormKeyValuePair]] key_value_pairs
+FrozenSet[str] DEBUG_FIELD_NAMES
+__init__(attached_to_filename : Optional[str] = None, bcc_recipient : Optional[list[str]] = None, category_depth : Optional[int] = None, cc_recipient : Optional[list[str]] = None, coordinates : Optional[CoordinatesMetadata] = None, data_source : Optional[DataSourceMetadata] = None, detection_class_prob : Optional[float] = None, emphasized_text_contents : Optional[list[str]] = None, emphasized_text_tags : Optional[list[str]] = None, file_directory : Optional[str] = None, filename : Optional[str | pathlib.Path] = None, filetype : Optional[str] = None, header_footer_type : Optional[str] = None, image_base64 : Optional[str] = None, image_mime_type : Optional[str] = None, image_url : Optional[str] = None, image_path : Optional[str] = None, is_continuation : Optional[bool] = None, languages : Optional[list[str]] = None, last_modified : Optional[str] = None, link_start_indexes : Optional[list[int]] = None, link_texts : Optional[list[str]] = None, link_urls : Optional[list[str]] = None, links : Optional[list[Link]] = None, email_message_id : Optional[str] = None, orig_elements : Optional[list[Element]] = None, page_name : Optional[str] = None, page_number : Optional[int] = None, parent_id : Optional[str] = None, sent_from : Optional[list[str]] = None, sent_to : Optional[list[str]] = None, signature : Optional[str] = None, subject : Optional[str] = None, table_as_cells : Optional[dict[str, str | int]] = None, text_as_html : Optional[str] = None, url : Optional[str] = None)
+__eq__(other : object) -> bool
+__getattr__(attr_name : str) -> None
+__setattr__(__name : str, __value : Any) -> None
+from_dict(meta_dict : dict[str, Any]) -> ElementMetadata
+fields() -> MappingProxyType[str, Any]
+known_fields() -> MappingProxyType[str, Any]
+to_dict() -> dict[str, Any]
+update(other : ElementMetadata) -> None
}
class DataSourceMetadata {
+Optional[str] url
+Optional[str] version
+Optional[dict[str, Any]] record_locator
+Optional[str] date_created
+Optional[str] date_modified
+Optional[str] date_processed
+Optional[list[dict[str, Any]]] permissions_data
+to_dict() -> dict[str, Any]
+from_dict(input_dict : dict[str, Any]) -> DataSourceMetadata
}
class CoordinatesMetadata {
+Optional[Points] points
+Optional[CoordinateSystem] system
+__init__(points : Optional[Points], system : Optional[CoordinateSystem])
+__eq__(other : Any) -> bool
+to_dict() -> dict[str, Any]
+from_dict(input_dict : dict[str, Any]) -> CoordinatesMetadata
}
class Link {
+Optional[str] text
+str url
+int start_index
}
class FormKeyOrValue {
+str text
+Optional[str] layout_element_id
+Optional[Text] custom_element
}
class FormKeyValuePair {
+FormKeyOrValue key
+Optional[FormKeyOrValue] value
+float confidence
}
ElementMetadata <|-- DataSourceMetadata
ElementMetadata <|-- CoordinatesMetadata
ElementMetadata <|-- Link
ElementMetadata <|-- FormKeyOrValue
ElementMetadata <|-- FormKeyValuePair
```

**Diagram sources** 
- [documents/elements.py](file://unstructured/documents/elements.py#L30-L523)

### Element Type Enum
The `ElementType` class defines constants for different types of document elements. These constants are used to categorize elements and determine their processing behavior.

```mermaid
classDiagram
class ElementType {
+str TITLE
+str TEXT
+str UNCATEGORIZED_TEXT
+str NARRATIVE_TEXT
+str BULLETED_TEXT
+str PARAGRAPH
+str ABSTRACT
+str THREADING
+str FORM
+str FIELD_NAME
+str VALUE
+str LINK
+str COMPOSITE_ELEMENT
+str IMAGE
+str PICTURE
+str FIGURE_CAPTION
+str FIGURE
+str CAPTION
+str LIST
+str LIST_ITEM
+str LIST_ITEM_OTHER
+str CHECKED
+str UNCHECKED
+str CHECK_BOX_CHECKED
+str CHECK_BOX_UNCHECKED
+str RADIO_BUTTON_CHECKED
+str RADIO_BUTTON_UNCHECKED
+str ADDRESS
+str EMAIL_ADDRESS
+str PAGE_BREAK
+str FORMULA
+str TABLE
+str HEADER
+str HEADLINE
+str SUB_HEADLINE
+str PAGE_HEADER
+str SECTION_HEADER
+str FOOTER
+str FOOTNOTE
+str PAGE_FOOTER
+str PAGE_NUMBER
+str CODE_SNIPPET
+str FORM_KEYS_VALUES
+str DOCUMENT_DATA
+to_dict() -> dict
}
```

**Diagram sources** 
- [documents/elements.py](file://unstructured/documents/elements.py#L601-L660)

## Partitioning Functions
The partitioning functions in the unstructured library are responsible for parsing documents and extracting structured elements from unstructured content. These functions support various document formats and processing strategies.

### API Partitioning Functions
The `partition_via_api` and `partition_multiple_via_api` functions provide interfaces to the Unstructured REST API for document processing.

```mermaid
classDiagram
class partition_via_api {
+partition_via_api(filename : Optional[str] = None, content_type : Optional[str] = None, file : Optional[IO[bytes]] = None, file_filename : Optional[str] = None, api_url : str = "https : //api.unstructured.io/general/v0/general", api_key : str = "", metadata_filename : Optional[str] = None, retries_initial_interval : [int] = None, retries_max_interval : Optional[int] = None, retries_exponent : Optional[float] = None, retries_max_elapsed_time : Optional[int] = None, retries_connection_errors : Optional[bool] = None, **request_kwargs : Any) -> list[Element]
}
class partition_multiple_via_api {
+partition_multiple_via_api(filenames : Optional[list[str]] = None, content_types : Optional[list[str]] = None, files : Optional[Sequence[IO[bytes]]] = None, file_filenames : Optional[list[str]] = None, api_url : str = "https : //api.unstructured.io/general/v0/general", api_key : str = "", metadata_filenames : Optional[list[str]] = None, **request_kwargs : Any) -> list[list[Element]]
}
class get_retries_config {
+get_retries_config(retries_connection_errors : Optional[bool], retries_exponent : Optional[float], retries_initial_interval : Optional[int], retries_max_elapsed_time : Optional[int], retries_max_interval : Optional[int], sdk : UnstructuredClient) -> Optional[retries.RetryConfig]
}
```

**Diagram sources** 
- [partition/api.py](file://unstructured/partition/api.py#L24-L342)

### Text Partitioning Function
The `partition_text` function processes plain text documents and extracts semantic elements such as titles, paragraphs, and lists.

```mermaid
classDiagram
class partition_text {
+partition_text(filename : str | None = None, file : IO[bytes] | None = None, encoding : str | None = None, text : str | None = None, paragraph_grouper : Callable[[str], str] | Literal[False] | None = None, detection_origin : str | None = "text", **kwargs : Any) -> list[Element]
}
class element_from_text {
+element_from_text(text : str, coordinates : tuple[tuple[float, float], ...] | None = None, coordinate_system : CoordinateSystem | None = None) -> Element
}
```

**Diagram sources** 
- [partition/text.py](file://unstructured/partition/text.py#L42-L110)

### PDF Partitioning Function
The `partition_pdf` function processes PDF documents using various strategies including hi-res, OCR-only, and fast mode.

```mermaid
classDiagram
class partition_pdf {
+partition_pdf(filename : Optional[str] = None, file : Optional[IO[bytes]] = None, include_page_breaks : bool = False, strategy : str = PartitionStrategy.AUTO, infer_table_structure : bool = False, ocr_languages : Optional[str] = None, languages : Optional[list[str]] = None, detect_language_per_element : bool = False, metadata_last_modified : Optional[str] = None, chunking_strategy : Optional[str] = None, hi_res_model_name : Optional[str] = None, extract_images_in_pdf : bool = False, extract_image_block_types : Optional[list[str]] = None, extract_image_block_output_dir : Optional[str] = None, extract_image_block_to_payload : bool = False, starting_page_number : int = 1, extract_forms : bool = False, form_extraction_skip_tables : bool = True, password : Optional[str] = None, pdfminer_line_margin : Optional[float] = None, pdfminer_char_margin : Optional[float] = None, pdfminer_line_overlap : Optional[float] = None, pdfminer_word_margin : Optional[float] = 0.185, **kwargs : Any) -> list[Element]
}
class partition_pdf_or_image {
+partition_pdf_or_image(filename : str = "", file : Optional[bytes | IO[bytes]] = None, is_image : bool = False, include_page_breaks : bool = False, strategy : str = PartitionStrategy.AUTO, infer_table_structure : bool = False, languages : Optional[list[str]] = None, detect_language_per_element : bool = False, metadata_last_modified : Optional[str] = None, hi_res_model_name : Optional[str] = None, extract_images_in_pdf : bool = False, extract_image_block_types : Optional[list[str]] = None, extract_image_block_output_dir : Optional[str] = None, extract_image_block_to_payload : bool = False, starting_page_number : int = 1, extract_forms : bool = False, form_extraction_skip_tables : bool = True, password : Optional[str] = None, pdfminer_line_margin : Optional[float] = None, pdfminer_char_margin : Optional[float] = None, pdfminer_line_overlap : Optional[float] = None, pdfminer_word_margin : Optional[float] = 0.185, ocr_agent : str = OCR_AGENT_TESSERACT, table_ocr_agent : str = OCR_AGENT_TESSERACT, **kwargs : Any) -> list[Element]
}
```

**Diagram sources** 
- [partition/pdf.py](file://unstructured/partition/pdf.py#L125-L248)

## Chunking Utilities
The chunking utilities in the unstructured library provide functionality for combining document elements into larger chunks for processing. These utilities support various chunking strategies and configuration options.

### Chunking Options
The `ChunkingOptions` class defines parameters for controlling the chunking behavior, including maximum chunk size, overlap, and text splitting.

```mermaid
classDiagram
class ChunkingOptions {
+__init__(**kwargs : Any)
+new(**kwargs : Any) -> Self
+boundary_predicates() -> tuple[BoundaryPredicate, ...]
+combine_text_under_n_chars() -> int
+hard_max() -> int
+include_orig_elements() -> bool
+inter_chunk_overlap() -> int
+overlap() -> int
+soft_max() -> int
+split() -> Callable[[str], tuple[str, str]]
+text_separator() -> str
+text_splitting_separators() -> tuple[str, ...]
+_validate() -> None
}
class PreChunker {
+__init__(elements : Iterable[Element], opts : ChunkingOptions)
+iter_pre_chunks(elements : Iterable[Element], opts : ChunkingOptions) -> Iterator[PreChunk]
+_iter_pre_chunks() -> Iterator[PreChunk]
+_boundary_predicates() -> tuple[BoundaryPredicate, ...]
+_is_in_new_semantic_unit(element : Element) -> bool
}
class PreChunkBuilder {
+__init__(opts : ChunkingOptions) -> None
+add_element(element : Element) -> None
+flush() -> Iterator[PreChunk]
+will_fit(element : Element) -> bool
+_remaining_space() -> int
+_reset_state(overlap_prefix : str) -> None
+_text_length() -> int
}
class PreChunk {
+__init__(elements : Iterable[Element], overlap_prefix : str, opts : ChunkingOptions) -> None
+__eq__(other : Any) -> bool
+can_combine(pre_chunk : PreChunk) -> bool
+combine(other_pre_chunk : PreChunk) -> PreChunk
+iter_chunks() -> Iterator[CompositeElement | Table | TableChunk]
+overlap_tail() -> str
+_iter_text_segments() -> Iterator[str]
+_text() -> str
}
class _Chunker {
+__init__(elements : Iterable[Element], text : str, opts : ChunkingOptions) -> None
+iter_chunks(elements : Iterable[Element], text : str, opts : ChunkingOptions) -> Iterator[CompositeElement]
+_iter_chunks() -> Iterator[CompositeElement]
+_all_metadata_values() -> dict[str, list[Any]]
+_consolidated_metadata() -> ElementMetadata
+_continuation_metadata() -> ElementMetadata
+_meta_kwargs() -> dict[str, Any]
+_orig_elements() -> list[Element]
}
class _TableChunker {
+__init__(table : Table, overlap_prefix : str, opts : ChunkingOptions) -> None
+iter_chunks(table : Table, overlap_prefix : str, opts : ChunkingOptions) -> Iterator[Table | TableChunk]
+_iter_chunks() -> Iterator[Table | TableChunk]
+_html() -> str
+_html_table() -> HtmlTable | None
+_iter_text_and_html_table_chunks() -> Iterator[TableChunk]
+_iter_text_only_table_chunks() -> Iterator[TableChunk]
}
```

**Diagram sources** 
- [chunking/base.py](file://unstructured/chunking/base.py#L54-L800)

### Basic Chunking Function
The `chunk_elements` function provides a baseline implementation of chunking that combines sequential elements into chunks based on specified text-length limits.

```mermaid
classDiagram
class chunk_elements {
+chunk_elements(elements : Iterable[Element], *, include_orig_elements : Optional[bool] = None, max_characters : Optional[int] = None, new_after_n_chars : Optional[int] = None, overlap : Optional[int] = None, overlap_all : Optional[bool] = None) -> list[Element]
}
class _chunk_elements {
+_chunk_elements(elements : Iterable[Element], opts : _BasicChunkingOptions) -> list[Element]
}
class _BasicChunkingOptions {
+_BasicChunkingOptions
}
```

**Diagram sources** 
- [chunking/basic.py](file://unstructured/chunking/basic.py#L24-L92)

### Chunking Dispatch
The chunking dispatch system provides a mechanism for registering and invoking different chunking strategies by name.

```mermaid
classDiagram
class Chunker {
+__call__(elements : Iterable[Element], *, max_characters : Optional[int]) -> list[Element]
}
class add_chunking_strategy {
+add_chunking_strategy(func : Callable[_P, list[Element]]) -> Callable[_P, list[Element]]
}
class chunk {
+chunk(elements : Iterable[Element], chunking_strategy : str, **kwargs : Any) -> list[Element]
}
class register_chunking_strategy {
+register_chunking_strategy(name : str, chunker : Chunker) -> None
}
class _ChunkerSpec {
+chunker : Chunker
+kw_arg_names() -> tuple[str, ...]
}
```

**Diagram sources** 
- [chunking/dispatch.py](file://unstructured/chunking/dispatch.py#L24-L130)

## Embedding Interfaces
The embedding interfaces in the unstructured library provide a unified API for generating embeddings using various providers.

```mermaid
classDiagram
class EMBEDDING_PROVIDER_TO_CLASS_MAP {
+EMBEDDING_PROVIDER_TO_CLASS_MAP
}
class BedrockEmbeddingEncoder {
+BedrockEmbeddingEncoder
}
class HuggingFaceEmbeddingEncoder {
+HuggingFaceEmbeddingEncoder
}
class MixedbreadAIEmbeddingEncoder {
+MixedbreadAIEmbeddingEncoder
}
class OctoAIEmbeddingEncoder {
+OctoAIEmbeddingEncoder
}
class OpenAIEmbeddingEncoder {
+OpenAIEmbeddingEncoder
}
class VertexAIEmbeddingEncoder {
+VertexAIEmbeddingEncoder
}
class VoyageAIEmbeddingEncoder {
+VoyageAIEmbeddingEncoder
}
EMBEDDING_PROVIDER_TO_CLASS_MAP --> BedrockEmbeddingEncoder
EMBEDDING_PROVIDER_TO_CLASS_MAP --> HuggingFaceEmbeddingEncoder
EMBEDDING_PROVIDER_TO_CLASS_MAP --> MixedbreadAIEmbeddingEncoder
EMBEDDING_PROVIDER_TO_CLASS_MAP --> OctoAIEmbeddingEncoder
EMBEDDING_PROVIDER_TO_CLASS_MAP --> OpenAIEmbeddingEncoder
EMBEDDING_PROVIDER_TO_CLASS_MAP --> VertexAIEmbeddingEncoder
EMBEDDING_PROVIDER_TO_CLASS_MAP --> VoyageAIEmbeddingEncoder
```

**Diagram sources** 
- [embed/__init__.py](file://unstructured/embed/__init__.py#L1-L28)

## Error Handling
The unstructured library defines custom exceptions for handling specific error conditions during document processing.

```mermaid
classDiagram
class PageCountExceededError {
+int document_pages
+int pdf_hi_res_max_pages
+str message
+__init__(document_pages : int, pdf_hi_res_max_pages : int)
}
class UnprocessableEntityError {
+UnprocessableEntityError
}
```

**Diagram sources** 
- [errors.py](file://unstructured/errors.py#L1-L16)

## Utility Functions
The unstructured library provides various utility functions for common operations such as metadata processing, coordinate calculations, and text analysis.

### Metadata Processing
The `process_metadata` decorator adds post-processing steps to document partitioners, including element ID assignment and metadata updates.

```mermaid
classDiagram
class process_metadata {
+process_metadata() -> Callable[[Callable[_P, list[Element]]], Callable[_P, list[Element]]]
}
class assign_and_map_hash_ids {
+assign_and_map_hash_ids(elements : list[Element]) -> list[Element]
}
```

**Diagram sources** 
- [documents/elements.py](file://unstructured/documents/elements.py#L529-L598)

### Coordinate and Text Utilities
The library provides functions for calculating coordinate overlaps, identifying overlapping elements, and analyzing text content.

```mermaid
classDiagram
class is_parent_box {
+is_parent_box(parent_target : Box, child_target : Box, add : float = 0.0) -> bool
}
class calculate_overlap_percentage {
+calculate_overlap_percentage(box1 : Points, box2 : Points, intersection_ratio_method : str = "total") -> tuple[float, float, float, float]
}
class identify_overlapping_case {
+identify_overlapping_case(box_pair : list[Points] | tuple[Points, Points], label_pair : list[str] | tuple[str, str], text_pair : list[str] | tuple[str, str], ix_pair : list[str] | tuple[str, str], sm_overlap_threshold : float = 10.0) -> tuple[Optional[list[str]], Optional[str], Optional[float], Optional[float], Optional[float], Optional[float], Optional[float], Optional[float]]
}
class identify_overlapping_or_nesting_case {
+identify_overlapping_or_nesting_case(box_pair : list[Points] | tuple[Points, Points], label_pair : list[str] | tuple[str, str], text_pair : list[str] | tuple[str, str], nested_error_tolerance_px : int = 5, sm_overlap_threshold : float = 10.0) -> tuple[Optional[list[str]], Optional[str], Optional[str], Optional[float], Optional[float], Optional[float], Optional[float], Optional[float], Optional[float], Optional[float]]
}
class catch_overlapping_and_nested_bboxes {
+catch_overlapping_and_nested_bboxes(elements : list["Text"], nested_error_tolerance_px : int = 5, sm_overlap_threshold : float = 10.0) -> tuple[bool, list[dict[str, Any]]]
}
class calculate_largest_ngram_percentage {
+calculate_largest_ngram_percentage(first_string : str, second_string : str) -> tuple[float, set[tuple[str, ...]], str]
}
class calculate_shared_ngram_percentage {
+calculate_shared_ngram_percentage(first_string : str, second_string : str, n : int) -> tuple[float, set[tuple[str, ...]]]
}
class ngrams {
+ngrams(s : list[str], n : int) -> list[tuple[str, ...]]
}
```

**Diagram sources** 
- [utils.py](file://unstructured/utils.py#L378-L749)

## Versioning and Backward Compatibility
The unstructured library maintains backward compatibility through careful versioning practices and deprecation warnings. The library follows semantic versioning principles, where major version increments indicate breaking changes, minor version increments indicate new features, and patch version increments indicate bug fixes.

When introducing changes that may affect backward compatibility, the library uses deprecation warnings to inform users of upcoming changes. For example, the `file_filename` parameter in the `partition_via_api` function is marked for deprecation in favor of the `metadata_filename` parameter.

The library also provides configuration options to control behavior changes, such as the `UNSTRUCTURED_INCLUDE_DEBUG_METADATA` environment variable that controls whether debug metadata fields are included in element metadata.

## Performance Recommendations
For high-throughput scenarios, the unstructured library provides several performance recommendations:

1. Use the appropriate partitioning strategy for your document type. For PDFs with extractable text, the "fast" strategy is significantly faster than the "hi_res" strategy.

2. When processing multiple documents, use batch processing functions like `partition_multiple_via_api` to reduce API overhead.

3. Configure chunking parameters appropriately for your use case. Setting `max_characters` to an appropriate value can reduce the number of chunks and improve processing efficiency.

4. Use the `include_orig_elements` parameter judiciously, as including original elements in chunk metadata increases memory usage.

5. For large documents, consider processing them in parallel using multiple threads or processes.

6. Cache frequently used models and resources to avoid repeated initialization overhead.

7. Use appropriate logging levels to minimize I/O overhead during processing.

8. When using the API, configure retry parameters appropriately to balance reliability and performance.