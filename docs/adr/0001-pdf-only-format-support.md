# 1. PDF-Only Format Support

Date: 2025-02-25

## Status

Accepted

## Context

PDFMathTranslate is a document translation tool that needs to handle various types of content, including mathematical formulas, while maintaining the original document's layout and formatting. The decision to support only PDF files as input format was made based on several technical and practical considerations.

## Decision

We have decided to support only PDF files as the input format for PDFMathTranslate. This decision is implemented through:

1. Core Dependencies:
   - `pdfminer` for PDF parsing and content extraction
   - `pymupdf` for PDF manipulation and font handling
   - Custom PDF-specific converters and interpreters

2. Architecture Components:
   - `PDFConverterEx` class for enhanced PDF conversion
   - `TranslateConverter` for handling PDF-specific layout and translation
   - PDF-specific font subsetting and handling

3. File Processing:
   - Direct processing of PDF files without intermediate format conversion
   - Preservation of PDF-specific features like layout, fonts, and mathematical formulas
   - Generation of both monolingual (translated) and bilingual PDF outputs

## Consequences

### Positive

1. **Specialized Processing**:
   - Optimized handling of PDF-specific features
   - Better preservation of mathematical formulas and complex layouts
   - Direct access to PDF structure without format conversion losses

2. **Quality Control**:
   - Consistent output format
   - Reliable handling of fonts and special characters
   - Maintained document structure and formatting

3. **Development Focus**:
   - Concentrated effort on perfecting PDF handling
   - Simplified testing and quality assurance
   - Reduced complexity in handling multiple formats

### Negative

1. **Limited Input Support**:
   - Users must convert other formats to PDF first
   - No direct support for common formats like DOCX, ODT, etc.
   - Potential loss of some features during conversion to PDF

2. **User Experience**:
   - Additional step required for non-PDF documents
   - May require external tools for format conversion
   - Could be a barrier for some users

3. **Future Considerations**:
   - Integration with `babeldoc` might be needed for broader format support
   - May need to maintain separate processing paths for different formats
   - Could require significant refactoring to add new format support

## Technical Details

### Current Implementation

```python
# File handling is strictly PDF-focused
def find_all_files_in_directory(directory_path):
    pdf_files = []
    for root, _, files in os.walk(directory_path):
        for file in files:
            if file.lower().endswith('.pdf'):
                pdf_files.append(os.path.join(root, file))
    return pdf_files
```

### Core Components

1. PDF Processing:
   - `PDFParser` for structural analysis
   - `PDFDocument` for document representation
   - `PDFPage` for page-level processing

2. Translation Pipeline:
   - PDF parsing → Content extraction → Translation → PDF generation
   - Maintains PDF-specific metadata and structure
   - Handles mathematical formulas with special care

## Related Decisions

- Font handling and subsetting strategy
- Mathematical formula preservation approach
- Output format decisions (mono/bilingual PDFs)

## References

- [PDFMiner Documentation](https://pdfminersix.readthedocs.io/)
- [PyMuPDF Documentation](https://pymupdf.readthedocs.io/)
- Project source code in `/pdf2zh` directory
