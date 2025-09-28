# OWPML Quick Reference

## Essential Includes

```cpp
#include "OWPMLApi/OWPMLApi.h"
#include "OWPML/OWPML.h"
```

## Core Operations

### Open Document
```cpp
OWPML::COwpmlDocumnet* doc = OWPML::COwpmlDocumnet::OpenDocument(L"file.hwpx");
// Always check for null!
if (doc) {
    // Use document
    delete doc; // Don't forget to delete!
}
```

### Create New Document
```cpp
OWPML::COwpmlDocumnet* doc = OWPML::COwpmlDocumnet::CreateDocument();
```

### Save Document
```cpp
BOOL success = doc->Save(L"output.hwpx");
```

### Get Document Info
```cpp
OWPML::CHWPMLHeadType* header = doc->GetHead();
if (header) {
    UINT sectionCount = header->GetSecCnt();
    LPCWSTR version = header->GetVersion();
}
```

### Access Sections
```cpp
std::vector<OWPML::CSectionType*>* sections = doc->GetSections();
for (size_t i = 0; i < sections->size(); i++) {
    OWPML::CSectionType* section = sections->at(i);
    // Process section
}
```

## Text Extraction

### Simple Text Extraction
```cpp
void ExtractText(OWPML::CObject* obj, std::wstring& result) {
    if (!obj) return;
    
    if (obj->GetID() == ID_PARA_T) {
        OWPML::CT* text = static_cast<OWPML::CT*>(obj);
        const wchar_t* textContent = text->GetText();
        if (textContent) result += textContent;
    }
    
    // Recurse through children
    OWPML::Objectlist* children = obj->GetObjectList();
    if (children) {
        for (auto child : *children) {
            ExtractText(child, result);
        }
    }
}
```

### Extract All Document Text
```cpp
std::wstring GetDocumentText(OWPML::COwpmlDocumnet* doc) {
    std::wstring result;
    auto sections = doc->GetSections();
    if (sections) {
        for (auto section : *sections) {
            ExtractText(section, result);
        }
    }
    return result;
}
```

## Content Creation

### Add Paragraph with Text
```cpp
OWPML::CSectionType* section = /* get or create section */;
OWPML::CPType* para = section->Setp();
OWPML::CRunType* run = para->Setrun();
OWPML::CT* text = run->Sett();
text->SetText(L"Your text here");
```

### Create Multiple Paragraphs
```cpp
std::vector<std::wstring> paragraphs = {L"Para 1", L"Para 2", L"Para 3"};
for (const auto& paraText : paragraphs) {
    OWPML::CPType* para = section->Setp();
    OWPML::CRunType* run = para->Setrun();
    OWPML::CT* text = run->Sett();
    text->SetText(paraText.c_str());
}
```

## Object Identification

### Common Object IDs
```cpp
ID_PARA_T          // Text content
ID_PARA_PType      // Paragraph
ID_PARA_RunType    // Text run
ID_PARA_LineSeg    // Line break
ID_PARA_Tab        // Tab
ID_PARA_Char       // Special character
ID_PARA_TableType  // Table
ID_PARA_PictureType // Image
```

### Check Object Type
```cpp
if (object->GetID() == ID_PARA_T) {
    OWPML::CT* textObj = static_cast<OWPML::CT*>(object);
    // Work with text
}
```

## Error Handling Patterns

### Safe Object Access
```cpp
OWPML::CObject* obj = GetSomeObject();
if (obj && obj->GetID() == EXPECTED_ID) {
    ExpectedType* typedObj = static_cast<ExpectedType*>(obj);
    // Safe to use typedObj
}
```

### Safe Collection Iteration
```cpp
OWPML::Objectlist* children = object->GetObjectList();
if (children && !children->empty()) {
    for (auto child : *children) {
        if (child) {
            // Safe to use child
        }
    }
}
```

### Document Validation
```cpp
bool IsValidDocument(OWPML::COwpmlDocumnet* doc) {
    if (!doc) return false;
    
    auto sections = doc->GetSections();
    if (!sections || sections->empty()) return false;
    
    auto header = doc->GetHead();
    if (!header) return false;
    
    return true;
}
```

## Memory Management

### RAII Pattern
```cpp
class DocumentGuard {
    OWPML::COwpmlDocumnet* m_doc;
public:
    DocumentGuard(LPCWSTR path) : m_doc(OWPML::COwpmlDocumnet::OpenDocument(path)) {}
    ~DocumentGuard() { delete m_doc; }
    
    OWPML::COwpmlDocumnet* get() const { return m_doc; }
    bool isValid() const { return m_doc != nullptr; }
};

// Usage
DocumentGuard guard(L"file.hwpx");
if (guard.isValid()) {
    // Use guard.get()
} // Automatic cleanup
```

## Common Patterns

### Find Objects by Type
```cpp
template<typename T>
void FindObjectsByType(OWPML::CObject* root, UINT typeId, std::vector<T*>& results) {
    if (!root) return;
    
    if (root->GetID() == typeId) {
        results.push_back(static_cast<T*>(root));
    }
    
    OWPML::Objectlist* children = root->GetObjectList();
    if (children) {
        for (auto child : *children) {
            FindObjectsByType<T>(child, typeId, results);
        }
    }
}

// Usage
std::vector<OWPML::CPictureType*> images;
FindObjectsByType<OWPML::CPictureType>(section, ID_PARA_PictureType, images);
```

### Count Elements
```cpp
int CountElementsOfType(OWPML::CObject* root, UINT typeId) {
    if (!root) return 0;
    
    int count = (root->GetID() == typeId) ? 1 : 0;
    
    OWPML::Objectlist* children = root->GetObjectList();
    if (children) {
        for (auto child : *children) {
            count += CountElementsOfType(child, typeId);
        }
    }
    
    return count;
}
```

### Deep Copy Section
```cpp
OWPML::CSectionType* CloneSection(OWPML::CSectionType* original) {
    if (!original) return nullptr;
    return original->Clone();
}
```

## File Operations

### Check File Exists
```cpp
#include <filesystem>
bool FileExists(const std::wstring& path) {
    return std::filesystem::exists(path);
}
```

### Save with Error Checking
```cpp
bool SaveDocumentSafely(OWPML::COwpmlDocumnet* doc, const std::wstring& path) {
    if (!doc) return false;
    
    // Check if directory exists
    std::filesystem::path filePath(path);
    std::filesystem::path directory = filePath.parent_path();
    
    if (!directory.empty() && !std::filesystem::exists(directory)) {
        std::filesystem::create_directories(directory);
    }
    
    return doc->Save(path.c_str()) == TRUE;
}
```

## Debugging Helpers

### Print Object Tree
```cpp
void PrintObjectTree(OWPML::CObject* obj, int indent = 0) {
    if (!obj) return;
    
    std::wstring indentStr(indent * 2, L' ');
    std::wcout << indentStr << L"Object ID: " << obj->GetID() << std::endl;
    
    OWPML::Objectlist* children = obj->GetObjectList();
    if (children) {
        for (auto child : *children) {
            PrintObjectTree(child, indent + 1);
        }
    }
}
```

### Count All Objects
```cpp
int CountAllObjects(OWPML::CObject* root) {
    if (!root) return 0;
    
    int count = 1; // Count this object
    
    OWPML::Objectlist* children = root->GetObjectList();
    if (children) {
        for (auto child : *children) {
            count += CountAllObjects(child);
        }
    }
    
    return count;
}
```

## Performance Tips

- Always check pointers before use
- Use references instead of pointers when possible
- Avoid deep recursion with large documents
- Process documents in chunks for large files
- Cache frequently accessed objects
- Delete documents promptly to free memory