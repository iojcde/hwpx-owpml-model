# OWPML Usage Examples

This document provides practical examples of using the OWPML library to work with HWPX documents.

## Table of Contents

1. [Basic Document Operations](#basic-document-operations)
2. [Text Extraction](#text-extraction)
3. [Document Creation](#document-creation)
4. [Working with Styles](#working-with-styles)
5. [Image and Binary Data](#image-and-binary-data)
6. [Tables and Objects](#tables-and-objects)
7. [Advanced Processing](#advanced-processing)

## Basic Document Operations

### Opening and Reading a Document

```cpp
#include "OWPMLApi/OWPMLApi.h"
#include "OWPML/OWPML.h"

bool OpenAndInspectDocument(const std::wstring& filePath) {
    // Open the document
    OWPML::COwpmlDocumnet* document = OWPML::COwpmlDocumnet::OpenDocument(filePath.c_str());
    if (!document) {
        std::wcout << L"Failed to open document: " << filePath << std::endl;
        return false;
    }
    
    // Get basic document information
    OWPML::CHWPMLHeadType* header = document->GetHead();
    if (header) {
        std::wcout << L"Document version: " << header->GetVersion() << std::endl;
        std::wcout << L"Number of sections: " << header->GetSecCnt() << std::endl;
    }
    
    // Get sections
    std::vector<OWPML::CSectionType*>* sections = document->GetSections();
    if (sections) {
        std::wcout << L"Found " << sections->size() << L" sections" << std::endl;
        
        for (size_t i = 0; i < sections->size(); i++) {
            OWPML::CSectionType* section = sections->at(i);
            int childCount = section->GetChildCount();
            std::wcout << L"Section " << i << L" has " << childCount << L" child elements" << std::endl;
        }
    }
    
    // Clean up
    delete document;
    return true;
}

int main() {
    OpenAndInspectDocument(L"C:\\Documents\\sample.hwpx");
    return 0;
}
```

### Saving a Document

```cpp
bool SaveDocumentAs(OWPML::COwpmlDocumnet* document, const std::wstring& outputPath) {
    if (!document) {
        return false;
    }
    
    BOOL result = document->Save(outputPath.c_str());
    if (result) {
        std::wcout << L"Document saved successfully to: " << outputPath << std::endl;
        return true;
    } else {
        std::wcout << L"Failed to save document to: " << outputPath << std::endl;
        return false;
    }
}
```

## Text Extraction

### Complete Text Extraction

```cpp
void ExtractAllText(OWPML::CObject* object, std::wstring& result) {
    if (!object) return;
    
    // Check if this is a text element
    if (object->GetID() == ID_PARA_T) {
        OWPML::CT* textElement = static_cast<OWPML::CT*>(object);
        if (textElement) {
            const wchar_t* text = textElement->GetText();
            if (text && wcslen(text) > 0) {
                result += text;
            }
        }
    }
    
    // Check for character elements
    else if (object->GetID() == ID_PARA_Char) {
        OWPML::CChar* charElement = static_cast<OWPML::CChar*>(object);
        if (charElement) {
            const wchar_t* charValue = charElement->Getval();
            if (charValue) {
                result += charValue;
            }
        }
    }
    
    // Check for line breaks
    else if (object->GetID() == ID_PARA_LineSeg) {
        result += L"\n";
    }
    
    // Check for tabs
    else if (object->GetID() == ID_PARA_Tab) {
        result += L"\t";
    }
    
    // Recursively process child objects
    OWPML::Objectlist* children = object->GetObjectList();
    if (children) {
        for (auto child : *children) {
            ExtractAllText(child, result);
        }
    }
}

std::wstring ExtractTextFromDocument(const std::wstring& filePath) {
    std::wstring result;
    
    OWPML::COwpmlDocumnet* document = OWPML::COwpmlDocumnet::OpenDocument(filePath.c_str());
    if (!document) {
        return result;
    }
    
    std::vector<OWPML::CSectionType*>* sections = document->GetSections();
    if (sections) {
        for (auto section : *sections) {
            ExtractAllText(section, result);
        }
    }
    
    delete document;
    return result;
}

// Usage
int main() {
    std::wstring text = ExtractTextFromDocument(L"document.hwpx");
    
    // Save to text file
    std::wofstream outFile(L"extracted_text.txt");
    outFile.imbue(std::locale(std::locale(), new std::codecvt_utf8_utf16<wchar_t>));
    outFile << text;
    outFile.close();
    
    return 0;
}
```

### Paragraph-by-Paragraph Text Extraction

```cpp
struct ParagraphInfo {
    std::wstring text;
    int paragraphIndex;
    int sectionIndex;
};

std::vector<ParagraphInfo> ExtractParagraphs(const std::wstring& filePath) {
    std::vector<ParagraphInfo> paragraphs;
    
    OWPML::COwpmlDocumnet* document = OWPML::COwpmlDocumnet::OpenDocument(filePath.c_str());
    if (!document) {
        return paragraphs;
    }
    
    std::vector<OWPML::CSectionType*>* sections = document->GetSections();
    if (sections) {
        for (size_t sectionIdx = 0; sectionIdx < sections->size(); sectionIdx++) {
            OWPML::CSectionType* section = sections->at(sectionIdx);
            
            int paraCount = section->GetChildCount();
            for (int paraIdx = 0; paraIdx < paraCount; paraIdx++) {
                OWPML::CObject* child = section->GetObjectByIndex(paraIdx);
                if (child && child->GetID() == ID_PARA_PType) {
                    OWPML::CPType* paragraph = static_cast<OWPML::CPType*>(child);
                    
                    ParagraphInfo paraInfo;
                    paraInfo.sectionIndex = static_cast<int>(sectionIdx);
                    paraInfo.paragraphIndex = paraIdx;
                    
                    ExtractAllText(paragraph, paraInfo.text);
                    
                    if (!paraInfo.text.empty()) {
                        paragraphs.push_back(paraInfo);
                    }
                }
            }
        }
    }
    
    delete document;
    return paragraphs;
}
```

## Document Creation

### Creating a Simple Document

```cpp
OWPML::COwpmlDocumnet* CreateSimpleDocument(const std::wstring& content) {
    // Create new document
    OWPML::COwpmlDocumnet* document = OWPML::COwpmlDocumnet::CreateDocument();
    if (!document) {
        return nullptr;
    }
    
    // Create a section
    OWPML::CSectionType* section = OWPML::CSectionType::Create();
    if (!section) {
        delete document;
        return nullptr;
    }
    
    // Create paragraph
    OWPML::CPType* paragraph = section->Setp();
    if (!paragraph) {
        delete document;
        return nullptr;
    }
    
    // Create text run
    OWPML::CRunType* run = paragraph->Setrun();
    if (!run) {
        delete document;
        return nullptr;
    }
    
    // Add text
    OWPML::CT* text = run->Sett();
    if (text) {
        text->SetText(content.c_str());
    }
    
    return document;
}

// Usage
int main() {
    OWPML::COwpmlDocumnet* doc = CreateSimpleDocument(L"Hello, HWPX World!");
    if (doc) {
        doc->Save(L"new_document.hwpx");
        delete doc;
    }
    return 0;
}
```

### Creating a Multi-Paragraph Document

```cpp
OWPML::COwpmlDocumnet* CreateMultiParagraphDocument(const std::vector<std::wstring>& paragraphs) {
    OWPML::COwpmlDocumnet* document = OWPML::COwpmlDocumnet::CreateDocument();
    if (!document) return nullptr;
    
    OWPML::CSectionType* section = OWPML::CSectionType::Create();
    if (!section) {
        delete document;
        return nullptr;
    }
    
    // Add each paragraph
    for (const auto& paragraphText : paragraphs) {
        OWPML::CPType* paragraph = section->Setp();
        if (paragraph) {
            OWPML::CRunType* run = paragraph->Setrun();
            if (run) {
                OWPML::CT* text = run->Sett();
                if (text) {
                    text->SetText(paragraphText.c_str());
                }
            }
        }
    }
    
    return document;
}

// Usage
int main() {
    std::vector<std::wstring> paragraphs = {
        L"First paragraph of the document.",
        L"Second paragraph with different content.",
        L"Third paragraph to demonstrate multiple paragraphs."
    };
    
    OWPML::COwpmlDocumnet* doc = CreateMultiParagraphDocument(paragraphs);
    if (doc) {
        doc->Save(L"multi_paragraph_document.hwpx");
        delete doc;
    }
    
    return 0;
}
```

## Working with Styles

### Examining Document Styles

```cpp
void AnalyzeDocumentStyles(OWPML::COwpmlDocumnet* document) {
    OWPML::CHWPMLHeadType* header = document->GetHead();
    if (!header) return;
    
    // Analyze font faces
    std::wcout << L"=== Font Faces ===" << std::endl;
    int fontIndex = 0;
    while (true) {
        OWPML::CFontfaceType* fontface = header->GetFontfaces(fontIndex);
        if (!fontface) break;
        
        std::wcout << L"Font " << fontIndex << L": " << fontface->GetName() << std::endl;
        fontIndex++;
    }
    
    // Analyze character shapes
    std::wcout << L"\n=== Character Shapes ===" << std::endl;
    int charShapeIndex = 0;
    while (true) {
        OWPML::CCharShapeType* charShape = header->GetCharshapes(charShapeIndex);
        if (!charShape) break;
        
        std::wcout << L"CharShape " << charShapeIndex << L":" << std::endl;
        std::wcout << L"  Font: " << charShape->GetFontRef() << std::endl;
        std::wcout << L"  Size: " << charShape->GetSize() << std::endl;
        std::wcout << L"  Bold: " << (charShape->GetBold() ? L"Yes" : L"No") << std::endl;
        std::wcout << L"  Italic: " << (charShape->GetItalic() ? L"Yes" : L"No") << std::endl;
        
        charShapeIndex++;
    }
    
    // Analyze paragraph shapes
    std::wcout << L"\n=== Paragraph Shapes ===" << std::endl;
    int paraShapeIndex = 0;
    while (true) {
        OWPML::CParaShapeType* paraShape = header->GetParashapes(paraShapeIndex);
        if (!paraShape) break;
        
        std::wcout << L"ParaShape " << paraShapeIndex << L":" << std::endl;
        std::wcout << L"  Alignment: " << paraShape->GetAlign() << std::endl;
        std::wcout << L"  Line Spacing: " << paraShape->GetLineSpacing() << std::endl;
        
        paraShapeIndex++;
    }
}
```

### Creating Custom Styles

```cpp
void SetupDocumentStyles(OWPML::COwpmlDocumnet* document) {
    OWPML::CHWPMLHeadType* header = document->GetHead();
    if (!header) return;
    
    // Add custom font
    OWPML::CFontfaceType* customFont = header->SetFontfaces();
    if (customFont) {
        customFont->SetName(L"Arial");
        customFont->SetType(L"ttf");
    }
    
    // Add character shape
    OWPML::CCharShapeType* charShape = header->SetCharshapes();
    if (charShape) {
        charShape->SetFontRef(L"Arial");
        charShape->SetSize(12);
        charShape->SetBold(TRUE);
        charShape->SetItalic(FALSE);
    }
    
    // Add paragraph shape
    OWPML::CParaShapeType* paraShape = header->SetParashapes();
    if (paraShape) {
        paraShape->SetAlign(1); // Center alignment
        paraShape->SetLineSpacing(120); // 120% line spacing
    }
}
```

## Image and Binary Data

### Extracting Binary Data Information

```cpp
void ListBinaryData(OWPML::COwpmlDocumnet* document) {
    // This requires access to the low-level serialization API
    // Note: This is a conceptual example - actual implementation may vary
    
    std::wcout << L"=== Binary Data Analysis ===" << std::endl;
    
    // You would typically use COwpmlSerialize for this
    // This demonstrates the concept
    std::wcout << L"Note: Binary data extraction requires low-level API access" << std::endl;
    std::wcout << L"Images and objects are stored in BinData/ directory" << std::endl;
    std::wcout << L"Use COwpmlSerialize::ReadBindata() for actual extraction" << std::endl;
}
```

### Finding Images in Document

```cpp
void FindImages(OWPML::CObject* object, std::vector<OWPML::CPictureType*>& images) {
    if (!object) return;
    
    // Check if this object is an image
    if (object->GetID() == ID_PARA_PictureType) {
        OWPML::CPictureType* picture = static_cast<OWPML::CPictureType*>(object);
        images.push_back(picture);
    }
    
    // Recursively search child objects
    OWPML::Objectlist* children = object->GetObjectList();
    if (children) {
        for (auto child : *children) {
            FindImages(child, images);
        }
    }
}

std::vector<OWPML::CPictureType*> GetAllImages(OWPML::COwpmlDocumnet* document) {
    std::vector<OWPML::CPictureType*> images;
    
    std::vector<OWPML::CSectionType*>* sections = document->GetSections();
    if (sections) {
        for (auto section : *sections) {
            FindImages(section, images);
        }
    }
    
    return images;
}
```

## Tables and Objects

### Finding and Analyzing Tables

```cpp
struct TableInfo {
    int sectionIndex;
    int rows;
    int columns;
    std::wstring firstCellText;
};

void AnalyzeTable(OWPML::CTableType* table, TableInfo& info) {
    if (!table) return;
    
    // Get table properties
    // Note: Actual property access depends on the specific table implementation
    info.rows = 0;
    info.columns = 0;
    
    // Count rows and columns by examining table structure
    // This is a simplified example - actual implementation would be more complex
    OWPML::Objectlist* children = table->GetObjectList();
    if (children) {
        for (auto child : *children) {
            if (child->GetID() == ID_PARA_TableRowType) {
                info.rows++;
                
                // Count columns in first row
                if (info.rows == 1) {
                    OWPML::Objectlist* rowChildren = child->GetObjectList();
                    if (rowChildren) {
                        for (auto rowChild : *rowChildren) {
                            if (rowChild->GetID() == ID_PARA_TableCellType) {
                                info.columns++;
                            }
                        }
                    }
                }
            }
        }
    }
}

std::vector<TableInfo> FindAllTables(OWPML::COwpmlDocumnet* document) {
    std::vector<TableInfo> tables;
    
    std::vector<OWPML::CSectionType*>* sections = document->GetSections();
    if (sections) {
        for (size_t sectionIdx = 0; sectionIdx < sections->size(); sectionIdx++) {
            OWPML::CSectionType* section = sections->at(sectionIdx);
            
            // Recursively find tables in this section
            FindTablesInObject(section, tables, static_cast<int>(sectionIdx));
        }
    }
    
    return tables;
}

void FindTablesInObject(OWPML::CObject* object, std::vector<TableInfo>& tables, int sectionIndex) {
    if (!object) return;
    
    if (object->GetID() == ID_PARA_TableType) {
        OWPML::CTableType* table = static_cast<OWPML::CTableType*>(object);
        
        TableInfo info;
        info.sectionIndex = sectionIndex;
        AnalyzeTable(table, info);
        tables.push_back(info);
    }
    
    // Recursively search children
    OWPML::Objectlist* children = object->GetObjectList();
    if (children) {
        for (auto child : *children) {
            FindTablesInObject(child, tables, sectionIndex);
        }
    }
}
```

## Advanced Processing

### Document Comparison

```cpp
struct DocumentStats {
    int sectionCount;
    int paragraphCount;
    int textElements;
    int imageCount;
    int tableCount;
    size_t totalTextLength;
};

DocumentStats AnalyzeDocument(OWPML::COwpmlDocumnet* document) {
    DocumentStats stats = {0};
    
    std::vector<OWPML::CSectionType*>* sections = document->GetSections();
    if (sections) {
        stats.sectionCount = static_cast<int>(sections->size());
        
        for (auto section : *sections) {
            AnalyzeObject(section, stats);
        }
    }
    
    return stats;
}

void AnalyzeObject(OWPML::CObject* object, DocumentStats& stats) {
    if (!object) return;
    
    switch (object->GetID()) {
        case ID_PARA_PType:
            stats.paragraphCount++;
            break;
            
        case ID_PARA_T:
            stats.textElements++;
            {
                OWPML::CT* textElement = static_cast<OWPML::CT*>(object);
                const wchar_t* text = textElement->GetText();
                if (text) {
                    stats.totalTextLength += wcslen(text);
                }
            }
            break;
            
        case ID_PARA_PictureType:
            stats.imageCount++;
            break;
            
        case ID_PARA_TableType:
            stats.tableCount++;
            break;
    }
    
    // Recursively analyze children
    OWPML::Objectlist* children = object->GetObjectList();
    if (children) {
        for (auto child : *children) {
            AnalyzeObject(child, stats);
        }
    }
}

void CompareDocuments(const std::wstring& file1, const std::wstring& file2) {
    OWPML::COwpmlDocumnet* doc1 = OWPML::COwpmlDocumnet::OpenDocument(file1.c_str());
    OWPML::COwpmlDocumnet* doc2 = OWPML::COwpmlDocumnet::OpenDocument(file2.c_str());
    
    if (!doc1 || !doc2) {
        std::wcout << L"Failed to open one or both documents" << std::endl;
        return;
    }
    
    DocumentStats stats1 = AnalyzeDocument(doc1);
    DocumentStats stats2 = AnalyzeDocument(doc2);
    
    std::wcout << L"Document Comparison:" << std::endl;
    std::wcout << L"  Document 1: " << file1 << std::endl;
    std::wcout << L"    Sections: " << stats1.sectionCount << std::endl;
    std::wcout << L"    Paragraphs: " << stats1.paragraphCount << std::endl;
    std::wcout << L"    Text Length: " << stats1.totalTextLength << std::endl;
    std::wcout << L"    Images: " << stats1.imageCount << std::endl;
    std::wcout << L"    Tables: " << stats1.tableCount << std::endl;
    
    std::wcout << L"  Document 2: " << file2 << std::endl;
    std::wcout << L"    Sections: " << stats2.sectionCount << std::endl;
    std::wcout << L"    Paragraphs: " << stats2.paragraphCount << std::endl;
    std::wcout << L"    Text Length: " << stats2.totalTextLength << std::endl;
    std::wcout << L"    Images: " << stats2.imageCount << std::endl;
    std::wcout << L"    Tables: " << stats2.tableCount << std::endl;
    
    delete doc1;
    delete doc2;
}
```

### Batch Processing

```cpp
#include <filesystem>
namespace fs = std::filesystem;

void ProcessHwpxDirectory(const std::wstring& directoryPath) {
    try {
        for (const auto& entry : fs::directory_iterator(directoryPath)) {
            if (entry.is_regular_file()) {
                std::wstring filePath = entry.path().wstring();
                
                // Check if it's a HWPX file
                if (filePath.ends_with(L".hwpx")) {
                    std::wcout << L"Processing: " << filePath << std::endl;
                    
                    // Extract text
                    std::wstring text = ExtractTextFromDocument(filePath);
                    
                    // Save extracted text
                    std::wstring outputPath = filePath + L".txt";
                    std::wofstream outFile(outputPath);
                    outFile.imbue(std::locale(std::locale(), new std::codecvt_utf8_utf16<wchar_t>));
                    outFile << text;
                    outFile.close();
                    
                    std::wcout << L"  Extracted text to: " << outputPath << std::endl;
                }
            }
        }
    } catch (const fs::filesystem_error& ex) {
        std::wcout << L"Filesystem error: " << ex.what() << std::endl;
    }
}
```

## Error Handling Best Practices

```cpp
class DocumentProcessor {
private:
    std::wstring m_lastError;
    
public:
    bool ProcessDocument(const std::wstring& inputPath, const std::wstring& outputPath) {
        // Clear previous error
        m_lastError.clear();
        
        // Validate input
        if (inputPath.empty() || outputPath.empty()) {
            m_lastError = L"Invalid file paths provided";
            return false;
        }
        
        // Check if input file exists
        if (!fs::exists(inputPath)) {
            m_lastError = L"Input file does not exist: " + inputPath;
            return false;
        }
        
        // Open document
        OWPML::COwpmlDocumnet* document = OWPML::COwpmlDocumnet::OpenDocument(inputPath.c_str());
        if (!document) {
            m_lastError = L"Failed to open document: " + inputPath;
            return false;
        }
        
        // Ensure cleanup
        std::unique_ptr<OWPML::COwpmlDocumnet, decltype(&DeleteDocument)> 
            docGuard(document, DeleteDocument);
        
        // Process document
        try {
            std::wstring text = ExtractTextFromDocument(inputPath);
            
            // Save result
            std::wofstream outFile(outputPath);
            if (!outFile.is_open()) {
                m_lastError = L"Failed to create output file: " + outputPath;
                return false;
            }
            
            outFile.imbue(std::locale(std::locale(), new std::codecvt_utf8_utf16<wchar_t>));
            outFile << text;
            outFile.close();
            
            return true;
            
        } catch (const std::exception& ex) {
            m_lastError = L"Processing error: " + std::wstring(ex.what(), ex.what() + strlen(ex.what()));
            return false;
        }
    }
    
    const std::wstring& GetLastError() const {
        return m_lastError;
    }
    
private:
    static void DeleteDocument(OWPML::COwpmlDocumnet* doc) {
        delete doc;
    }
};
```

These examples demonstrate the most common use cases for the OWPML library. Remember to always handle memory management properly and check return values for error conditions.