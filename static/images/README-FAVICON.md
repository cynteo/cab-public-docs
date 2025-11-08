# Favicon Setup

## Required Files

To complete the favicon setup, add these image files to this directory:

1. **favicon.ico** - Place in `/static/` directory (root level)
   - Classic .ico format
   - Multiple sizes: 16x16, 32x32, 48x48

2. **favicon-16x16.png** - Place here in `/static/images/`
   - 16x16 pixels PNG
   - For modern browsers

3. **favicon-32x32.png** - Place here in `/static/images/`
   - 32x32 pixels PNG
   - For modern browsers

4. **apple-touch-icon.png** - Place here in `/static/images/`
   - 180x180 pixels PNG
   - For iOS home screen icons

## Creating Favicons

### Option 1: Use Online Generator
Visit [favicon.io](https://favicon.io/favicon-generator/) or [realfavicongenerator.net](https://realfavicongenerator.net/)

1. Upload your logo/image
2. Generate all favicon sizes
3. Download the package
4. Extract files to the appropriate directories

### Option 2: Use Your Logo
If you have a Cynteo logo:

1. Resize to 32x32 pixels (square)
2. Export as PNG
3. Use an online converter to create .ico file
4. Create larger versions for apple-touch-icon

## Recommended Approach

1. Start with your company logo (square format preferred)
2. Use [favicon.io](https://favicon.io/) to generate all sizes
3. Place files in correct directories
4. Commit and push

## Current Configuration

The HTML is already configured in `/layouts/partials/head-favicon.html` and linked in the base template.

Once you add the image files, they will automatically appear!

