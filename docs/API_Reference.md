# OWPML API Reference

## Quick Start

### Basic Usage

```cpp
#include "OWPMLApi/OWPMLApi.h"
#include "OWPML/OWPML.h"

// Open a HWPX document
OWPML::COwpmlDocumnet* document = OWPML::COwpmlDocumnet::OpenDocument(L"document.hwpx");
if (document) {
    // Work with document
    // ...
    delete document;
}
```

### Text Extraction Example

```cpp
#include "OWPMLTest/HwpxToText.h"

void ExtractTextFromHwpx(LPCWSTR inputPath, LPCWSTR outputPath) {
    OWPML::COwpmlDocumnet* document = OWPML::COwpmlDocumnet::OpenDocument(inputPath);
    if (!document) return;
    
    std::wstring extractedText;
    std::vector<OWPML::CSectionType*>* sections = document->GetSections();
    
    for (auto section : *sections) {
        ProcessingTextElement(section, extractedText);
    }
    
    // Save to file
    std::wofstream outFile(outputPath);
    outFile << extractedText;
    outFile.close();
    
    delete document;
}
```

## Core API Classes

### COwpmlDocumnet

The main document interface for high-level operations.

#### Static Methods

```cpp
static COwpmlDocumnet* OpenDocument(LPCWSTR path);
```
- **Purpose**: Opens an existing HWPX document
- **Parameters**: `path` - Full path to the HWPX file
- **Returns**: Document object or `nullptr` on failure
- **Example**:
```cpp
auto doc = OWPML::COwpmlDocumnet::OpenDocument(L"C:\\Documents\\test.hwpx");
```

```cpp
static COwpmlDocumnet* CreateDocument();
```
- **Purpose**: Creates a new empty document
- **Returns**: New document object
- **Example**:
```cpp
auto doc = OWPML::COwpmlDocumnet::CreateDocument();
```

#### Instance Methods

```cpp
std::vector<OWPML::CSectionType*>* GetSections();
```
- **Purpose**: Get all document sections
- **Returns**: Vector of section objects
- **Example**:
```cpp
auto sections = doc->GetSections();
for (size_t i = 0; i < sections->size(); i++) {
    OWPML::CSectionType* section = sections->at(i);
    // Process section
}
```

```cpp
OWPML::CHWPMLHeadType* GetHead();
```
- **Purpose**: Get document header with styles and formatting
- **Returns**: Header object containing document-level definitions

```cpp
BOOL Save(LPCWSTR path);
```
- **Purpose**: Save document to HWPX file
- **Parameters**: `path` - Output file path
- **Returns**: `TRUE` on success, `FALSE` on failure

### COwpmlSerialize

Low-level serialization interface for advanced operations.

#### Constructor and Basic Operations

```cpp
COwpmlSerialize();
~COwpmlSerialize();
```

```cpp
BOOL Open(LPCWSTR path);
```
- **Purpose**: Opens HWPX file for reading
- **Parameters**: `path` - File path
- **Returns**: `TRUE` on success

```cpp
BOOL Save(LPCWSTR path);
```
- **Purpose**: Saves current state to HWPX file
- **Parameters**: `path` - Output file path
- **Returns**: `TRUE` on success

#### Access Methods

```cpp
OWPML::CVersion* GetVersion();
```
- **Returns**: Document version information

```cpp
OWPML::CHWPApplicationSetting* GetApplicationSetting();
```
- **Returns**: Application-specific settings

```cpp
OWPML::CPackage* GetPackage();
```
- **Returns**: Package information (content.hpf)

```cpp
std::vector<OWPML::CSectionType*>* GetSections();
```
- **Returns**: Document sections

```cpp
std::vector<OWPML::CHWPMLHistoryType*>* GetHistories();
```
- **Returns**: Document revision history

#### Binary Data Handling

```cpp
BOOL ReadBindata(LPCWSTR idstring, BOOL bOle);
```
- **Purpose**: Read binary data (images, objects) from document
- **Parameters**: 
  - `idstring` - Binary data identifier
  - `bOle` - Whether this is an OLE object
- **Returns**: `TRUE` on success

## Document Structure Classes

### CSectionType

Represents a document section with its own page settings and content.

```cpp
static CSectionType* Create();
CSectionType* Clone();
```
- **Purpose**: Create new or clone existing section

```cpp
CPType* Setp(CPType* pp = nullptr);
```
- **Purpose**: Add a paragraph to the section
- **Parameters**: `pp` - Existing paragraph or `nullptr` to create new
- **Returns**: Paragraph object

```cpp
CPType* Getp(int index);
```
- **Purpose**: Get paragraph by index
- **Parameters**: `index` - Zero-based paragraph index
- **Returns**: Paragraph object or `nullptr`

### CPType (Paragraph)

Represents a paragraph containing text runs and objects.

```cpp
static CPType* Create();
```
- **Purpose**: Create new paragraph

```cpp
CRunType* Setrun(CRunType* run = nullptr);
```
- **Purpose**: Add a text run to paragraph
- **Returns**: Run object

```cpp
CTableType* Settbl(CTableType* tbl = nullptr);
```
- **Purpose**: Add a table to paragraph
- **Returns**: Table object

### CRunType

Represents a run of text with consistent formatting.

```cpp
CT* Sett(CT* text = nullptr);
```
- **Purpose**: Add text content to run
- **Returns**: Text object

```cpp
CChar* Setchar(CChar* ch = nullptr);
```
- **Purpose**: Add special character to run
- **Returns**: Character object

### CT (Text)

Contains actual text content.

```cpp
void SetText(LPCWSTR text);
LPCWSTR GetText();
```
- **Purpose**: Set or get text content

## Header and Style Classes

### CHWPMLHeadType

Document header containing all style and formatting definitions.

```cpp
UINT GetSecCnt();
void SetSecCnt(UINT secCnt);
```
- **Purpose**: Get/set number of sections in document

```cpp
LPCWSTR GetVersion();
void SetVersion(LPCWSTR version);
```
- **Purpose**: Get/set document format version

#### Style Access Methods

```cpp
CFontfaceType* GetFontfaces(int index = 0);
CCharShapeType* GetCharshapes(int index = 0);  
CParaShapeType* GetParashapes(int index = 0);
CStyleType* GetStyles(int index = 0);
CNumberingType* GetNumberings(int index = 0);
CBorderFillType* GetBorderfills(int index = 0);
```

### CCharShapeType

Character formatting definition.

```cpp
// Font properties
LPCWSTR GetFontRef();
void SetFontRef(LPCWSTR fontRef);

// Size and scaling
UINT GetSize();
void SetSize(UINT size);

// Text effects
BOOL GetBold();
void SetBold(BOOL bold);

BOOL GetItalic();
void SetItalic(BOOL italic);
```

### CParaShapeType

Paragraph formatting definition.

```cpp
// Alignment
UINT GetAlign();
void SetAlign(UINT align);

// Indentation
UINT GetIndent1();
void SetIndent1(UINT indent);

// Line spacing
UINT GetLineSpacing();
void SetLineSpacing(UINT spacing);
```

## Utility Functions

### Text Processing

```cpp
void ProcessingTextElement(OWPML::CObject* object, std::wstring& outtext);
```
- **Purpose**: Recursively extract text from any OWPML object
- **Parameters**:
  - `object` - Starting object (section, paragraph, etc.)
  - `outtext` - Output string to append text to

```cpp
void PrintText(OWPML::CT* text, std::wstring& outtext);
```
- **Purpose**: Extract text from a text element
- **Parameters**:
  - `text` - Text object
  - `outtext` - Output string

## Error Handling

### Return Values
Most API methods return `BOOL` values:
- `TRUE` indicates success
- `FALSE` indicates failure

### Null Pointer Checks
Always check for null pointers when calling accessor methods:

```cpp
OWPML::CSectionType* section = sections->at(0);
if (section) {
    OWPML::CPType* para = section->Getp(0);
    if (para) {
        // Safe to use para
    }
}
```

### Exception Handling
The current API does not use C++ exceptions. Check return values and null pointers instead.

## Memory Management

### Document Lifetime
Documents must be explicitly deleted:

```cpp
OWPML::COwpmlDocumnet* doc = OWPML::COwpmlDocumnet::OpenDocument(path);
// Use document...
delete doc;  // Important: explicitly delete
```

### Object Ownership
- Document owns all child objects
- Don't delete individual sections, paragraphs, etc.
- Only delete the root document object

## Common Patterns

### Reading Document Content

```cpp
void ReadDocumentStructure(OWPML::COwpmlDocumnet* doc) {
    // Get sections
    auto sections = doc->GetSections();
    
    for (size_t sectionIdx = 0; sectionIdx < sections->size(); sectionIdx++) {
        OWPML::CSectionType* section = sections->at(sectionIdx);
        
        // Enumerate paragraphs in section
        int paraCount = section->GetChildCount();
        for (int paraIdx = 0; paraIdx < paraCount; paraIdx++) {
            OWPML::CPType* para = section->Getp(paraIdx);
            if (para) {
                // Process paragraph content
                ProcessParagraph(para);
            }
        }
    }
}
```

### Creating New Document Content

```cpp
OWPML::COwpmlDocumnet* CreateSimpleDocument() {
    OWPML::COwpmlDocumnet* doc = OWPML::COwpmlDocumnet::CreateDocument();
    
    // Create section
    OWPML::CSectionType* section = OWPML::CSectionType::Create();
    
    // Create paragraph
    OWPML::CPType* para = section->Setp();
    
    // Create text run
    OWPML::CRunType* run = para->Setrun();
    
    // Add text
    OWPML::CT* text = run->Sett();
    text->SetText(L"Hello, World!");
    
    return doc;
}
```

### Working with Tables

```cpp
void CreateTable(OWPML::CPType* paragraph) {
    OWPML::CTableType* table = paragraph->Settbl();
    
    // Configure table properties
    // Add rows and cells
    // Set cell content
}
```

## Build Integration

### Visual Studio
1. Add include directories:
   - `$(ProjectDir)OWPML`
   - `$(ProjectDir)OWPMLApi`
   - `$(ProjectDir)OWPMLUtil`

2. Link libraries:
   - `OWPML.lib`
   - `OWPMLApi.lib`
   - `OWPMLUtil.lib`

### CMake Example

```cmake
# Find OWPML libraries
find_library(OWPML_LIB OWPML PATHS ${OWPML_ROOT}/lib)
find_library(OWPMLAPI_LIB OWPMLApi PATHS ${OWPML_ROOT}/lib)
find_library(OWPMLUTIL_LIB OWPMLUtil PATHS ${OWPML_ROOT}/lib)

# Include directories
target_include_directories(your_target PRIVATE 
    ${OWPML_ROOT}/OWPML
    ${OWPML_ROOT}/OWPMLApi
    ${OWPML_ROOT}/OWPMLUtil
)

# Link libraries
target_link_libraries(your_target ${OWPML_LIB} ${OWPMLAPI_LIB} ${OWPMLUTIL_LIB})
```