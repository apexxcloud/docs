# Document Transformation Documentation

## Basic Format
Transformations follow this pattern: `tr-{parameter_name}_{value}`
Multiple parameters can be combined with commas: `tr-p_1-3,f_pdf`

## Available Transformations

### 1. Format Conversion
- **Format**: `f_{format}`
  - Converts document to specified format
  - Supported formats: `pdf`, `docx`, `xlsx`, `pptx`
  - Example: `tr-f_pdf`

### 2. Thumbnail Generation
- **Format**: `thumb-{parameters}`
  - Generates document thumbnail with optional parameters
  - Can be combined with image transformations
  - Example: `thumb-w_300,h_200,f_webp`

### 3. Quality & Compression
- **Quality**: `q_{value}`
  - Sets output quality/compression level
  - Auto modes:
    - `auto:high`: Minimal compression
    - `auto:standard`: Balanced compression
    - `auto:low`: Maximum compression
  - Example: `tr-q_auto:standard`

- **Compression**: `c_{mode}`
  - Sets specific compression mode
  - Available modes:
    - `high`: Maximum compression
    - `medium`: Balanced compression
    - `low`: Minimal compression
  - Example: `tr-c_high`

  Note: Don't use quality and compression together.


## Example Combinations

1. Convert to PDF and extract first 3 pages:
```
tr-f_pdf,p_1-3
```

2. Rotate and add watermark:
```
tr-r_90,w_Confidential
```

3. High compression PDF:
```
tr-f_pdf,c_high
```

4. Convert presentation with specific quality:
```
tr-f_pdf,q_auto:high
```

## Format-Specific Features

### PDF
- Supports all transformations
- Thumbnail support
- Best format for compression options


### DOCX
- Format conversion to PDF
- Thumbnail support
- Basic compression

### XLSX
- Format conversion to PDF
- Thumbnail support
- Sheet selection (future feature)

### PPT/PPTX
- Format conversion to PDF
- Thumbnail support
- Basic slide selection


## Best Practices

1. **Format Selection**:
   - Use PDF for best compatibility
   - Consider end-user requirements
   - Balance quality vs file size

2. **Performance**:
   - Use appropriate compression levels
   - Limit page ranges when possible
   - Consider document size impact

3. **Watermarks**:
   - Keep text concise
   - Use URL encoding for special characters
   - Test visibility across formats

4. **Compression**:
   - Use `auto:standard` for general use
   - `high` compression for archival
   - `low` compression for quality-critical documents

