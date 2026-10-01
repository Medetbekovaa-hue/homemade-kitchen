# Homemade Kitchen

**Made From Scratch. Served With Love.**

A responsive, production-ready frontend for *Homemade Kitchen*, a family-style restaurant in Chicago, IL. It's built with **React + Vite** and served in production by **nginx** in a multi-stage Docker image.

> Frontend only: there is no backend or database. Online ordering and catering requests work entirely in the browser for demo purposes.

---

## Features

- **Hero** with the headline, a subheading, and "View Our Menu" / "Order Now" calls to action
- **Sticky navigation** that highlights the active section, plus an **Order Online** button with a cart badge and a mobile hamburger menu
- **Featured Dishes**: six dish cards, each with an image, description, price, and Order button
- **Our Story**: "Food That Feels Like Home"
- **Why Choose Us**: Fresh Ingredients, Made From Scratch, Family Recipes, Prepared Daily
- **Tabbed Menu**: Starters, Main Dishes, Pasta, Soups & Salads, Desserts, Drinks
- **Today's Homemade Special**: a promo block for Slow-Cooked Beef Pot Roast ($19.99)
- **Catering**: event types and a validated "Request Catering" form
- **Customer Reviews**: four testimonial cards
- **Contact / Location**: address, phone, hours, an embedded map, and "Get Directions" / "Call Us" buttons
- **Footer**: quick links, social icons (Instagram, Facebook, TikTok), and copyright
- **Order drawer**: add, remove, and change quantities; pickup or delivery; tax and total; order confirmation
- Fully responsive for desktop, tablet, and mobile. Keyboard accessible, with reduced-motion support

## Tech Stack

| Layer      | Tool                                 |
| ---------- | ------------------------------------ |
| UI         | React 19                             |
| Build tool | Vite 7                               |
| Language   | JavaScript (ES modules, JSX)         |
| Styling    | Plain CSS (custom properties)        |
| Linting    | ESLint 9 (flat config)               |
| Serving    | nginx (alpine) in Docker             |

## Getting Started

**Prerequisites:** Node.js 20.19+ (Node 22 LTS recommended) and npm.

```bash
npm install      # install dependencies
npm run dev      # start dev server at http://localhost:5173
```

### Available scripts

| Command           | Description                                     |
| ----------------- | ----------------------------------------------- |
| `npm run dev`     | Start the Vite development server with HMR      |
| `npm run build`   | Create an optimized production build in `dist/` |
| `npm run preview` | Preview the production build locally            |
| `npm run lint`    | Run ESLint on the whole project                 |

## Docker

The `Dockerfile` uses a **multi-stage build**:

1. **Build stage** (`node:22-alpine`): runs `npm ci` and then `npm run build`
2. **Runtime stage** (`nginx:alpine`): copies `dist/` into `/usr/share/nginx/html` and serves it on **port 80**

```bash
# Build the image
docker build -t homemade-kitchen:latest .

# Run the container
docker run -d --name homemade-kitchen -p 8080:80 homemade-kitchen:latest

# Open http://localhost:8080
```

`nginx.conf` provides:

- Single-page-app fallback to `index.html`
- Long-term immutable caching for hashed `/assets/*`, with `no-cache` on `index.html`
- gzip compression and basic security headers
- A `/healthz` endpoint, which the Docker `HEALTHCHECK` uses

## Project Structure

```
homemade-kitchen/
├── public/
│   └── favicon.svg
├── src/
│   ├── components/        # One component per section, each with its own CSS
│   │   ├── Header.jsx
│   │   ├── Hero.jsx
│   │   ├── FeaturedDishes.jsx
│   │   ├── OurStory.jsx
│   │   ├── WhyChooseUs.jsx
│   │   ├── Menu.jsx
│   │   ├── TodaysSpecial.jsx
│   │   ├── Catering.jsx
│   │   ├── Reviews.jsx
│   │   ├── Contact.jsx
│   │   ├── Footer.jsx
│   │   ├── OrderDrawer.jsx
│   │   └── ...
│   ├── data/content.js    # All restaurant content: dishes, menu, reviews, hours
│   ├── styles/global.css  # Design tokens, base styles, buttons
│   ├── utils/format.js
│   ├── App.jsx
│   └── main.jsx
├── Dockerfile
├── nginx.conf
├── .dockerignore
├── .gitignore
├── eslint.config.js
├── vite.config.js
├── index.html
└── package.json
```

## Customizing Content

All text, prices, menu items, reviews, hours, and image URLs are in **`src/data/content.js`**. Edit that one file to update the site. Colors and fonts are CSS custom properties at the top of **`src/styles/global.css`**.

## Credits

Food photography is from [Unsplash](https://unsplash.com) and is loaded from the Unsplash CDN.

---

© 2026 Homemade Kitchen.
# homemade-kitchen
