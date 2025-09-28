# HWPX Object Type Reference

This document provides detailed specifications for every object type in the HWPX document model, organized by functional category.

## Head Objects (Document-Level Definitions)

### HWPMLHeadType
**XML Element**: `<hh:head>`  
**Purpose**: Root container for all document-level definitions including styles, fonts, and global settings.

**Attributes**:
- `version`: OWPML format version (e.g., "1.31")
- `secCnt`: Number of sections in the document

**Child Elements**:
- `beginNum`: Starting numbers for various counters
- `refList`: Reference mappings and lookup tables
- `forbiddenWordList`: Words that cannot be hyphenated
- `compatibleDocument`: Compatibility settings for different applications
- `docOption`: Document behavior options
- `trackChangeConfig`: Track changes configuration
- `fontfaces`: Font definitions
- `borderfills`: Border and fill style definitions
- `charshapes`: Character formatting definitions
- `parashapes`: Paragraph formatting definitions
- `styles`: Named style definitions
- `numberings`: Numbering scheme definitions
- `bullets`: Bullet style definitions
- `memoProperties`: Comment and annotation properties

### FontfaceType
**XML Element**: `<hh:fontface>`  
**Purpose**: Defines a font family with language-specific variants and fallback options.

**Attributes**:
- `name`: Font family name
- `type`: Font type ("ttf", "otf", "woff", etc.)

**Child Elements**:
- `font`: Individual font definitions for different languages
- `substFont`: Substitute font mappings
- `typeInfo`: Font metrics and characteristics

**Language Support**: Supports separate font definitions for:
- Hangul (Korean)
- Latin (Western characters)
- Hanja (Chinese characters)
- Japanese characters
- Symbols and special characters

### CharShapeType
**XML Element**: `<hh:charshape>`  
**Purpose**: Defines character-level formatting that can be applied to text runs.

**Attributes**:
- `id`: Unique identifier for referencing
- `height`: Base font size in points
- `textColor`: Text color specification
- `shadeColor`: Background color for text
- `useFontSpace`: Whether to use font-specific spacing
- `useKerning`: Whether to apply kerning
- `symMark`: Symbol mark type for diacritics

**Child Elements**:
- `fontRef`: References to font faces for different languages
- `ratio`: Scaling ratios for width/height
- `spacing`: Character spacing adjustments
- `relSz`: Relative size adjustments
- `offset`: Baseline offset adjustments
- `italic`: Italic formatting settings
- `bold`: Bold formatting settings
- `underline`: Underline style definitions
- `strikeout`: Strikethrough formatting
- `outline`: Text outline effects
- `shadow`: Text shadow effects
- `emboss`: Embossed text effects
- `engrave`: Engraved text effects
- `supscript`: Superscript positioning
- `subscript`: Subscript positioning

### ParaShapeType
**XML Element**: `<hh:parashape>`  
**Purpose**: Defines paragraph-level formatting including alignment, spacing, and indentation.

**Attributes**:
- `id`: Unique identifier
- `align`: Text alignment (left, center, right, justify, distribute)
- `heading`: Outline heading level
- `breakSetting`: Page and column break behavior
- `condense`: Text condensation percentage
- `fontLineHeight`: Line height calculation method
- `snapToGrid`: Whether to snap to document grid
- `suppressLineNumbers`: Whether to suppress line numbering
- `checked`: Checkbox state for list items

**Child Elements**:
- `margin`: Paragraph margins (left, right, first line, hanging)
- `lineSpacing`: Line spacing definitions
- `border`: Paragraph border settings
- `autoSpacing`: Automatic spacing between paragraphs

### BorderFillType
**XML Element**: `<hh:borderfill>`  
**Purpose**: Defines border and background fill styles for various elements.

**Attributes**:
- `id`: Unique identifier
- `threeD`: 3D border effect settings
- `shadow`: Shadow effect settings

**Child Elements**:
- `slash`: Diagonal line patterns
- `borders`: Individual border definitions (left, right, top, bottom)

**Border Properties**:
- Line style (solid, dotted, dashed, double, etc.)
- Line width and color
- Corner styles and rounding

**Fill Properties**:
- Solid colors with transparency
- Gradient fills (linear, radial, patterns)
- Image backgrounds and textures
- Pattern fills and hatching

### StyleType
**XML Element**: `<hh:style>`  
**Purpose**: Named style definitions that combine character and paragraph formatting.

**Attributes**:
- `id`: Unique identifier
- `type`: Style type ("PARA" for paragraph, "CHAR" for character)
- `name`: User-visible style name
- `engName`: English name for the style
- `paraPr`: Reference to paragraph shape
- `charPr`: Reference to character shape
- `nextStyle`: Style to apply to following paragraphs
- `langId`: Language identifier
- `lockForm`: Whether style formatting is locked

### NumberingType
**XML Element**: `<hh:numbering>`  
**Purpose**: Defines numbering schemes for ordered lists and outline numbering.

**Attributes**:
- `id`: Unique identifier

**Child Elements**:
- `paraHead`: Paragraph heading definitions for each level
- `start`: Starting numbers for each level
- `format`: Number formatting (decimal, roman, letters, etc.)

**Numbering Formats**:
- Decimal (1, 2, 3...)
- Upper/lower Roman (I, II, III... or i, ii, iii...)
- Upper/lower letters (A, B, C... or a, b, c...)
- Korean numbering systems
- Custom format strings

### BulletType
**XML Element**: `<hh:bullet>`  
**Purpose**: Defines bullet styles for unordered lists.

**Attributes**:
- `id`: Unique identifier
- `char`: Bullet character
- `checkedChar`: Character for checked items
- `useImage`: Whether to use image bullets

**Image Bullets**: Can reference embedded images for custom bullet appearance.

## Section Objects (Document Structure)

### SectionType
**XML Element**: `<hs:sec>`  
**Purpose**: Major document divisions with independent page layout and content flow.

**Child Elements**:
- Content paragraphs and objects
- Page layout definitions
- Headers and footers
- Column specifications

**Properties**:
- Page dimensions and orientation
- Margin settings
- Multi-column layout
- Section break behavior

## Paragraph Objects (Content Organization)

### PType (Paragraph)
**XML Element**: `<hp:p>`  
**Purpose**: Container for text and inline objects with paragraph-level formatting.

**Attributes**:
- `paraShapeIDRef`: Reference to paragraph shape definition
- `styleIDRef`: Reference to named style
- `pageBreak`: Page break behavior
- `columnBreak`: Column break behavior

**Child Elements**:
- `run`: Text runs with character formatting
- `lineseg`: Line break indicators
- `ctrl`: Control characters and special elements

### RunType
**XML Element**: `<hp:run>`  
**Purpose**: Sequence of text with consistent character formatting.

**Attributes**:
- `charShapeIDRef`: Reference to character shape definition

**Child Elements**:
- `t`: Plain text content
- `char`: Special characters
- `tab`: Tab characters
- `nbspace`: Non-breaking spaces
- `hypen`: Hyphenation points
- `fwspace`: Fixed-width spaces
- `lineBreak`: Manual line breaks
- `indexmark`: Index entry markers
- `bookmark`: Bookmark definitions
- `markpenBegin`/`markpenEnd`: Highlight markers

### T (Text)
**XML Element**: `<hp:t>`  
**Purpose**: Plain text content within a text run.

**Content**: Unicode text string
**Attributes**: None - pure text content

### Char (Special Character)
**XML Element**: `<hp:char>`  
**Purpose**: Special characters requiring specific handling or rendering.

**Attributes**:
- `code`: Character code or identifier
- `val`: Character value or replacement text

**Special Character Types**:
- Mathematical symbols
- Diacritical marks
- Control characters
- Custom symbols
- Placeholder characters

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