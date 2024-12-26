I'll help create comprehensive documentation for the image transformation system. Let's break down all available transformations and their usage.

# Image Transformation Documentation

## Basic Format
Transformations follow this pattern: `tr-{parameter_name}_{value}`
Multiple parameters can be combined with commas: `tr-w_200,h_100`
Multiple transformation sets can be chained with plus signs: `tr-w_200,h_100+g_face,c_crop`

## Available Transformations

### 1. Dimensions and Scaling
- **Width**: `w_{pixels}`
  - Sets image width in pixels
  - Example: `tr-w_300`

- **Height**: `h_{pixels}`
  - Sets image height in pixels
  - Example: `tr-h_200`

- **Aspect Ratio**: `ar_{ratio}`
  - Sets desired aspect ratio (width/height)
  - Example: `tr-ar_1.5`

- **Device Pixel Ratio**: `dpr_{value}`
  - Multiplies dimensions by specified factor (1.0-5.0)
  - Example: `tr-dpr_2`

- **Zoom**: `z_{factor}`
  - Applies zoom factor to dimensions
  - Example: `tr-z_1.5`

### 2. Cropping and Resizing
- **Crop Mode**: `c_{mode}`
  - Available modes:
    - `crop`: Crops to exact dimensions
    - `fill`: Resizes to fill dimensions and crops excess
    - `fit`: Resizes to fit within dimensions
    - `scale`: Simple resize to dimensions
    - `limit`: Only scales down, never up
    - `pad`: Adds padding to reach dimensions
  - Example: `tr-c_crop`

- **Gravity**: `g_{position}`
  - Controls focus point for cropping
  - Available positions:
    - `center` (default)
    - `north`, `south`, `east`, `west`
    - `north_east`, `north_west`, `south_east`, `south_west`
    - `face`: Centers on detected face
    - `faces`: Centers on multiple detected faces
    - `object:{name}`: Centers on detected object
  - Example: `tr-g_face` or `tr-g_object:car`

### 3. Visual Effects
- **Background**: `b_{color}`
  - Sets background color
  - Accepts hex colors or `transparent`
  - Example: `tr-b_ff0000` or `tr-b_transparent`

- **Border**: `bo_{width_style_color}`
  - Adds border to image
  - Example: `tr-bo_5px_solid_000000`

- **Radius**: `r_{pixels}`
  - Rounds corners
  - Single value or up to 4 values separated by colons
  - Special value `max` for circular/elliptical shape
  - Example: `tr-r_20` or `tr-r_20:30:20:30` or `tr-r_max`

- **Angle**: `a_{degrees}`
  - Rotates image
  - Example: `tr-a_90`

- **Opacity**: `o_{percent}`
  - Sets image opacity (0-100)
  - Example: `tr-o_50`

- **Blur**: `blur_{radius}`
  - Applies Gaussian blur
  - Radius: 1-100
  - Example: `tr-blur_10`

- **Effects**: `e_{effect}`
  - Available effects:
    - `grayscale`: Converts to grayscale
  - Example: `tr-e_grayscale`

### 4. Output Quality
- **Format**: `f_{format}`
  - Sets output format
  - Supported: `jpeg`, `png`, `webp`
  - Example: `tr-f_webp`

- **Quality**: `q_{value}`
  - Sets compression quality
  - Numeric value (1-100) or auto modes:
    - `auto:best` (92)
    - `auto:good` (85)
    - `auto:eco` (75)
    - `auto:low` (60)
  - Example: `tr-q_85` or `tr-q_auto:good`

## Example Combinations

1. Resize and crop to face:
```
tr-w_500,h_500,c_crop,g_face
```

2. Create circular thumbnail with border:
```
tr-w_200,h_200,r_max,bo_2px_solid_000000
```

3. Optimize for web with automatic quality:
```
tr-w_800,f_webp,q_auto:good
```

4. Complex transformation chain:
```
tr-w_500,h_300,c_fill,g_face+e_grayscale,r_20+bo_5px_solid_000000
```

## Best Practices

1. **Order Matters**: Transformations are applied in the order specified. For best results:
   - Apply size/crop transformations first
   - Apply visual effects (grayscale, blur) next
   - Apply borders and rounding last

2. **Performance**: 
   - Use appropriate image dimensions for your use case
   - Leverage automatic quality settings for optimization
   - Use WebP format when possible for better compression

3. **Caching**:
   - Transformed images are cached for improved performance
   - Cache TTL is 1 hour by default

Would you like me to provide more specific examples or elaborate on any particular transformation?


Ah yes, let me add the overlay/layer documentation!

## 5. Overlays and Layers
Overlays are added using the `l_{type}` parameter. There are two types of overlays: text and image.

### Text Overlays
Format: `l_text:{text}:parameter1:parameter2...`

Parameters:
- `l_font_{name}`: Font family name
- `l_size_{pixels}`: Font size
- `l_color_{hex}`: Text color
- `l_gravity_{position}`: Text position
- `l_x_{pixels}`: X offset from gravity position
- `l_y_{pixels}`: Y offset from gravity position
- `l_o_{opacity}`: Text opacity (0-100)

Example:
```
tr-l_text:Hello World:l_font_Arial:l_size_24:l_color_000000:l_gravity_center:l_x_0:l_y_0:l_o_100
```

### Image Overlays
Format: `l_image:{image_key}:parameter1:parameter2...`

> **Important**: For image keys containing forward slashes (/), replace them with `@@`
> Example: `assets/images/logo.png` becomes `assets@@images@@logo.png`

Parameters:
- `l_gravity_{position}`: Overlay position
- `l_x_{pixels}`: X offset from gravity position
- `l_y_{pixels}`: Y offset from gravity position
- `l_o_{opacity}`: Image opacity (0-100)
- `l_scale_{factor}`: Scale factor for overlay image
- `l_width_{pixels}`: Specific width for overlay
- `l_height_{pixels}`: Specific height for overlay
- `l_ar_{ratio}`: Aspect ratio for overlay

Example:
```
tr-l_image:brand@@logos@@watermark.png:l_gravity_southeast:l_x_10:l_y_10:l_o_50:l_scale_0.5
```

## Complex Examples with Overlays

1. Image with watermark:
```
tr-w_800,h_600,c_fill+l_image:logo.png:l_gravity_southeast:l_x_20:l_y_20:l_o_70
```

2. Social media post with text:
```
tr-w_1200,h_630,c_fill,g_face+l_text:Welcome:l_font_Arial:l_size_48:l_color_ffffff:l_gravity_center
```

3. Product image with price tag:
```
tr-w_500,h_500,c_fill+l_text:$99.99:l_font_Arial:l_size_32:l_color_ff0000:l_gravity_northeast:l_x_10:l_y_10
```

4. Multiple overlays:
```
tr-w_1000,h_1000,c_fill+l_image:background.png:l_gravity_center+l_text:Sale:l_font_Arial:l_size_64:l_color_ffffff:l_gravity_center
```

## Best Practices for Overlays

1. **Text Overlays**:
   - Use web-safe fonts for consistent rendering
   - Consider adding a slight blur or shadow for better readability
   - Test text positioning across different image sizes

2. **Image Overlays**:
   - Use PNG format for overlays that require transparency
   - Keep overlay images small for better performance
   - Consider using opacity for watermarks

3. **Performance Considerations**:
   - Each overlay adds processing time
   - Cache complex transformations when possible
   - Optimize overlay images before uploading

4. **Responsive Design**:
   - Use relative positioning when possible
   - Consider different overlay sizes for different viewport sizes
   - Test overlay visibility on various backgrounds

Would you like me to provide more specific examples or elaborate on any particular overlay feature?
