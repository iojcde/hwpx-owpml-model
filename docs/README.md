# HWPX Document Format Documentation

This directory contains comprehensive documentation for the HWPX file format and document object model.

## Documentation Index

### 📄 [HWPX Format Specification](HWPX_Format_Specification.md)
**Complete technical specification of the HWPX file format**
- ZIP container structure and file organization
- XML namespaces and schema definitions
- Document packaging and manifest structure
- Binary data handling and embedding
- Security features and digital signatures

### 🏗️ [HWPX Document Model](HWPX_Document_Model.md)
**Comprehensive overview of the HWPX document object model**
- Document structure and hierarchy
- Object categories and relationships
- Content organization and flow
- Formatting and styling model
- Interactive and media elements

### 📚 [HWPX Object Reference](HWPX_Object_Reference.md)
**Detailed specification of every HWPX object type**
- Complete object type catalog
- XML element definitions and attributes
- Object properties and child elements
- Relationship mappings and dependencies
- Usage examples for each object type

## Quick Start

For developers and researchers who want to understand the HWPX format:

1. **📄 File Format**: Start with [HWPX Format Specification](HWPX_Format_Specification.md) to understand the ZIP/XML structure
2. **🏗️ Document Model**: Read [HWPX Document Model](HWPX_Document_Model.md) for conceptual understanding of document objects
3. **📚 Object Details**: Reference [HWPX Object Reference](HWPX_Object_Reference.md) for specific object type information
4. **🔍 Implementation**: Use the C++ OWPML library in this repository to work with HWPX files programmatically

## File Format Overview

HWPX (Hancom Word Processor X) is an XML-based document format that uses a ZIP container structure similar to OOXML. Key characteristics:

- **Container Format**: ZIP archive with structured directories and manifest
- **Content Format**: XML using OWPML (Open Word processing ML) schemas
- **Object Model**: Hierarchical document structure with specialized object types
- **Compatibility**: OOXML-inspired design for broad interoperability
- **Features**: Comprehensive support for Korean typography, advanced formatting, multimedia, and interactive elements

## Document Object Model

The HWPX document model provides a rich object hierarchy:

- **Document Structure**: Sections, paragraphs, runs, and text content
- **Formatting Objects**: Character shapes, paragraph shapes, borders, and fills
- **Content Objects**: Tables, images, equations, charts, and media
- **Interactive Elements**: Forms, buttons, hyperlinks, and controls
- **Layout Objects**: Headers, footers, page layouts, and positioning
- **Metadata Objects**: Document properties, versioning, and package information

## Common Use Cases

### Document Analysis and Processing
- **Format Analysis**: Understanding HWPX file structure and content organization
- **Content Extraction**: Extracting text, images, and metadata from HWPX documents
- **Document Conversion**: Converting between HWPX and other document formats
- **Batch Processing**: Automated processing of multiple HWPX documents

### Document Understanding and Research
- **Format Research**: Studying the HWPX specification and implementation details
- **Compatibility Analysis**: Understanding relationships between HWPX and other formats
- **Content Mining**: Extracting structured data from document collections
- **Accessibility Processing**: Converting documents for accessibility tools

### Integration and Development
- **Format Support**: Adding HWPX support to document processing applications
- **Viewer Development**: Building HWPX document viewers and editors
- **Conversion Tools**: Creating utilities for format conversion and migration
- **Archive Processing**: Working with document archives and repositories

## Prerequisites

### For Document Format Research
- **XML Knowledge**: Understanding of XML structure and namespaces
- **ZIP Archives**: Familiarity with ZIP file structure and contents
- **Document Formats**: Background in document format specifications (OOXML, ODF)
- **Text Processing**: Understanding of typography and document layout concepts

### For Development with OWPML Library
- **Development Environment**: Windows with Visual Studio 2017 15.9.42 or later
- **C++ Knowledge**: C++11 or later for working with the OWPML library
- **XML Processing**: Experience with XML parsing and serialization
- **Document Processing**: Understanding of document object models

### Dependencies (Included)
- **Expat XML Parser**: libexpat 2.0.1/2.1.0 for XML processing
- **ZIP Library**: Built-in ZIP archive handling
- **Windows APIs**: Platform-specific functionality for Windows

## Getting Help

### Documentation Resources
- **Format Specification**: Complete technical details of the HWPX format
- **Object Reference**: Detailed information about every document object type
- **Implementation Examples**: Sample code in the `OWPMLTest/` directory
- **Official Specification**: [Hancom OWPML Documentation](https://www.hancom.com/etc/hwpDownload.do)

### Community Support
- **GitHub Discussions**: Use [Discussions](https://github.com/hancom-io/hwpx-owpml-model/discussions) for questions about the format or library
- **Issue Reporting**: Report bugs or format inconsistencies through GitHub Issues
- **Contributions**: Contribute documentation improvements and corrections via Pull Requests

### Research and Development
- **Format Evolution**: Track changes and updates to the HWPX specification
- **Implementation Notes**: Share experiences and findings with the community
- **Compatibility Testing**: Report compatibility issues with different HWPX versions

## Contributing to Documentation

### Improving Format Documentation
- **Object Specifications**: Add missing object types or correct existing specifications
- **Format Details**: Clarify XML schema definitions and element relationships
- **Examples**: Provide real-world examples of HWPX document structures
- **Compatibility Notes**: Document differences between HWPX versions

### Enhancing Reference Materials
- **Object Relationships**: Document parent-child and reference relationships
- **Attribute Specifications**: Complete attribute definitions and valid values
- **Usage Patterns**: Document common patterns and best practices
- **Format Evolution**: Track changes across HWPX format versions

### Documentation Standards
- **Technical Accuracy**: Ensure all specifications match actual HWPX behavior
- **Clear Examples**: Include well-formatted XML and structural examples
- **Consistent Terminology**: Use consistent naming for objects and concepts
- **Cross-References**: Link related objects and concepts appropriately

## Version Information

- **HWPX Format Version**: 1.31 (current specification)
- **OWPML Namespace Version**: 2011 (schema year)
- **Documentation Version**: 2.0 (focused on document model)
- **Last Updated**: 2024
- **Compatibility**: HWPX format versions 1.0 through 1.31

## License

This documentation is provided under the same Apache 2.0 license as the OWPML library. See [LICENSE.txt](../LICENSE.txt) for complete details.

---

For questions about this documentation or the HWPX format, please use the project's [Discussions](https://github.com/hancom-io/hwpx-owpml-model/discussions) page.