# HWPX Object Type Reference

This document provides detailed XML specifications for every object type in the HWPX document model, focusing on XML elements, attributes, namespaces, and schema structure.

## XML Namespaces and Schema Overview

HWPX documents are built using the OWPML (Open Word processing ML) XML schema with multiple namespaces. Understanding these namespaces is critical for working with HWPX objects:

### Primary OWPML Namespaces

| Prefix | Namespace URI | Purpose | Example Elements |
|--------|---------------|---------|------------------|
| `hh` | `http://www.hancom.co.kr/hwpml/2011/head` | Header definitions | `<hh:head>`, `<hh:fontface>`, `<hh:style>` |
| `hs` | `http://www.hancom.co.kr/hwpml/2011/section` | Section structure | `<hs:sec>` |
| `hp` | `http://www.hancom.co.kr/hwpml/2011/paragraph` | Paragraph content | `<hp:p>`, `<hp:run>`, `<hp:t>` |
| `hc` | `http://www.hancom.co.kr/hwpml/2011/core` | Core elements | `<hc:sz>`, `<hc:pos>` |
| `he` | `http://www.hancom.co.kr/hwpml/2011/etc` | Miscellaneous elements | `<he:reference>` |

### Standard XML Namespaces

| Prefix | Namespace URI | Purpose |
|--------|---------------|---------|
| `opf` | `http://www.idpf.org/2007/opf/` | Open Packaging Format |
| `dc` | `http://purl.org/dc/elements/1.1/` | Dublin Core metadata |
| `odf` | `urn:oasis:names:tc:opendocument:xmlns:manifest:1.0` | ODF manifest |
| `ocf` | `urn:oasis:names:tc:opendocument:xmlns:container` | OCF container |

### XML Schema Structure

The HWPX XML schema follows a hierarchical structure:

```xml
<!-- Document root with namespace declarations -->
<hh:head xmlns:hh="http://www.hancom.co.kr/hwpml/2011/head" 
         xmlns:hp="http://www.hancom.co.kr/hwpml/2011/paragraph"
         xmlns:hs="http://www.hancom.co.kr/hwpml/2011/section"
         xmlns:hc="http://www.hancom.co.kr/hwpml/2011/core"
         version="1.31" secCnt="1">
    
    <!-- Document-level definitions -->
    <hh:fontfaces>...</hh:fontfaces>
    <hh:charshapes>...</hh:charshapes>
    <hh:parashapes>...</hh:parashapes>
    <hh:styles>...</hh:styles>
</hh:head>
```

## Head Objects (Document-Level XML Definitions)

### HWPMLHeadType
**XML Element**: `<hh:head>`  
**XML Namespace**: `http://www.hancom.co.kr/hwpml/2011/head`  
**Purpose**: Root container for all document-level definitions including styles, fonts, and global settings.

**XML Schema Definition**:
```xml
<hh:head xmlns:hh="http://www.hancom.co.kr/hwpml/2011/head" 
         version="1.31" 
         secCnt="1">
    <hh:beginNum>...</hh:beginNum>
    <hh:refList>...</hh:refList>
    <hh:fontfaces>...</hh:fontfaces>
    <hh:borderfills>...</hh:borderfills>
    <hh:charshapes>...</hh:charshapes>
    <hh:parashapes>...</hh:parashapes>
    <hh:styles>...</hh:styles>
    <hh:numberings>...</hh:numberings>
    <hh:bullets>...</hh:bullets>
</hh:head>
```

**XML Attributes**:
| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `version` | string | Yes | OWPML format version (e.g., "1.31") |
| `secCnt` | integer | Yes | Number of sections in the document |

**XML Child Elements**:
| Element | XML Tag | Occurrence | Description |
|---------|---------|------------|-------------|
| Begin Numbers | `<hh:beginNum>` | 0..1 | Starting numbers for various counters |
| Reference List | `<hh:refList>` | 0..1 | Reference mappings and lookup tables |
| Forbidden Words | `<hh:forbiddenWordList>` | 0..1 | Words that cannot be hyphenated |
| Compatible Document | `<hh:compatibleDocument>` | 0..1 | Compatibility settings |
| Document Options | `<hh:docOption>` | 0..1 | Document behavior options |
| Track Changes | `<hh:trackChangeConfig>` | 0..1 | Track changes configuration |
| Font Faces | `<hh:fontfaces>` | 1 | Font definitions collection |
| Border Fills | `<hh:borderfills>` | 1 | Border and fill style definitions |
| Character Shapes | `<hh:charshapes>` | 1 | Character formatting definitions |
| Paragraph Shapes | `<hh:parashapes>` | 1 | Paragraph formatting definitions |
| Styles | `<hh:styles>` | 1 | Named style definitions |
| Numberings | `<hh:numberings>` | 0..1 | Numbering scheme definitions |
| Bullets | `<hh:bullets>` | 0..1 | Bullet style definitions |
| Memo Properties | `<hh:memoProperties>` | 0..1 | Comment and annotation properties |

### FontfaceType
**XML Element**: `<hh:fontface>`  
**XML Namespace**: `http://www.hancom.co.kr/hwpml/2011/head`  
**Parent Element**: `<hh:fontfaces>`  
**Purpose**: Defines a font family with language-specific variants and fallback options.

**XML Schema Definition**:
```xml
<hh:fontface name="맑은 고딕" type="ttf">
    <hh:font lang="HANGUL" name="맑은 고딕"/>
    <hh:font lang="LATIN" name="Malgun Gothic"/>
    <hh:font lang="HANJA" name="맑은 고딕"/>
    <hh:font lang="JAPANESE" name="Malgun Gothic"/>
    <hh:font lang="OTHER" name="Malgun Gothic"/>
    <hh:font lang="SYMBOL" name="Malgun Gothic"/>
    <hh:font lang="USER" name="Malgun Gothic"/>
    <hh:substFont type="ttf" name="Arial"/>
    <hh:typeInfo familyType="2" serifType="11" weight="5" 
                 proportion="2" contrast="4" strokeVariation="5" 
                 armStyle="5" letterform="2" midline="2" xHeight="4"/>
</hh:fontface>
```

**XML Attributes**:
| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Font family name |
| `type` | enum | Yes | Font type: "ttf", "otf", "woff", "eot" |

**XML Child Elements**:
| Element | XML Tag | Occurrence | Description |
|---------|---------|------------|-------------|
| Font Definition | `<hh:font>` | 1..* | Individual font per language |
| Substitute Font | `<hh:substFont>` | 0..1 | Fallback font mapping |
| Type Information | `<hh:typeInfo>` | 0..1 | Font metrics and characteristics |

**Language-Specific Font Support**:
The `<hh:font>` element supports different languages via the `lang` attribute:
- `HANGUL`: Korean characters
- `LATIN`: Western/Latin characters  
- `HANJA`: Chinese characters
- `JAPANESE`: Japanese characters
- `SYMBOL`: Symbol characters
- `OTHER`: Other character sets
- `USER`: User-defined characters

### CharShapeType
**XML Element**: `<hh:charshape>`  
**XML Namespace**: `http://www.hancom.co.kr/hwpml/2011/head`  
**Parent Element**: `<hh:charshapes>`  
**Purpose**: Defines character-level formatting that can be applied to text runs.

**XML Schema Definition**:
```xml
<hh:charshape id="0" height="1000" textColor="0" shadeColor="4294967295" 
              useFontSpace="false" useKerning="false" symMark="0">
    <hh:fontRef hangul="0" latin="0" hanja="0" japanese="0" 
                other="0" symbol="0" user="0"/>
    <hh:ratio hangul="100" latin="100" hanja="100" japanese="100" 
              other="100" symbol="100" user="100"/>
    <hh:spacing hangul="0" latin="0" hanja="0" japanese="0" 
                other="0" symbol="0" user="0"/>
    <hh:relSz hangul="100" latin="100" hanja="100" japanese="100" 
              other="100" symbol="100" user="100"/>
    <hh:offset hangul="0" latin="0" hanja="0" japanese="0" 
               other="0" symbol="0" user="0"/>
    <hh:italic hangul="false" latin="false" hanja="false" japanese="false" 
               other="false" symbol="false" user="false"/>
    <hh:bold hangul="false" latin="false" hanja="false" japanese="false" 
             other="false" symbol="false" user="false"/>
    <hh:underline type="none" shape="straight" color="0"/>
    <hh:strikeout type="none" shape="straight" color="0"/>
    <hh:outline type="none"/>
    <hh:shadow type="none" color="0" offsetX="283" offsetY="283"/>
    <hh:emboss type="none"/>
    <hh:engrave type="none"/>
    <hh:supscript type="none"/>
    <hh:subscript type="none"/>
</hh:charshape>
```

**XML Attributes**:
| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | integer | Yes | Unique identifier for referencing |
| `height` | integer | Yes | Base font size in HWPML units (1000 = 10pt) |
| `textColor` | integer | No | Text color as RGB integer |
| `shadeColor` | integer | No | Background color for text |
| `useFontSpace` | boolean | No | Whether to use font-specific spacing |
| `useKerning` | boolean | No | Whether to apply kerning |
| `symMark` | integer | No | Symbol mark type for diacritics |

**XML Child Elements** (all language-specific):
| Element | XML Tag | Attributes | Description |
|---------|---------|------------|-------------|
| Font References | `<hh:fontRef>` | hangul, latin, hanja, japanese, other, symbol, user | References to font faces |
| Size Ratios | `<hh:ratio>` | hangul, latin, hanja, japanese, other, symbol, user | Width/height scaling ratios |
| Character Spacing | `<hh:spacing>` | hangul, latin, hanja, japanese, other, symbol, user | Character spacing adjustments |
| Relative Sizes | `<hh:relSz>` | hangul, latin, hanja, japanese, other, symbol, user | Relative size adjustments |
| Baseline Offsets | `<hh:offset>` | hangul, latin, hanja, japanese, other, symbol, user | Baseline offset adjustments |
| Italic Style | `<hh:italic>` | hangul, latin, hanja, japanese, other, symbol, user | Italic formatting per language |
| Bold Style | `<hh:bold>` | hangul, latin, hanja, japanese, other, symbol, user | Bold formatting per language |
| Underline | `<hh:underline>` | type, shape, color | Underline style definition |
| Strikeout | `<hh:strikeout>` | type, shape, color | Strikethrough formatting |
| Outline | `<hh:outline>` | type | Text outline effects |
| Shadow | `<hh:shadow>` | type, color, offsetX, offsetY | Text shadow effects |
| Emboss | `<hh:emboss>` | type | Embossed text effects |
| Engrave | `<hh:engrave>` | type | Engraved text effects |
| Superscript | `<hh:supscript>` | type | Superscript positioning |
| Subscript | `<hh:subscript>` | type | Subscript positioning |

### ParaShapeType
**XML Element**: `<hh:parashape>`  
**XML Namespace**: `http://www.hancom.co.kr/hwpml/2011/head`  
**Parent Element**: `<hh:parashapes>`  
**Purpose**: Defines paragraph-level formatting including alignment, spacing, and indentation.

**XML Schema Definition**:
```xml
<hh:parashape id="0" align="LEFT" heading="0" breakSetting="0" 
              condense="0" fontLineHeight="false" snapToGrid="false" 
              suppressLineNumbers="false" checked="false">
    <hh:margin left="0" right="0" first="0" hanging="0"/>
    <hh:lineSpacing type="PERCENT" value="160" unit="PERCENT"/>
    <hh:border borderFillIDRef="1"/>
    <hh:autoSpacing eParaHeadTail="false" eParaTail="false" 
                    eParaHead="false" eParaMiddle="false"/>
</hh:parashape>
```

**XML Attributes**:
| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | integer | Yes | Unique identifier |
| `align` | enum | No | Text alignment: "LEFT", "CENTER", "RIGHT", "JUSTIFY", "DISTRIBUTE" |
| `heading` | integer | No | Outline heading level |
| `breakSetting` | integer | No | Page and column break behavior flags |
| `condense` | integer | No | Text condensation percentage |
| `fontLineHeight` | boolean | No | Line height calculation method |
| `snapToGrid` | boolean | No | Whether to snap to document grid |
| `suppressLineNumbers` | boolean | No | Whether to suppress line numbering |
| `checked` | boolean | No | Checkbox state for list items |

**XML Child Elements**:
| Element | XML Tag | Attributes | Description |
|---------|---------|------------|-------------|
| Margins | `<hh:margin>` | left, right, first, hanging | Paragraph margin specifications |
| Line Spacing | `<hh:lineSpacing>` | type, value, unit | Line spacing definitions |
| Border | `<hh:border>` | borderFillIDRef | Reference to border/fill definition |
| Auto Spacing | `<hh:autoSpacing>` | eParaHeadTail, eParaTail, eParaHead, eParaMiddle | Automatic spacing between paragraphs |

### BorderFillType
**XML Element**: `<hh:borderfill>`  
**XML Namespace**: `http://www.hancom.co.kr/hwpml/2011/head`  
**Parent Element**: `<hh:borderfills>`  
**Purpose**: Defines border and background fill styles for various elements.

**XML Schema Definition**:
```xml
<hh:borderfill id="1" threeD="false" shadow="false">
    <hh:slash type="NONE" crooked="false" isCounter="false"/>
    <hh:leftBorder type="NONE" width="0" color="0"/>
    <hh:rightBorder type="NONE" width="0" color="0"/>
    <hh:topBorder type="NONE" width="0" color="0"/>
    <hh:bottomBorder type="NONE" width="0" color="0"/>
    <hh:diagonal type="NONE" width="0" color="0"/>
    <hh:fillBrush>
        <hh:windowBrush faceColor="4294967295" hatchColor="0" hatchStyle="HS_HORIZONTAL"/>
    </hh:fillBrush>
</hh:borderfill>
```

**XML Attributes**:
| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | integer | Yes | Unique identifier |
| `threeD` | boolean | No | 3D border effect settings |
| `shadow` | boolean | No | Shadow effect settings |

**XML Child Elements**:
| Element | XML Tag | Attributes | Description |
|---------|---------|------------|-------------|
| Slash Pattern | `<hh:slash>` | type, crooked, isCounter | Diagonal line patterns |
| Border Sides | `<hh:leftBorder>`, `<hh:rightBorder>`, `<hh:topBorder>`, `<hh:bottomBorder>` | type, width, color | Individual border definitions |
| Diagonal | `<hh:diagonal>` | type, width, color | Diagonal border |
| Fill Brush | `<hh:fillBrush>` | - | Background fill definition |

**Border Line Types**: `NONE`, `SOLID`, `THICK`, `DOUBLE`, `DOTTED`, `DASHED`, `DASHDOTTED`, `DASHDOTDOTTED`, `LONGDASH`, `CIRCLED`, `DOUBLE_SLIM`, `SLIM_THICK`, `THICK_SLIM`, `SLIM_THICK_SLIM`, `WAVE`, `DOUBLE_WAVE`, `THICK_3D`, `THICK_3D_LOWERED`, `3D`, `3D_LOWERED`

### StyleType
**XML Element**: `<hh:style>`  
**XML Namespace**: `http://www.hancom.co.kr/hwpml/2011/head`  
**Parent Element**: `<hh:styles>`  
**Purpose**: Named style definitions that combine character and paragraph formatting.

**XML Schema Definition**:
```xml
<hh:style id="0" type="PARA" name="본문" engName="Normal" 
          paraPr="0" charPr="0" nextStyle="0" langId="1042" lockForm="false">
    <!-- Style properties are referenced through paraPr and charPr IDs -->
</hh:style>
```

**XML Attributes**:
| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | integer | Yes | Unique identifier |
| `type` | enum | Yes | Style type: "PARA" (paragraph), "CHAR" (character) |
| `name` | string | Yes | User-visible style name |
| `engName` | string | No | English name for the style |
| `paraPr` | integer | No | Reference to paragraph shape ID |
| `charPr` | integer | No | Reference to character shape ID |
| `nextStyle` | integer | No | Style ID to apply to following paragraphs |
| `langId` | integer | No | Language identifier (1042 = Korean) |
| `lockForm` | boolean | No | Whether style formatting is locked |

### NumberingType
**XML Element**: `<hh:numbering>`  
**XML Namespace**: `http://www.hancom.co.kr/hwpml/2011/head`  
**Parent Element**: `<hh:numberings>`  
**Purpose**: Defines numbering schemes for ordered lists and outline numbering.

**XML Schema Definition**:
```xml
<hh:numbering id="0">
    <hh:paraHead level="0" align="LEFT" useInstWidth="false" 
                 autoIndent="true" widthAdjust="false" 
                 textOffsetType="PERCENT" textOffset="50" 
                 numFormat="DIGIT" charShape="0">
        <hh:start value="1"/>
    </hh:paraHead>
    <hh:paraHead level="1" align="LEFT" useInstWidth="false" 
                 autoIndent="true" widthAdjust="false" 
                 textOffsetType="PERCENT" textOffset="50" 
                 numFormat="CIRCLED_DIGIT" charShape="0">
        <hh:start value="1"/>
    </hh:paraHead>
    <!-- Additional levels as needed -->
</hh:numbering>
```

**XML Attributes**:
| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | integer | Yes | Unique identifier |

**XML Child Elements**:
| Element | XML Tag | Attributes | Description |
|---------|---------|------------|-------------|
| Paragraph Head | `<hh:paraHead>` | level, align, useInstWidth, autoIndent, widthAdjust, textOffsetType, textOffset, numFormat, charShape | Numbering definition for each level |
| Start Number | `<hh:start>` | value | Starting number for the level |

**Numbering Formats** (numFormat attribute):
- `DIGIT`: Decimal (1, 2, 3...)
- `CIRCLED_DIGIT`: Circled decimal (①, ②, ③...)
- `ROMAN_CAPITAL`: Upper Roman (I, II, III...)
- `ROMAN_SMALL`: Lower Roman (i, ii, iii...)
- `LETTER_CAPITAL`: Upper letters (A, B, C...)
- `LETTER_SMALL`: Lower letters (a, b, c...)
- `HANGUL_SYLLABLE`: Korean syllables (가, 나, 다...)
- `HANGUL_JAMO`: Korean jamo (ㄱ, ㄴ, ㄷ...)
- `HANGUL_PHONETIC`: Korean phonetic
- `IDEOGRAPH_DIGITAL`: Chinese numerals

### BulletType
**XML Element**: `<hh:bullet>`  
**XML Namespace**: `http://www.hancom.co.kr/hwpml/2011/head`  
**Parent Element**: `<hh:bullets>`  
**Purpose**: Defines bullet styles for unordered lists.

**XML Schema Definition**:
```xml
<hh:bullet id="0" char="●" checkedChar="☑" useImage="false">
    <hh:img src="BIN0001.png" embedded="true" binaryItem="BIN0001"/>
</hh:bullet>
```

**XML Attributes**:
| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | integer | Yes | Unique identifier |
| `char` | string | No | Bullet character (Unicode) |
| `checkedChar` | string | No | Character for checked items |
| `useImage` | boolean | No | Whether to use image bullets |

**XML Child Elements**:
| Element | XML Tag | Attributes | Description |
|---------|---------|------------|-------------|
| Image Bullet | `<hh:img>` | src, embedded, binaryItem | Image source for custom bullets |

## Section Objects (Document Structure XML)

### SectionType
**XML Element**: `<hs:sec>`  
**XML Namespace**: `http://www.hancom.co.kr/hwpml/2011/section`  
**Purpose**: Major document divisions with independent page layout and content flow.

**XML Schema Definition**:
```xml
<hs:sec xmlns:hs="http://www.hancom.co.kr/hwpml/2011/section"
        xmlns:hp="http://www.hancom.co.kr/hwpml/2011/paragraph">
    <hs:secPr>
        <hs:pageSz width="59528" height="84188"/>
        <hs:pageMargin left="8504" right="8504" top="5668" bottom="4536" 
                       header="4536" footer="4536" gutter="0"/>
        <hs:landscape value="false"/>
        <hs:cols type="1" layout="LEFT"/>
    </hs:secPr>
    
    <!-- Section content -->
    <hp:p paraShapeIDRef="0" styleIDRef="0">
        <hp:run charShapeIDRef="0">
            <hp:t>Section content here</hp:t>
        </hp:run>
    </hp:p>
</hs:sec>
```

**XML Child Elements**:
| Element | XML Tag | Namespace | Description |
|---------|---------|-----------|-------------|
| Section Properties | `<hs:secPr>` | hs | Page layout and formatting |
| Page Size | `<hs:pageSz>` | hs | Page dimensions |
| Page Margins | `<hs:pageMargin>` | hs | Page margin settings |
| Landscape | `<hs:landscape>` | hs | Page orientation |
| Columns | `<hs:cols>` | hs | Column layout specification |
| Paragraphs | `<hp:p>` | hp | Content paragraphs |
| Objects | Various | hp | Tables, images, drawings, etc. |

**Properties**:
- Page dimensions in HWPML units (1/7200 inch)
- Margin settings for all sides plus header/footer
- Multi-column layout support
- Section break behavior control

## Paragraph Objects (Content Organization XML)

### PType (Paragraph)
**XML Element**: `<hp:p>`  
**XML Namespace**: `http://www.hancom.co.kr/hwpml/2011/paragraph`  
**Purpose**: Container for text and inline objects with paragraph-level formatting.

**XML Schema Definition**:
```xml
<hp:p paraShapeIDRef="0" styleIDRef="0" pageBreak="false" columnBreak="false">
    <hp:run charShapeIDRef="0">
        <hp:t>This is paragraph text content.</hp:t>
    </hp:run>
    <hp:run charShapeIDRef="1">
        <hp:t>Text with different formatting.</hp:t>
    </hp:run>
    <hp:lineseg/>
</hp:p>
```

**XML Attributes**:
| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `paraShapeIDRef` | integer | No | Reference to paragraph shape definition |
| `styleIDRef` | integer | No | Reference to named style |
| `pageBreak` | boolean | No | Force page break before paragraph |
| `columnBreak` | boolean | No | Force column break before paragraph |

**XML Child Elements**:
| Element | XML Tag | Occurrence | Description |
|---------|---------|------------|-------------|
| Text Run | `<hp:run>` | 0..* | Text segments with character formatting |
| Line Segment | `<hp:lineseg>` | 0..* | Line break indicators |
| Control | `<hp:ctrl>` | 0..* | Control characters and special elements |

### RunType
**XML Element**: `<hp:run>`  
**XML Namespace**: `http://www.hancom.co.kr/hwpml/2011/paragraph`  
**Parent Element**: `<hp:p>`  
**Purpose**: Sequence of text with consistent character formatting.

**XML Schema Definition**:
```xml
<hp:run charShapeIDRef="0">
    <hp:t>Regular text content</hp:t>
    <hp:char code="9" val="&#9;"/>  <!-- Tab character -->
    <hp:nbspace/>  <!-- Non-breaking space -->
    <hp:lineBreak/>  <!-- Manual line break -->
    <hp:bookmark name="bookmark1"/>
    <hp:markpenBegin/>
    <hp:t>Highlighted text</hp:t>
    <hp:markpenEnd/>
</hp:run>
```

**XML Attributes**:
| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `charShapeIDRef` | integer | No | Reference to character shape definition |

**XML Child Elements**:
| Element | XML Tag | Attributes | Description |
|---------|---------|------------|-------------|
| Text Content | `<hp:t>` | - | Plain Unicode text |
| Special Character | `<hp:char>` | code, val | Special/control characters |
| Tab | `<hp:tab>` | - | Tab character |
| Non-breaking Space | `<hp:nbspace>` | - | Non-breaking space |
| Hyphen | `<hp:hypen>` | - | Hyphenation point |
| Fixed-width Space | `<hp:fwspace>` | - | Fixed-width space |
| Line Break | `<hp:lineBreak>` | - | Manual line break |
| Index Mark | `<hp:indexmark>` | keyword1, keyword2 | Index entry marker |
| Bookmark | `<hp:bookmark>` | name | Bookmark definition |
| Highlight Begin | `<hp:markpenBegin>` | - | Start of highlighted text |
| Highlight End | `<hp:markpenEnd>` | - | End of highlighted text |

### T (Text)
**XML Element**: `<hp:t>`  
**XML Namespace**: `http://www.hancom.co.kr/hwpml/2011/paragraph`  
**Parent Element**: `<hp:run>`  
**Purpose**: Plain text content within a text run.

**XML Schema Definition**:
```xml
<hp:t>This is plain Unicode text content.</hp:t>
```

**Content**: Unicode text string  
**Attributes**: None - contains pure text content  
**Character Encoding**: UTF-8/UTF-16 Unicode

### Char (Special Character)
**XML Element**: `<hp:char>`  
**XML Namespace**: `http://www.hancom.co.kr/hwpml/2011/paragraph`  
**Parent Element**: `<hp:run>`  
**Purpose**: Special characters requiring specific handling or rendering.

**XML Schema Definition**:
```xml
<hp:char code="9" val="&#9;"/>      <!-- Tab -->
<hp:char code="13" val="&#13;"/>    <!-- Carriage return -->
<hp:char code="8203" val="&#8203;"/> <!-- Zero-width space -->
<hp:char code="8364" val="€"/>      <!-- Euro symbol -->
```

**XML Attributes**:
| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | integer | Yes | Unicode character code point |
| `val` | string | No | Character value or replacement text |

**Special Character Categories**:
- **Control Characters**: Tab (9), Line Feed (10), Carriage Return (13)
- **Mathematical Symbols**: Various Unicode math symbols
- **Diacritical Marks**: Combining diacritical marks
- **Typography**: Em dash, en dash, various quotes
- **Currency**: Euro (€), Yen (¥), Pound (£), etc.
- **Zero-width Characters**: Zero-width space, joiner, non-joiner

## Table Objects (Tabular Data)

### TableType
**XML Element**: `<hp:tbl>`  
**Purpose**: Structured tabular data with complex formatting capabilities.

**Attributes**:
- `inlineTable`: Whether table is inline or floating
- `pageBreak`: Page break behavior
- `repeatHeader`: Whether to repeat header rows

**Child Elements**:
- `cellzoneList`: Cell grouping and styling
- `tr`: Table rows
- `coldef`: Column definitions

**Table Features**:
- Merged cells and complex spanning
- Nested tables within cells
- Custom cell borders and backgrounds
- Automatic row height adjustment
- Table captioning and titling

### TrType (Table Row)
**XML Element**: `<hp:tr>`  
**Purpose**: Horizontal grouping of table cells.

**Child Elements**:
- `tc`: Table cells

### TcType (Table Cell)
**XML Element**: `<hp:tc>`  
**Purpose**: Individual table cell containing document content.

**Attributes**:
- `colspan`: Number of columns spanned
- `rowspan`: Number of rows spanned
- `width`: Cell width specification
- `height`: Cell height specification
- `header`: Whether cell is a header cell
- `hasMargin`: Whether cell has custom margins
- `protect`: Whether cell content is protected
- `editable`: Whether cell content is editable
- `dirty`: Whether cell content has been modified
- `borderFillIDRef`: Reference to border/fill definition

**Child Elements**:
- `cellMargin`: Cell margin specifications
- `p`: Cell content paragraphs
- Any valid paragraph content including nested tables

## Media and Object Elements

### PictureType
**XML Element**: `<hp:pic>`  
**Purpose**: Embedded or linked image content with positioning and formatting.

**Attributes**:
- `reverse`: Whether image is reversed/mirrored
- `watermark`: Whether image is used as watermark
- `effect`: Image effects (brightness, contrast, etc.)

**Child Elements**:
- `img`: Image specifications and source
- `effects`: Visual effects and filters

**Image Properties**:
- Source file references
- Dimensions and scaling
- Cropping and clipping
- Color adjustments
- Transparency and blending

### ImgType
**XML Element**: `<hp:img>`  
**Purpose**: Image source and display specifications.

**Attributes**:
- `src`: Image source file reference
- `binaryItem`: Reference to binary data
- `embedded`: Whether image is embedded or linked

**Child Elements**:
- `clip`: Image clipping boundaries
- `effects`: Image processing effects

### EquationType
**XML Element**: `<hp:equation>`  
**Purpose**: Mathematical expressions and formulas.

**Attributes**:
- `name`: Equation name or identifier
- `font`: Font used for equation rendering
- `fontSize`: Base font size for equation
- `textColor`: Text color for equation
- `baseLine`: Baseline alignment

**Child Elements**:
- `script`: Mathematical expression script/markup

**Mathematical Features**:
- Complex fraction notation
- Radical expressions
- Integral and summation notation
- Matrix and array layouts
- Subscripts and superscripts
- Mathematical symbols and operators

### OLEType
**XML Element**: `<hp:ole>`  
**Purpose**: Embedded objects from external applications.

**Attributes**:
- `progId`: Program identifier for the embedded object
- `drawAspect`: Display aspect (content, icon, thumbnail)

**Child Elements**:
- `img`: Preview image for the object
- Binary object data

**OLE Object Types**:
- Microsoft Office documents
- Charts and graphs
- Media files (audio, video)
- Custom application objects

## Form Controls

### EditType
**XML Element**: `<hp:edit>`  
**Purpose**: Text input controls for data entry.

**Attributes**:
- `multiLine`: Whether control accepts multiple lines
- `password`: Whether to mask input (password field)
- `maxLength`: Maximum input length
- `scrollBars`: Scroll bar display options
- `tabStop`: Whether control participates in tab order
- `fieldName`: Field name for form processing
- `value`: Default or current value

**Validation**: Supports input validation rules and formatting constraints.

### ComboBoxType
**XML Element**: `<hp:combobox>`  
**Purpose**: Dropdown selection controls.

**Attributes**:
- `dropDown`: Whether to show dropdown list
- `editable`: Whether custom values are allowed
- `listItemCount`: Number of visible list items
- `selectedIndex`: Currently selected item index

**Child Elements**:
- `listItem`: Individual dropdown options

### ListBoxType
**XML Element**: `<hp:listbox>`  
**Purpose**: Multi-selection list controls.

**Attributes**:
- `multiSelect`: Whether multiple selection is allowed
- `size`: Number of visible items
- `listItemCount`: Total number of items

**Child Elements**:
- `listItem`: Individual list options

### AbstractButtonObjectType
**Purpose**: Base class for button controls including radio buttons, checkboxes, and command buttons.

**Common Attributes**:
- `caption`: Button text or label
- `value`: Button value or state
- `checked`: Whether button is selected/checked
- `groupName`: Radio button group identifier

## Drawing Objects

### LineShapeType
**XML Element**: `<hp:line>`  
**Purpose**: Simple line drawing objects.

**Attributes**:
- `startPos`: Line starting position
- `endPos`: Line ending position
- `lineType`: Line style and weight

### RectangleType
**XML Element**: `<hp:rect>`  
**Purpose**: Rectangle and square drawing objects.

**Attributes**:
- `round`: Corner rounding radius

### EllipseType
**XML Element**: `<hp:ellipse>`  
**Purpose**: Circle and ellipse drawing objects.

**Properties**: Standard shape positioning and styling attributes.

### ArcType
**XML Element**: `<hp:arc>`  
**Purpose**: Arc and curved line segments.

**Attributes**:
- `startAngle`: Arc starting angle
- `endAngle`: Arc ending angle
- `clockwise`: Arc direction

### PolygonType
**XML Element**: `<hp:polygon>`  
**Purpose**: Multi-sided polygon shapes.

**Child Elements**:
- `pt`: Polygon vertex points

### CurveType
**XML Element**: `<hp:curve>`  
**Purpose**: Bezier curves and complex paths.

**Child Elements**:
- `pt`: Curve control points

### ConnectLineType
**XML Element**: `<hp:connectLine>`  
**Purpose**: Connector lines between shapes.

**Attributes**:
- `startShape`: Starting shape reference
- `endShape`: Ending shape reference
- `startPos`: Connection point on start shape
- `endPos`: Connection point on end shape

## Field Objects (Dynamic Content)

### FieldBegin/FieldEnd
**XML Elements**: `<hp:fieldBegin>`, `<hp:fieldEnd>`  
**Purpose**: Delimit field content for dynamic data.

**Field Types**:
- Page numbering
- Date and time
- Document properties
- Cross-references
- Mail merge fields

### PageNumCtrlType
**XML Element**: `<hp:pageNumCtrl>`  
**Purpose**: Page number display and formatting.

**Attributes**:
- `pageNumFormat`: Number format (decimal, roman, etc.)
- `sideName`: Side identifier for different page types

### BookmarkType
**XML Element**: `<hp:bookmark>`  
**Purpose**: Named locations for cross-referencing and navigation.

**Attributes**:
- `name`: Bookmark name/identifier

## Layout and Positioning Objects

### PositionType
**Purpose**: Defines object positioning within document flow.

**Positioning Types**:
- Inline with text
- Floating with text wrapping
- Absolute positioning
- Relative to page, paragraph, or other objects

### InsideMarginType
**XML Element**: `<hp:insideMargin>`  
**Purpose**: Margin specifications for floating objects.

### CellMarginType
**XML Element**: `<hp:cellMargin>`  
**Purpose**: Margin specifications for table cells.

### TextMarginType
**XML Element**: `<hp:textMargin>`  
**Purpose**: Text margins within objects and containers.

## Effects and Visual Enhancement

### EffectsType
**XML Element**: `<hp:effects>`  
**Purpose**: Visual effects for images and objects.

**Effect Types**:
- `shadow`: Drop shadow effects
- `glow`: Glow and halo effects
- `reflection`: Mirror reflection effects
- `softEdge`: Soft edge blurring

### ShadowType
**XML Element**: `<hp:shadow>`  
**Purpose**: Shadow effect specifications.

**Attributes**:
- `offsetX`, `offsetY`: Shadow offset distances
- `blurRadius`: Shadow blur amount
- `color`: Shadow color
- `transparency`: Shadow opacity

### GlowType
**XML Element**: `<hp:glow>`  
**Purpose**: Glow effect around objects.

### ReflectionType
**XML Element**: `<hp:reflection>`  
**Purpose**: Mirror reflection effects below objects.

## Core Data Types

### PointType
**Purpose**: 2D coordinate specifications.

**Attributes**:
- `x`: Horizontal coordinate
- `y`: Vertical coordinate

### RGBType
**XML Element**: `<hp:rgb>`  
**Purpose**: RGB color specifications.

**Attributes**:
- `r`: Red component (0-255)
- `g`: Green component (0-255)
- `b`: Blue component (0-255)

### CMYKType
**XML Element**: `<hp:cmyk>`  
**Purpose**: CMYK color specifications for print.

**Attributes**:
- `c`: Cyan percentage
- `m`: Magenta percentage
- `y`: Yellow percentage
- `k`: Black percentage

### MatrixType
**Purpose**: 2D transformation matrices for object positioning and scaling.

### ImageType
**Purpose**: Image data and format specifications.

## Document Package Objects

### PackageType
**XML Element**: `<package>`  
**Purpose**: OPF package definition for document structure.

**Child Elements**:
- `metadata`: Document metadata
- `manifest`: File listing
- `spine`: Reading order

### ManifestType
**XML Element**: `<manifest>`  
**Purpose**: Complete file listing for document package.

**Child Elements**:
- `item`: Individual file entries

### ItemType
**XML Element**: `<item>`  
**Purpose**: Individual file entry in manifest.

**Attributes**:
- `id`: Unique item identifier
- `href`: File path within package
- `media-type`: MIME type of file

### MetadataType
**XML Element**: `<metadata>`  
**Purpose**: Document metadata in Dublin Core format.

**Child Elements**:
- `title`: Document title
- `creator`: Document author
- `subject`: Document subject/topic
- `description`: Document description
- `publisher`: Publishing information
- `date`: Creation/modification dates
- `language`: Document language

This comprehensive reference covers every major object type in the HWPX document model, providing detailed specifications for structure, attributes, and relationships between objects.