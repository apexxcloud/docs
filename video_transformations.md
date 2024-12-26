
# Video Transformation Documentation

## Basic Format
Transformations follow this pattern: `tr-{parameter_name}_{value}`
Multiple parameters can be combined with commas: `tr-w_200,h_100`
Multiple transformation sets can be chained with plus signs: `tr-w_200,h_100+blur_10`

## Available Transformations

### 1. Dimensions and Scaling
- **Width**: `w_{pixels}`
  - Sets video width in pixels (1-5000)
  - Example: `tr-w_300`

- **Height**: `h_{pixels}`
  - Sets video height in pixels (1-5000)
  - Example: `tr-h_200`

- **Aspect Ratio**: `ar_{ratio}`
  - Sets desired aspect ratio (width/height)
  - Example: `tr-ar_1.5`

### 2. Cropping and Resizing
- **Crop Mode**: `c_{mode}`
  - Available modes:
    - `crop`: Extracts region without scaling
    - `fill`: Scales to fill dimensions, crops excess
    - `scale`: Exact resize to dimensions
    - `fit`: Scales to fit within dimensions
    - `limit`: Only scales down if larger
    - `pad`: Adds padding to reach dimensions
  - Example: `tr-c_crop`

- **Gravity**: `g_{position}`
  - Controls positioning for crop/pad operations
  - Positions: `center` (default), `north`, `south`, `east`, `west`, `north_east`, `north_west`, `south_east`, `south_west`
  - Example: `tr-g_center`

### 3. Effects
- **Background**: `b_{option}`
  - For padding operations
  - Options: hex color (e.g., `000000`) or `blur`
  - Example: `tr-b_000000` or `tr-b_blur`

- **Blur**: `blur_{strength}`
  - Applies box blur effect
  - Example: `tr-blur_10`

### 4. Format
- **Format**: `f_{format}`
  - Output formats: `mp4` (default), `webm`, `mov`, `auto`
  - Codecs:
    - mp4/mov: libx264/aac
    - webm: libvpx-vp9/libvorbis
  - Example: `tr-f_webm`

### 5. Layers (Overlays)
Layers are added using the `l_{type}:{options}` parameter. Multiple layers can be added in a transformation chain.

#### Text Layer
```
tr-l_text:Hello World:l_font_Arial:l_size_32:l_color_FFFFFF:l_gravity_north_west:l_x_20:l_y_20:l_o_100
```
Parameters:
- Text content (first parameter after type)
- `l_font_{name}`: Font family name
- `l_size_{pixels}`: Font size in pixels (1-500, default: 24)
- `l_color_{hex}`: Hex color code without # (default: 000000)
- `l_gravity_{position}`: Position reference point (default: center)
- `l_x_{pixels}`: X offset from gravity point
- `l_y_{pixels}`: Y offset from gravity point
- `l_o_{opacity}`: Opacity 0-100 (default: 100)

#### Image Layer
```
tr-l_image:l_key_watermarks@@logo.png:l_gravity_south_east:l_x_10:l_y_10:l_o_50:l_scale_1.0:l_width_100:l_height_100:l_ar_1.5
```
Parameters:
- `l_key_{path}`: Media path (use @@ instead of /)
- `l_gravity_{position}`: Position reference point (default: center)
- `l_x_{pixels}`: X offset from gravity point
- `l_y_{pixels}`: Y offset from gravity point
- `l_o_{opacity}`: Opacity 0-100 (default: 100)
- `l_scale_{factor}`: Scale factor 0-10 (default: 1.0)
- `l_width_{pixels}`: Target width
- `l_height_{pixels}`: Target height
- `l_ar_{ratio}`: Target aspect ratio

#### Video Layer
```
tr-l_video:l_key_overlays@@intro.mp4:l_gravity_center:l_x_0:l_y_0:l_o_100:l_scale_1.0
```
- Same parameters as image layer

Example with multiple layers:
```
tr-w_1280,h_720,l_image:l_key_bg.png:l_x_0:l_y_0+l_text:Hello:l_color_FFFFFF:l_x_10:l_y_10
```

Notes:
1. Layer parameters are separated by colons (:)
2. Each layer starts with `l_{type}:`
3. Multiple layers can be chained with plus (+) signs
4. Layer order matters (first to last)
5. All layer parameters must be prefixed with "l_" except the text content in text layers
6. Media paths use @@ instead of / in the path

Gravity Options:
- `center` (default)
- `north`, `south`, `east`, `west`
- `north_east`, `north_west`, `south_east`, `south_west`

### 6. Thumbnails
- **Thumbnail**: `thumb-so_{seconds}`
  - Generates thumbnail at timestamp
  - Can combine with image transformations
  - Example: `thumb-so_5.0,w_300,h_200`

## Examples

1. Resize with blurred padding:
```
tr-w_800,h_600,c_pad,b_blur
```

2. Add watermark:
```
tr-w_1280,h_720+l_image,key_watermarks@@logo.png,x_10,y_10,g_south_east,op_50
```

3. Text overlay with background:
```
tr-c_fill+l_text,text_Copyright 2024,color_FFFFFF,size_24,x_0,y_20,g_south
```

4. Multiple layers:
```
tr-w_1280,h_720+l_image,key_bg.png,x_0,y_0+l_text,text_Hello,x_10,y_10
```


