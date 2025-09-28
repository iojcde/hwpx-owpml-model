# OWPML Model Architecture

## Overview

The OWPML (Open Word processing ML) model is the C++ object-oriented framework used to parse, manipulate, and generate HWPX documents. The model is organized into a hierarchical class structure that mirrors the XML structure of HWPX files.

## Project Structure

```
hwpx-owpml-model/
├── OWPML/                    # Core OWPML library
│   ├── Base/                 # Base classes and utilities
│   ├── Class/                # OWPML element classes
│   │   ├── Core/             # Core elements (colors, images, etc.)
│   │   ├── Head/             # Header elements (styles, fonts, etc.)
│   │   ├── Para/             # Paragraph and content elements
│   │   ├── Etc/              # Miscellaneous elements
│   │   └── RDF/              # RDF metadata elements
│   ├── Document.cpp/h        # Main document handling
│   ├── Expat/                # XML parser (Expat library)
│   ├── Hwp/                  # Hancom specific implementations
│   └── Zip/                  # ZIP file handling
├── OWPMLApi/                 # High-level API
├── OWPMLUtil/                # Utility functions
└── OWPMLTest/                # Example and test code
```

## Core Classes

### Document Classes

#### `CDocument`
The main document class that handles:
- ZIP file operations for HWPX files
- XML parsing and serialization
- Container and manifest parsing
- Document lifecycle management

```cpp
namespace OWPML {
    class CDocument {
    public:
        BOOL Read(LPCWSTR path);           // Open HWPX file
        BOOL Write(LPCWSTR path);          // Save HWPX file
        BOOL Parse(LPCWSTR xmlPath, CDefaultHandler* handler);
        // ... other methods
    };
}
```

#### `COwpmlDocumnet` (API wrapper)
High-level document interface that provides:
- Static document creation and opening methods
- Access to document sections and metadata
- Text extraction capabilities

```cpp
namespace OWPML {
    class COwpmlDocumnet {
    public:
        static COwpmlDocumnet* OpenDocument(LPCWSTR path);
        static COwpmlDocumnet* CreateDocument();
        
        std::vector<CSectionType*>* GetSections();
        CHWPMLHeadType* GetHead();
        // ... other methods
    };
}
```

### Serialization Classes

#### `COwpmlSerialize`
Handles the conversion between HWPX files and OWPML objects:
- Reading HWPX components (sections, headers, binary data)
- Writing OWPML objects back to HWPX format
- Managing file relationships and references

```cpp
namespace OWPML {
    class COwpmlSerialize {
    public:
        BOOL Open(LPCWSTR path);
        BOOL Save(LPCWSTR path);
        
        // Reading methods
        BOOL _ReadSections();
        BOOL _ReadHead();
        BOOL _ReadPreviewText();
        
        // Writing methods  
        BOOL _WriteSections();
        BOOL _WriteHead();
        BOOL _WriteMimetype();
        // ... other methods
    };
}
```

## Class Hierarchy

### Base Classes

All OWPML elements inherit from base classes that provide common functionality:

```cpp
// Base object class
class CObject {
public:
    virtual UINT GetID() = 0;
    virtual Objectlist* GetObjectList();
    virtual void SetObjectList(Objectlist* pObjectList);
    // ... other methods
};

// Named object with XML serialization
class CNamedObject : public CObject {
public:
    virtual bool WriteElement(LPCWSTR pCurObjName, CSerializer* serialize, CAttribute* att);
    virtual bool ReadAttribute(CAttribute* att);
    // ... other methods
};

// Extended object with additional properties
class CExtObject : public CNamedObject {
    // Extended functionality
};
```

### Element Categories

#### Head Elements (`OWPML/Class/Head/`)
Document-level definitions and styles:

- **`CHWPMLHeadType`**: Root header element
- **`CFontfaceType`**: Font definitions
- **`CCharShapeType`**: Character formatting definitions
- **`CParaShapeType`**: Paragraph formatting definitions
- **`CStyleType`**: Named styles
- **`CBorderFillType`**: Border and fill definitions
- **`CNumberingType`**: Numbering schemes

```cpp
class CHWPMLHeadType : public CNamedObject {
public:
    UINT GetSecCnt();                    // Number of sections
    void SetSecCnt(UINT secCnt);
    
    // Child element accessors
    CFontfaceType* GetFontfaces(int index = 0);
    CCharShapeType* GetCharshapes(int index = 0);
    CParaShapeType* GetParashapes(int index = 0);
    // ... other accessors
};
```

#### Section Elements (`OWPML/Class/Etc/`)
Document structure and content containers:

- **`CSectionType`**: Document sections
- **`CParaListType`**: Paragraph lists
- **`CHeaderFooterType`**: Headers and footers

```cpp
class CSectionType : public CNamedObject {
public:
    static CSectionType* Create();
    CPType* Setp(CPType* pp = NULL);     // Add paragraph
    CPType* Getp(int index);             // Get paragraph by index
    // ... other methods
};
```

#### Paragraph Elements (`OWPML/Class/Para/`)
Content and inline elements:

- **`CPType`**: Paragraphs
- **`CRunType`**: Text runs
- **`CT`**: Text content
- **`CTableType`**: Tables
- **`CPictureType`**: Images
- **`CEquationType`**: Equations

```cpp
class CPType : public CNamedObject {
public:
    static CPType* Create();
    CRunType* Setrun(CRunType* run = NULL);
    CTableType* Settbl(CTableType* tbl = NULL);
    // ... other content setters
};

class CRunType : public CNamedObject {
public:
    CT* Sett(CT* text = NULL);           // Add text
    CChar* Setchar(CChar* ch = NULL);    // Add character
    // ... other inline element setters
};
```

#### Core Elements (`OWPML/Class/Core/`)
Basic data types and common elements:

- **`CPointType`**: Point coordinates
- **`CMatrixType`**: Transformation matrices
- **`CImageType`**: Image references
- **`CColorType`**: Color definitions

## Object Identification System

Each OWPML class has a unique identifier defined in `ClassID.h`:

```cpp
// Example IDs from enumeration
enum {
    ID_BODY_SectionType = 1000,
    ID_PARA_PType = 2000,
    ID_PARA_RunType = 2001,
    ID_PARA_T = 2002,
    ID_HEAD_CharShape = 3000,
    // ... many more IDs
};
```

This system allows for:
- Runtime type identification
- Dynamic object creation
- Efficient object searching and filtering

## Serialization Framework

### XML Serialization
The model uses a sophisticated serialization system:

```cpp
class CSerializer {
public:
    virtual bool WriteElement(LPCWSTR elementName, CObject* object, 
                             CAttribute* att, LPCWSTR value);
    virtual bool ReadElement(/* parameters */);
    // ... other methods
};

class CXMLSerializer : public CSerializer {
    // XML-specific implementation
};
```

### Attribute Handling
Elements handle their XML attributes through the `CAttribute` class:

```cpp
class CAttribute {
public:
    bool SetValue(LPCWSTR name, LPCWSTR value);
    LPCWSTR GetValue(LPCWSTR name);
    // ... other methods
};
```

## Memory Management

The OWPML model uses several memory management strategies:

- **Object Pools**: Reuse of common objects
- **Smart Pointers**: Automatic memory cleanup (in some areas)
- **Manual Management**: Traditional new/delete (legacy areas)
- **Container Classes**: STL containers for collections

## Usage Patterns

### Creating Documents
```cpp
// Create new document
COwpmlDocumnet* doc = COwpmlDocumnet::CreateDocument();

// Add content
CSectionType* section = CSectionType::Create();
CPType* paragraph = section->Setp();
CRunType* run = paragraph->Setrun();
CT* text = run->Sett();
text->SetText(L"Hello World");

// Save document
doc->Save(L"output.hwpx");
```

### Reading Documents
```cpp
// Open existing document
COwpmlDocumnet* doc = COwpmlDocumnet::OpenDocument(L"input.hwpx");

// Access content
std::vector<CSectionType*>* sections = doc->GetSections();
for (auto section : *sections) {
    // Process section content
    ProcessSection(section);
}
```

### Text Extraction
```cpp
void ExtractText(CObject* object, std::wstring& output) {
    if (object->GetID() == ID_PARA_T) {
        CT* textElement = static_cast<CT*>(object);
        output += textElement->GetText();
    }
    
    // Recursively process child objects
    Objectlist* children = object->GetObjectList();
    if (children) {
        for (auto child : *children) {
            ExtractText(child, output);
        }
    }
}
```

## Extension Points

The OWPML model supports extensions through:

1. **Custom Elements**: Inherit from base classes
2. **Custom Handlers**: Implement parsing for new elements
3. **Custom Serializers**: Handle new output formats
4. **Plugin Architecture**: Load additional functionality

## Performance Considerations

- **Lazy Loading**: Content loaded on demand
- **Streaming**: Large documents processed in chunks
- **Caching**: Frequently accessed data cached
- **Memory Pools**: Reduce allocation overhead

## Thread Safety

The current OWPML model is **not thread-safe**. When using in multi-threaded applications:
- Use separate document instances per thread
- Implement external synchronization for shared access
- Consider the upcoming thread-safe version (if planned)

## Debugging and Diagnostics

The model includes debugging support:
- **Assertion Macros**: Runtime validation
- **Logging**: Trace document processing
- **Validation**: XML schema validation
- **Memory Debugging**: Leak detection in debug builds