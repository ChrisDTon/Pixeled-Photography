# Pixeled-Photography

## Project Idea

Make photos look like pixel art through dynamic downscaling and gradient maps. If the devices are strong enough, processing happens directly on device letting you see changes and settings as you tune. If you do not the pictures, if a server is boot, will be processed in the cloud, letting you see a preview after submiting settings, and finalize.

### Inspiration

The Pixeless camera by Carlo, but made into a software rather than using physical hardware to achieve the effect, letting you take similar pictures with the devices you own.

## System

### Pipeline

1. Image uploaded
2. Image displayed on preview with settings applied, if server processed show a waiting line asking for initial settings to be applied
3. User apply settings
4. Downscale image
5. Apply color science e.g. Grayscale Conversion + Gradient Palette Mapping
6. Upscale image, if wanted
7. Download

### Responsive Design

Front-end design for both mobile and desktop platforms. 
Both will also have a design for whether or not there is onboard processing or processing will be off loaded to cloud.

### File handling

Can make a user account to save settings and files to cloud, but all functionality should be available without doing so.
Settings should also be exportable as a file that can be locally stored and imported later.
If there is onboard processing, images will not leave the devices and never enter a server.
If handled through cloud, the images will not be permanently saved, unless explicitly asked to do so by user.

#### User functionality
- Saving settings such as downscaling amount, and gradient maps.
- Storing images finished processed images.
- Limit storage capacity to 1 GB, or unlimited.

## Functionality

### Processing

Both client-side processing, or server-side processing depending on device capabilities, or preference of the user.

### Downscaling

- Nearest Neighbor, best for pixel art scaling
- Bilinear, creates smooth downscale
- Bicubic, creates sharper downscale 
- Should be able to set amount.
- Checkbox for a relative or absolute scale. Relative will be based of the dimensions of an image, making similarly sized images look as pixelated with setting applied. Absolute will use a scaling based on resolution making similarly sized images, but more granular look different to each other.

### Color

- Gradient map or color quantization.
- Gradient map ignores the original color, only focusing on luminance / brightness and applies colors according to a predetermined strip i.e. mapping brightness to colors and gradient.
- Color quantization uses the colors of the images and removes granularity by decreasing ranges and picking out a few specific hues.

### Upscaling

- Most modern devices will apply bilinear upscaling to displayed small images making the downscaled image look blurry. Avoid this by giving the option to upscale the image when saving locally, not to server.
- Upscaling is always handled using nearest neighbor to retain pixel effect.

### Listed

1. Sign in.
2. Sign up.
3. User page.
- Saved images, readjust, delete, or download.
- Saved settings, adjustable within, delete, or download.
4. Editing page.
- Next to sign in and sign up option, a button to toggle client-side or server side processing
- Upload button.
- Support JPG, PNG, WebP, RAW, AVIF.
- Option for downscaling method, defaulting to bilinear.
- Option for downscaling amount.
- Options for downscaling scale, relative or absolute.
- Option for color quantization or gradient map.
- If color quantization, no more options for color, shows a strip showing what color the image will be using i.e. the palette.
- If gradient map, default to grayscale, lets user add a color for every point between 1-100.
- Import button to only load the gradient map from settings file.
- Download button, with a upscale checkbox.
- Save button to save the processed image to cloud.
- Share button to send or download the image to other services.
- Export settings button to locally save a file containing all settings i.e. downscaling method, amount, relative/absolute, color science.
