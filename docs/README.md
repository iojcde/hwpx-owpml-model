# HWPX-OWPML Model Documentation

This directory contains comprehensive documentation for the HWPX file format and OWPML model library.

## Documentation Index

### 📄 [HWPX Format Specification](HWPX_Format_Specification.md)
**Complete technical specification of the HWPX file format**
- File structure and internal organization
- XML namespaces and schemas
- Document hierarchy and content model
- Binary data handling
- Security and extensibility features

### 🏗️ [OWPML Model Architecture](OWPML_Model_Architecture.md)
**Technical overview of the C++ OWPML object model**
- Project structure and organization
- Class hierarchy and inheritance
- Object identification system
- Serialization framework
- Memory management and performance considerations

### 📚 [API Reference](API_Reference.md)
**Complete API documentation and reference**
- Core classes and methods
- Function signatures and parameters
- Return values and error handling
- Memory management guidelines
- Build and integration instructions

### 💡 [Usage Examples](Usage_Examples.md)
**Practical code examples and tutorials**
- Basic document operations
- Text extraction techniques
- Document creation from scratch
- Working with styles and formatting
- Advanced processing scenarios
- Error handling best practices

### ⚡ [Quick Reference](Quick_Reference.md)
**Essential code snippets and common patterns**
- Core operations cheat sheet
- Text extraction patterns
- Content creation shortcuts
- Error handling templates
- Memory management helpers
- Performance tips

## Quick Start

For developers who want to get started quickly:

1. **⚡ Essential Patterns**: See [Quick Reference](Quick_Reference.md) for common code snippets
2. **📖 Reading Documents**: See [Basic Document Operations](Usage_Examples.md#basic-document-operations)
3. **📝 Extracting Text**: See [Text Extraction](Usage_Examples.md#text-extraction)
4. **🆕 Creating Documents**: See [Document Creation](Usage_Examples.md#document-creation)

## File Format Overview

HWPX (Hancom Word Processor X) is an XML-based document format that uses a ZIP container structure similar to OOXML. Key characteristics:

- **Container Format**: ZIP archive with structured directories
- **Content Format**: XML using OWPML schemas
- **Compatibility**: OOXML-inspired design for interoperability
- **Features**: Full support for Korean typography, advanced formatting, embedded objects

## Library Overview

The OWPML model provides:

- **C++ Object Model**: Type-safe representation of HWPX documents
- **XML Serialization**: Bidirectional conversion between objects and XML
- **File I/O**: Complete HWPX file reading and writing capabilities
- **Memory Management**: Efficient handling of large documents
- **Cross-Platform**: Windows-focused but portable design

## Common Use Cases

### Document Processing
- **Text Extraction**: Convert HWPX to plain text or other formats
- **Content Analysis**: Analyze document structure and statistics
- **Batch Processing**: Process multiple documents programmatically
- **Format Conversion**: Convert between HWPX and other formats

### Document Generation
- **Report Generation**: Create formatted documents programmatically
- **Template Processing**: Fill document templates with data
- **Content Assembly**: Combine multiple sources into documents
- **Custom Publishing**: Generate documents with specific formatting

### Integration Scenarios
- **Enterprise Systems**: Integrate HWPX support into business applications
- **Content Management**: Build document management systems
- **Workflow Automation**: Automate document processing pipelines
- **Data Migration**: Convert legacy documents to HWPX format

## Prerequisites

### Development Environment
- **Windows**: Primary target platform (Windows 10/11)
- **Visual Studio**: 2017 15.9.42 or later recommended
- **C++ Standard**: C++11 or later
- **Architecture**: x86 primary, x64 supported

### Dependencies
- **Expat XML Parser**: Included (libexpat 2.0.1/2.1.0)
- **ZIP Library**: Built-in ZIP handling
- **Windows APIs**: Platform-specific functionality

## Getting Help

### Documentation
- Read through the specification and examples
- Check the API reference for specific functions
- Review the architecture documentation for design decisions

### Sample Code
- Examine `OWPMLTest/` for working examples
- Study the text extraction implementation
- Look at the document creation patterns

### Community
- Use GitHub [Discussions](https://github.com/hancom-io/hwpx-owpml-model/discussions) for questions
- Report issues through GitHub Issues
- Contribute improvements via Pull Requests

## Contributing to Documentation

### Improving Existing Docs
- Fix typos and clarify explanations
- Add missing information or examples
- Update outdated references

### Adding New Content
- Create tutorials for specific scenarios
- Document advanced use cases
- Add troubleshooting guides

### Documentation Standards
- Use clear, concise language
- Include working code examples
- Test all code snippets
- Follow existing formatting conventions

## Version Information

- **OWPML Version**: 1.31 (current)
- **Documentation Version**: 1.0
- **Last Updated**: 2024
- **Compatibility**: HWPX format versions 1.0+

## License

This documentation is provided under the same license as the OWPML library. See [LICENSE.txt](../LICENSE.txt) for details.

---

For questions about this documentation, please use the project's [Discussions](https://github.com/hancom-io/hwpx-owpml-model/discussions) page.