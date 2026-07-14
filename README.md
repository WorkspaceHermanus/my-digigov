# MyDigiGovSA Static Website

This project is a production-quality static HTML, CSS and JavaScript recreation of the supplied MyDigiGovSA marketing website mockup. It uses semantic HTML5, modular CSS, local SVG assets and small progressive JavaScript for navigation and reveal behavior.

## File Structure

```text
/
├── index.html
├── assets/
│   ├── images/
│   │   ├── avatar-lerato.svg
│   │   └── hero-worker.svg
│   ├── icons/
│   │   ├── favicon.svg
│   │   └── icon-sprite.svg
│   └── fonts/
├── css/
│   ├── reset.css
│   ├── variables.css
│   ├── base.css
│   ├── components.css
│   ├── sections.css
│   └── responsive.css
├── js/
│   ├── main.js
│   └── navigation.js
└── README.md
```

## Run Locally

Open `index.html` directly in a browser, or serve the folder with any simple static server.

Example:

```sh
python3 -m http.server 4173
```

Then visit `http://localhost:4173`.

No package installation or build process is required for the static page.

## Replace Images

Local artwork lives in `assets/images/`.

- Replace `hero-worker.svg` with a production hero image or SVG. Keep the filename, or update the reference in `css/sections.css`.
- Replace `avatar-lerato.svg` with a real profile image or brand-approved illustration. Keep the filename, or update the image path in `index.html`.
- Use descriptive `alt` text for meaningful images.

## Replace the Logo

The current logo is text-based and appears in `index.html` as `.brand`.

To replace it with an image:

1. Add the logo file to `assets/images/` or `assets/icons/`.
2. Replace the `.brand__mark` and `.brand__name` spans with an `<img>`.
3. Keep the `aria-label="MyDigiGovSA home"` on the home link.

## Edit Colours

The design system is defined in `css/variables.css`.

Edit these variables to adjust the brand palette:

- `--color-navy`
- `--color-navy-dark`
- `--color-green`
- `--color-green-dark`
- `--color-green-light`
- `--color-background`
- `--color-surface`
- `--color-text`
- `--color-text-muted`
- `--color-border`

Other layout tokens, such as container width, section spacing, radius, shadows and type scale, are also in `css/variables.css`.

## Edit Navigation

Navigation links are in the header of `index.html`.

If you add or remove a section:

1. Update the `<nav>` list in the header.
2. Make sure the linked section has a matching `id`.
3. Update footer links if needed.

The mobile menu behavior is handled in `js/navigation.js`.

## Add or Remove Service Cards

Service cards are in the "One Secure Digital Workplace" section of `index.html`.

To add a card:

1. Copy an existing `.service-card` article.
2. Change the icon symbol, heading and description.
3. Confirm the icon exists in `assets/icons/icon-sprite.svg`.

The grid automatically adapts from four columns to two columns to one column through `css/responsive.css`.

## Deploy to Static Hosting

Upload these files and folders to any static host:

- `index.html`
- `assets/`
- `css/`
- `js/`

No server-side code is required. If deploying under a subdirectory, keep asset paths relative or adjust them to match the hosting path.
