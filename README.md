# Simple Image Generator — Pollinations.ai

A lightweight browser-based image generator powered by [Pollinations.ai](https://pollinations.ai).

The app provides a simple UI for generating images with Pollinations using **Bring Your Own Pollen (BYOP)** authentication. Users sign in with their Pollinations account, and image generation usage is charged to their own account.

## Features

* 🔐 Sign in with Pollinations OAuth
* 💳 Bring Your Own Pollen — users pay for their own generation usage
* 🎨 Multiple image generation models
* 📐 Several image sizes and aspect ratios
* 🌱 Optional seed for reproducible generations
* 🚫 Option to disable the Pollinations logo
* 🖼️ Image preview directly in the browser
* 💾 Download generated images
* 🔗 Open generated images at full size
* 📱 Responsive layout for desktop and mobile
* 🌙 Minimal dark UI
* 🔑 Session-based token storage using `sessionStorage`
* ⚡ No backend required

## Available Models

The interface currently provides three models:

| Model                           | Description                                     |
| ------------------------------- | ----------------------------------------------- |
| `lykon/dreamshaper-8-lcm`       | DreamShaper 8 LCM — cheaper generation          |
| `tongyi-mai/z-image-turbo`      | Z-Image Turbo — balanced option                 |
| `microsoft/mai-image-2.6-flash` | MAI Image 2.6 Flash — higher-quality generation |

The available models depend on the Pollinations API and may change over time.

## How It Works

The application is completely client-side.

### 1. Authentication

When the user clicks **Sign in with Pollinations**, the application redirects them to Pollinations:

```text
https://enter.pollinations.ai/authorize
```

The application sends:

* `client_id`
* `redirect_uri`
* `scope`

After authorization, Pollinations redirects back to the application with the API key in the URL hash.

The application immediately removes the hash from the address bar and stores the key in `sessionStorage`.

### 2. Image Generation

When the user generates an image, the application builds a Pollinations image URL:

```text
https://gen.pollinations.ai/image/{prompt}
```

with parameters such as:

```text
model
width
height
seed
nologo
key
```

The image is then loaded directly by the browser.

### 3. Session Storage

The user's Pollinations key is stored under:

```text
polli_byop_token
```

using `sessionStorage`.

This means the key is available only for the current browser session and is removed when the session storage is cleared.

## Requirements

No backend or build system is required.

You only need:

* A modern web browser
* A Pollinations account
* A registered Pollinations application key

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/your-username/your-repository.git
cd your-repository
```

### 2. Configure the application key

Open the HTML file and locate:

```js
const APP_KEY = 'pk_...';
```

Replace it with your Pollinations application key.

The application key is intended to be public and can be included in client-side code.

### 3. Configure the Redirect URI

The application dynamically calculates its redirect URI:

```js
function redirectUri() {
  return location.origin + location.pathname + location.search;
}
```

Therefore, the exact page URL must be registered as an allowed Redirect URI for your Pollinations application.

For example:

```text
https://example.com/
```

or:

```text
https://example.com/generator.html
```

Make sure the URL registered with Pollinations exactly matches the URL used by the application.

### 4. Run the application

Because this is a static application, you can use any static HTTP server.

For example, with Python:

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

You can also deploy the project to any static hosting provider.

## Project Structure

The project can be as simple as:

```text
.
└── index.html
```

All HTML, CSS, and JavaScript are contained in a single file.

This keeps the project easy to deploy and useful as a minimal example of integrating Pollinations image generation into a client-side application.

## Keyboard Shortcut

After signing in, you can press:

```text
Ctrl + Enter
```

or:

```text
Cmd + Enter
```

inside the prompt field to generate an image.

## Image Generation Parameters

### Prompt

The text description used to generate the image.

Example:

```text
A red fox sitting in a mossy forest, soft morning light, highly detailed
```

### Model

Selects the Pollinations image generation model.

### Size

Available sizes currently include:

```text
1024x1024
1280x720
720x1280
768x768
```

### Seed

Leaving the seed empty generates an image with a random seed.

Providing a seed can be useful when you want reproducible generations.

### No Logo

When enabled, the application sends:

```text
nologo=true
```

to the Pollinations API.

## Security Notes

This application is designed around Pollinations' client-side BYOP authentication flow.

The Pollinations application key:

```js
const APP_KEY = 'pk_...';
```

is a **public client identifier** and is intentionally included in the frontend.

The user's personal API key is different. It is returned after authentication and stored temporarily in:

```text
sessionStorage
```

The OAuth callback also removes the URL hash immediately:

```js
history.replaceState(null, '', redirectUri());
```

This prevents the returned key from remaining visible in the browser's address bar.

### Important

This project should **not** be treated as a general-purpose secure API-key management architecture.

If you add a backend, database, user accounts, or privileged operations, authentication and secret management should be redesigned accordingly.

## Error Handling

If image generation fails, the application displays an error message.

Possible causes include:

* Invalid or expired authentication
* Insufficient Pollen balance
* Invalid generation parameters
* Temporary Pollinations API problems
* Network connectivity issues
* An unavailable model

The application suggests checking the user's Pollen balance or trying another model when an image fails to load.

## Downloading Images

The **Download** button first attempts to fetch the generated image and save it as a local file:

```text
pollinations-{timestamp}.jpg
```

If the browser blocks the request because of CORS or another network restriction, the application opens the image in a new tab instead so the user can save it manually.

## Deployment

Because the application has no backend and no build step, it can be deployed to practically any static hosting service.

Examples include:

* GitHub Pages
* Cloudflare Pages
* Netlify
* Vercel
* Any traditional web server

When deploying, remember to register the production URL as the application's Pollinations Redirect URI.

## Technologies

* HTML5
* CSS3
* Vanilla JavaScript
* Pollinations.ai Image API
* Pollinations authentication

No frameworks or external JavaScript dependencies are required.

## License

Add your preferred license here.

For example:

```text
MIT License
```

if you want to release the project under the MIT license.

---

## Acknowledgements

Image generation is provided by [Pollinations.ai](https://pollinations.ai).

This project is a lightweight client-side interface built on top of the Pollinations platform.
