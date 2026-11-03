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
- If server is down, show popup error message, but client side processing should work flawlessly without
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

## Technology

### Note
- Server side compute and storage will be handled using services and self-owned processing and storage, showing production-ready skills, knowing how to leverage cloud ecosystems, while having the privacy focused solution ready for own use.
- Rather than branches that will make upkeep complicated, it will be handled through same branch using a component-based approach.
- A flexible app that toggles its backend with a single configuration switch

### Deployment
1. Service component
- Vercel Automatic Git Deploys
3. Self-hoseted
- Docker & Docker Compose
- Wrap Express app, Postgres, and MinIO into containers so they can all spin up on server with a single command

### Language

#### Programming

1. TypeScript / JavaScript
- Front-end: Powers Next.js application, handles state management (like tracking slider values), and performs the client-side pixel manipulation inside the HTML5 Canvas loop.
- Back-end: Powers server-side processing engine (whether running on Vercel Serverless or a standalone Express.js server) using Node.js to drive the Sharp image library.
2. SQL (Structured Query Language)
- Used to talk to PostgreSQL database (via Supabase or Docker). Basic SQL queries or migrations to create tables that store user account details, settings presets, and image file paths.

#### Markup & Structure

1. TSX / JSX (TypeScript/JavaScript XML)
- Component markup language used by React/Next.js.
- Looks almost identical to HTML but allows embedding of TypeScript logic directly into UI structure (e.g., rendering your image preview canvas or mapping over saved palette options).
2. HTML5
- Underlying standard browser markup.
- While no pure .html files in this project, I leverage HTML5 elements such as the native <canvas> element for on-device image rendering—and standard file input types (<input type="file">).

#### Styling

1. CSS (Cascading Style Sheets)
- Foundational styling language
2. Tailwind Utility Directives
- Directly inside TSX markup.
- Specific CSS rendering properties like image-rendering: pixelated; to make downscaled canvases look perfectly crisp and retro in the browser.

#### Configuration & Serialization

1. JSON (JavaScript Object Notation)
- To serialize user settings so they can be exported as a local file (settings.json).
- Data type database will use (JSONB) to store dynamic palette hex arrays.
2. YAML
- Used exclusively to build the self-hosted version to write docker-compose.yml file, which orchestrates local server, Postgres, and MinIO storage containers.

### User-free File Handling
- Store in LocalStorage or IndexedDB
- JSON file download (settings.json)

### Front-end

#### Framework
- Next.js

#### Markup language
- HTML5

#### Styling
- Tailwind CSS + shadcn/ui

#### Client-Side Canvas/WebGL
- Abandoned math heavy WebGL, that slowed down processing
- standard HTML5 Canvas 2D Context

### Back-end

#### Server-Side Processing
1. Service component
- Next.js API Routes + 'Sharp' (Image Processing Library)
- Host on Vercel or Netlify
2. Self-hosted
- Image processing code using Sharp stays 100% identica
- Node.js (Express or Fastify)

#### Database
- Text, numbers, booleans, and JSON configurations. (e.g., User profiles, custom gradient hex codes, slider positions).
- Instant and queryable. Can ask it to: "Find the settings preset created by User X on Tuesday."
- Small strings of text (a few bytes or kilobytes per row).
1. Service component
- Supabase (Auth, Postgres for settings, 1GB Free Object Storage)
2. Self-hosted
- PostgreSQL in a Docker Container

#### File Storage
- Large, raw binary files (Blobs). (e.g., The original .jpg or .png images and the finalized pixel art files).
- Path-based (URLs). Can only ask it to: "Give me the file located at /users/123/avatar.png."
- Large files (Megabytes to Gigabytes per file).
1. Service component
- Supabase Storage (S3-compatible)
3. Self-hosted
- MinIO or Local File System, MinIO is an open-source, self-hosted storage server that mimics Amazon S3. Because it uses the same API as Supabase/S3, code barely has to change.

# Monorepo idea

Never store actual high-res images directly inside a database row because it destroys the database's performance. Instead, upload the image to File Storage, get back a URL string (e.g., https://storage.com), and save that text URL inside the database.

pixeled-photography/
  ├── frontend/             # Next.js + Tailwind UI (Shared by both backends)
  ├── backend-serverless/   # API routes tailored for Vercel deployment
  ├── backend-selfhosted/   # Express.js server + Dockerfile for your private server
  └── docker-compose.yml    # Spins up Postgres, MinIO, and Express locally with 1 click

// Automatically switch the target based on where the app is running
const API_URL = process.env.NEXT_PUBLIC_ENVIRONMENT === 'self-hosted'
  ? 'http://your-server-ip:5000/api'
  : '/api/serverless'; 
