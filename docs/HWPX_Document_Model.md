# HWPX Document Object Model

## Document Structure Overview

The HWPX document model is organized hierarchically with distinct object categories that represent different aspects of a document. Every element in an HWPX document is represented by a specific object type with defined properties and relationships.

## Object Categories

### Document Container Objects

#### Document Root (`CDocument`)
The root container that represents the entire HWPX document package. This object manages the ZIP container and provides access to all document components.

**Purpose**: Serves as the entry point for accessing all document content and metadata.

**Key Components**:
- Package manifest and metadata
- Version information
- Security and encryption settings
- File relationships and dependencies

#### Section (`CSectionType`) 
Represents a major division of the document with its own page layout and content flow. Each section can have different page settings, headers, footers, and content.

**Purpose**: Provides document structure and allows different formatting for different parts of the document.

**Properties**:
- Page layout settings (margins, orientation, size)
- Header and footer definitions
- Column layout configuration
- Section breaks and continuation settings

**Content**: Contains paragraphs, tables, images, and other content objects.

### Content Objects

#### Paragraph (`CPType`)
The fundamental content container that holds text runs, inline objects, and formatting information.

**Purpose**: Organizes text and inline content with consistent paragraph-level formatting.

**Structure**:
- Contains one or more text runs (`CRunType`)
- May include inline objects (images, OLE objects, form controls)
- Maintains paragraph-level formatting properties
- Handles line breaking and spacing

**Properties**:
- Paragraph style reference
- Alignment and indentation
- Line spacing and paragraph spacing
- Border and shading
- Tab stops and numbering

#### Text Run (`CRunType`)
A sequence of text with consistent character-level formatting. Runs break when formatting changes or special elements are inserted.

**Purpose**: Maintains character formatting consistency within paragraph content.

**Content Types**:
- Plain text (`CT`)
- Special characters (`CChar`)
- Tabs (`CTab`)
- Line breaks (`CLineBreak`)
- Fields and variables
- Inline objects and controls

**Properties**:
- Character shape reference (font, size, style)
- Text effects (bold, italic, underline, etc.)
- Color and highlighting
- Character spacing and scaling

#### Text Content (`CT`)
The actual text content within a text run. This is the leaf node that contains the readable text.

**Purpose**: Stores the actual character data that appears in the document.

**Properties**:
- Text string content
- Language and locale information
- Text direction (left-to-right, right-to-left)
- Character encoding information

#### Special Character (`CChar`)
Represents special characters, symbols, and non-printable characters that require special handling.

**Purpose**: Handles characters that cannot be represented as plain text or require special rendering.

**Types**:
- Unicode symbols and special characters
- Mathematical symbols
- Diacritical marks and accents
- Control characters and formatting marks
- Custom symbols and user-defined characters

### Table Objects

#### Table (`CTableType`)
A structured data container organized in rows and columns with complex formatting capabilities.

**Purpose**: Presents tabular data with precise control over layout, formatting, and content flow.

**Structure**:
- Contains table rows (`CTableRowType`)
- Defines column specifications (`CColumnDefType`)
- Manages table-wide properties and styling

**Properties**:
- Table dimensions and sizing
- Border and shading properties
- Cell spacing and padding
- Table alignment and positioning
- Table caption and title information

#### Table Row (`CTableRowType`)
A horizontal group of table cells that maintains row-level properties.

**Purpose**: Groups cells horizontally and applies row-level formatting.

**Properties**:
- Row height specifications
- Row-level borders and shading
- Row repetition settings (for headers)
- Row breaking behavior across pages

#### Table Cell (`CTableCellType`)
Individual cell within a table that can contain any document content including nested tables.

**Purpose**: Contains and formats content within table structure.

**Content**: Can contain paragraphs, lists, images, nested tables, and other document objects.

**Properties**:
- Cell dimensions and spanning
- Cell borders and background
- Content alignment and margins
- Cell data type and validation
- Merge and split information

### Media and Object Elements

#### Picture (`CPictureType`)
Embedded or linked image content with positioning and formatting options.

**Purpose**: Displays raster and vector images within document content.

**Image Types**:
- Raster formats (PNG, JPEG, BMP, TIFF)
- Vector formats (SVG, WMF, EMF)
- Compressed formats specific to HWPX

**Properties**:
- Image dimensions and scaling
- Positioning and text wrapping
- Border and effects
- Alternative text and accessibility
- Compression and quality settings

#### OLE Object (`COLEType`)
Embedded or linked objects from other applications (spreadsheets, presentations, etc.).

**Purpose**: Integrates content from external applications while maintaining editability.

**Object Types**:
- Microsoft Office documents
- Charts and graphs
- Mathematical equations
- Custom application objects

**Properties**:
- Object type and application information
- Display options and sizing
- Update and linking behavior
- Security and sandboxing settings

#### Chart (`CChartType`)
Data visualization objects including graphs, charts, and diagrams.

**Purpose**: Provides visual representation of data with customizable appearance.

**Chart Types**:
- Column and bar charts
- Line and area charts
- Pie and donut charts
- Scatter and bubble charts
- Complex combination charts

**Properties**:
- Data source and ranges
- Chart styling and colors
- Axis configuration and labels
- Legend and title settings
- Animation and interaction options

#### Equation (`CEquationType`)
Mathematical expressions and formulas with sophisticated formatting.

**Purpose**: Displays complex mathematical notation with precise typography.

**Features**:
- Mathematical symbols and operators
- Fractions, radicals, and integrals
- Superscripts and subscripts
- Matrix and array notation
- Custom mathematical functions

**Properties**:
- Equation formatting and layout
- Font and symbol selection
- Alignment and positioning
- Numbering and referencing

### Form and Interactive Objects

#### Form Controls
Interactive elements that allow user input and data collection.

##### Edit Control (`CEditType`)
Text input fields for user data entry.

**Purpose**: Collects text input from users with validation and formatting options.

**Types**:
- Single-line text input
- Multi-line text areas
- Formatted input fields (dates, numbers)
- Password and secure input fields

**Properties**:
- Input validation rules
- Default values and placeholders
- Character limits and formatting
- Tab order and accessibility

##### Combo Box (`CComboBoxType`)
Dropdown selection controls with predefined options.

**Purpose**: Provides selection from predefined lists with optional custom input.

**Features**:
- Dropdown list of options
- Editable and read-only modes
- Multi-selection capabilities
- Dynamic option loading

**Properties**:
- Option list and values
- Selection behavior
- Sorting and filtering
- Custom value handling

##### List Box (`CListBoxType`)
Multi-option selection controls for choosing from lists.

**Purpose**: Allows selection of one or multiple items from a visible list.

**Properties**:
- Item list and organization
- Selection modes (single, multiple)
- Scrolling and navigation
- Item grouping and categorization

##### Button Controls (`CAbstractButtonObjectType`)
Interactive buttons for actions and navigation.

**Types**:
- Command buttons
- Radio buttons
- Checkboxes
- Image buttons

**Properties**:
- Button text and images
- Action definitions
- State and enabled status
- Grouping and mutual exclusion

### Page Layout Objects

#### Header and Footer (`CHeaderFooterType`)
Content that appears at the top and bottom of pages with positioning options.

**Purpose**: Provides consistent information across pages while allowing content variation.

**Content Types**:
- Static text and formatting
- Dynamic fields (page numbers, dates)
- Images and logos
- Tables and structured content

**Properties**:
- Positioning and margins
- Different first page settings
- Odd and even page variations
- Section-specific content

#### Master Page (`CMasterPageType`)
Template definitions that control page layout and formatting.

**Purpose**: Defines consistent page structure and formatting across document sections.

**Components**:
- Page dimensions and orientation
- Margin and gutter settings
- Header and footer regions
- Background and watermarks
- Column and grid layouts

#### Page Properties (`CPagePrType`)
Detailed page layout specifications including dimensions, margins, and orientation.

**Purpose**: Controls the physical layout and appearance of printed and displayed pages.

**Settings**:
- Page size (standard and custom)
- Orientation (portrait, landscape)
- Margins and gutters
- Border and background
- Grid and alignment guides

### Style and Formatting Objects

#### Character Shape (`CCharShapeType`)
Defines character-level formatting properties that can be referenced and reused.

**Purpose**: Provides consistent character formatting through reusable style definitions.

**Properties**:
- Font family, size, and weight
- Text effects (bold, italic, underline, strikethrough)
- Character spacing and scaling
- Color and highlighting
- Superscript and subscript positioning
- Language and locale settings

#### Paragraph Shape (`CParaShapeType`)
Defines paragraph-level formatting properties including alignment, spacing, and indentation.

**Purpose**: Ensures consistent paragraph formatting through reusable style definitions.

**Properties**:
- Text alignment (left, center, right, justify)
- Indentation (first line, left, right)
- Line spacing (single, double, custom)
- Paragraph spacing (before, after)
- Tab stops and leaders
- Border and shading
- Numbering and bullets

#### Border and Fill (`CBorderFillType`)
Defines visual styling for borders, backgrounds, and shading effects.

**Purpose**: Provides reusable visual styling for various document elements.

**Border Properties**:
- Line styles and weights
- Colors and patterns
- Corner rounding and effects
- Shadow and 3D effects

**Fill Properties**:
- Solid colors and transparency
- Gradient fills and patterns
- Image backgrounds and textures
- Transparency and blending modes

#### Font Face (`CFontfaceType`)
Font definitions including typeface information, embedding, and fallback options.

**Purpose**: Manages font resources and ensures consistent typography across documents.

**Properties**:
- Font family name and alternatives
- Font file references and embedding
- Character set and encoding support
- Fallback font specifications
- Font metrics and spacing

### Document Metadata Objects

#### Version Information (`CVersionType`)
Document format version and compatibility information.

**Purpose**: Ensures proper handling of document features across different application versions.

**Information**:
- OWPML format version
- Application version compatibility
- Feature support levels
- Migration and upgrade paths

#### Application Settings (`CHWPApplicationSettingType`)
Application-specific settings and preferences that affect document behavior.

**Purpose**: Maintains application-specific behavior and user preferences.

**Settings**:
- User interface preferences
- Printing and export options
- Security and permission settings
- Language and localization options

#### Package Information (`CPackageType`)
OPF (Open Packaging Format) metadata that describes document structure and relationships.

**Purpose**: Provides navigation and structural information for document processing.

**Components**:
- File manifest and media types
- Reading order and navigation
- Metadata and annotations
- Digital rights and permissions

#### Manifest (`CManifestType`)
Complete listing of all files and resources within the HWPX package.

**Purpose**: Catalogs all document components and their relationships.

**Information**:
- File paths and identifiers
- Media types and formats
- Compression and encoding
- Dependencies and references

### Document History and Tracking

#### History Entry (`CHWPMLHistoryType`)
Revision tracking and change history information.

**Purpose**: Maintains detailed record of document changes for collaboration and auditing.

**Information**:
- Author and timestamp data
- Change descriptions and categories
- Version comparison data
- Approval and review status

#### Track Changes (`CTrackChangeType`)
Detailed change tracking for collaborative editing with revision markup.

**Purpose**: Enables collaborative editing with visible change tracking and review processes.

**Change Types**:
- Text insertions and deletions
- Formatting modifications
- Structural changes (tables, objects)
- Comment and review annotations

### Specialty Objects

#### Field Objects
Dynamic content that updates based on document context or external data.

**Types**:
- Page numbering and references
- Date and time stamps
- Document properties and metadata
- Cross-references and bookmarks
- Mail merge and data fields

**Properties**:
- Field type and calculation
- Formatting and display options
- Update behavior and triggers
- Data sources and connections

#### Annotation Objects
Comments, notes, and markup that provide additional information without affecting main content.

**Types**:
- Comments and reviews
- Footnotes and endnotes
- Bookmarks and anchors
- Hyperlinks and navigation
- Hidden comments and markup

**Properties**:
- Author and timestamp information
- Positioning and display options
- Reply threading and conversations
- Visibility and access controls

#### Drawing Objects
Vector graphics and shape objects with advanced styling capabilities.

**Shape Types**:
- Basic shapes (rectangles, circles, lines)
- Complex paths and curves
- Text boxes and callouts
- Connectors and flow charts
- Custom and composite shapes

**Properties**:
- Geometry and dimensions
- Fill and stroke styling
- Transformation and positioning
- Grouping and layering
- Animation and effects

## Object Relationships and Hierarchy

### Parent-Child Relationships
The HWPX document model follows a strict hierarchical structure where each object type has defined parent and child relationships:

- **Document** → **Sections** → **Paragraphs** → **Runs** → **Text/Objects**
- **Tables** → **Rows** → **Cells** → **Content Objects**
- **Headers/Footers** → **Content Objects**
- **Forms** → **Form Controls**

### Reference Relationships
Many objects reference shared resources through ID-based relationships:

- **Content Objects** → **Character Shapes** (formatting)
- **Paragraphs** → **Paragraph Shapes** (layout)
- **Visual Elements** → **Border/Fill Definitions** (styling)
- **Text** → **Font Faces** (typography)

### Dependency Relationships
Some objects depend on others for complete functionality:

- **Fields** depend on **Document Properties**
- **Cross-references** depend on **Bookmarks**
- **Charts** depend on **Data Sources**
- **Styles** depend on **Base Definitions**

This comprehensive object model ensures that every aspect of document content, formatting, and behavior is precisely defined and consistently handled across different implementations and versions of HWPX-compatible applications.