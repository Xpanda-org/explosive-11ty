# Explosive Website Template

NOT PRODUCTION READY!!!

An 11ty (Eleventy) website template which attempts to be customisable and flexible enough for most use cases while providing a reasonable developer experience.
The template’s collection management and layout systems leverage 11ty to handle complex content structures; Open Props provides theming, and Datastar powers front-end dynamic behaviour.

## Features

### Static Pages
- Individual page templates with customisable layouts
- Support for featured images, table of contents, and related pages
- Flexible content structure with optional components
- SEO-optimised with meta tags and social sharing

### Advanced Collections System
- Posts can belong to multiple collections using `postCollections` front matter
- Dynamic collection pages with automatic post filtering
- Nested collections support (e.g., tutorials → web-development, design)
- Rich collection metadata with descriptions, colours, and featured images
- Cross-referencing between collections and posts

### Dynamic Navigation
- Automatically generated navigation from static pages and collections
- Mobile-responsive hamburger menu with smooth animations
- Footer navigation with customizable links
- Breadcrumb navigation for nested collections
- Skip links for accessibility

### Modern Design System
- **Open Props integration** for consistent, scalable design tokens
- Mobile-first responsive design with CSS Grid and Flexbox
- Comprehensive design system with colours, typography, spacing, and animations
- Card-based layouts with hover effects and smooth transitions
- Typography system with web fonts
- Built-in normalize.css and button components from Open Props

### Performance & Accessibility
- Static site generation for fast loading
- Lazy loading for images and media
- Semantic HTML structure with ARIA labels
- Screen reader support and keyboard navigation
- Optimised asset handling and caching

## Quick Start

### Prerequisites
- Node.js 14 or higher
- npm or yarn package manager

### Installation

1. **Clone or download the template**
   ```bash
   git clone https://github.com/Xpanda-org/explosive-11ty.git my-explosive-site
   cd my-explosive-site
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start development server**
   ```bash
   npm run serve
   ```

4. **Build for production**
   ```bash
   npm run build
   ```
   Or **Build for GitHub Pages**
   ```bash
   npm run build:github
   ```

Your site will be available at `http://localhost:8080` during development.

## Project Structure

```
src/
├── _includes/          # Reusable components
│   ├── header.njk      # Site header with navigation
│   └── footer.njk      # Site footer
├── _layouts/           # Page templates
│   ├── base.njk        # Base layout with HTML structure
│   ├── home.njk        # Homepage with hero and featured content
│   ├── page.njk        # Static page template
│   ├── post.njk        # Blog post template
│   ├── collection.njk  # Collection listing template
│   └── collections-overview.njk  # All collections overview
├── _data/              # Site configuration
│   ├── site.json       # Global site settings
│   ├── theme.js        # Theme configuration (colours, fonts, paths)
│   └── collectionData.json  # Collection metadata
├── _user/              # User customisations (override system)
│   ├── assets/        # Custom CSS, JS, and other assets
│   ├── data/          # Custom data files (merged with base)
│   ├── includes/      # Custom includes (override base)
│   └── layouts/       # Custom layouts (override base)
├── assets/             # Static assets
│   ├── css/           # Stylesheets
│   ├── js/            # JavaScript files
│   └── images/        # Image assets
├── posts/              # Blog posts and articles
├── pages/              # Static pages
├── collections/        # Collection overview pages
└── media/              # Media files for posts
```

## Content Creation

### Creating Posts

Create a new Markdown file in `src/posts/`:

```markdown
---
layout: post.njk
title: Your Post Title
description: Brief description for SEO and previews
date: 2024-01-15
author:
  name: Author Name
  avatar: /assets/images/authors/author.jpg
postCollections: [blog, tutorials]  # Can belong to multiple collections
tags: [javascript, web-development]
featuredImage: /assets/images/posts/featured.jpg
readingTime: 5
showShareButtons: true
showRelatedPosts: true
---

Your post content here...
```

### Creating Static Pages

Add pages to `src/pages/`:

```markdown
---
layout: page.njk
title: Page Title
description: Page description
navTitle: Nav Title  # Optional: different title for navigation
order: 1  # Optional: navigation order
showInNav: true
showInFooter: true
---

Your page content here...
```

### Configuring Collections

Edit `src/_data/collectionData.json` to customise collections:

```json
{
  "name": "tutorials",
  "displayName": "Tutorials",
  "description": "Step-by-step guides and how-to articles",
  "showInNav": true,
  "featuredImage": "/assets/images/collections/tutorials.jpg",
  "hasSubmenu": true,
  "submenuCollections": ["web-development", "design"],
  "color": "#e74c3c"
}
```

## Customization Architecture

This template uses a **User Directory** architecture to ensure that your custom changes do not create merge conflicts when pulling updates from the upstream template. All of your site's specific customisations should be placed in the `src/_user` directory.

The core template files are designed to be extended and overridden by files in `src/_user`.

### Override System Architecture

The template implements three parallel override systems that work at build time:

1. **Layout Override System** - Override template layouts
2. **Includes Override System** - Override reusable components
3. **Data Override System** - Override and merge configuration data

All three systems work the same way:
- Base files live in `src/_layouts/`, `src/_includes/`, and `src/_data/`
- User overrides live in `src/_user/layouts/`, `src/_user/includes/`, and `src/_user/data/`
- At build time, files are merged into `.cache/` directories
- Eleventy uses the merged cache directories
- No source files are ever modified - clean separation!

### How to Change Site Colours and Theme

The template uses a **Data Override System** that automatically merges your custom theme settings with the base theme. You can use either JavaScript (`.js`) or JSON (`.json`) files.

**How the Data Override System Works:**

The system uses a **deep merge** strategy at build time:

1. **Base data files** → `src/_data/` (template defaults)
2. **User data files** → `src/_user/data/` (your customizations)
3. **Build-time merge** → `.cache/data/` (merged result)
4. **Eleventy uses** → `.cache/data/` as the data directory

**Merge Behaviour:**
- **Nested objects** are merged recursively (property-level override)
- **Arrays** are replaced entirely (not merged)
- **You only specify what you want to change** - all other values are kept from base
- **No source files are modified** - clean separation prevents merge conflicts

**Example: Deep Merge in Action**

Base theme (`src/_data/theme.js`):
```javascript
{
  "colors": {
    "primary": "#333",
    "accent": "tomato",
    "background": "#fff"
  },
  "fonts": {
    "body": "Arial, sans-serif"
  }
}
```

Your override (`src/_user/data/theme.js`):
```javascript
module.exports = {
  "colors": {
    "primary": "#0056b3",      // Override primary
    "background": "#f8f9fa"    // Override background
  }
  // Note: accent color and fonts not specified
};
```

Merged result (`.cache/data/theme.js`):
```javascript
{
  "colors": {
    "primary": "#0056b3",      // ← Your override
    "accent": "tomato",         // ← Base value kept
    "background": "#f8f9fa"     // ← Your override
  },
  "fonts": {
    "body": "Arial, sans-serif" // ← Base value kept
  }
}
```

**Build Output:**

When you build, you'll see:
```
[Data Override] Merged user data: theme.js
```

This confirms your custom theme is being applied.

**Available Theme Properties:**

```javascript
{
  "colors": {
    "primary": "#333",      // Main brand colour
    "accent": "tomato",     // Accent/highlight colour
    "background": "#fff",   // Page background
    "text": "#333",         // Main text colour
    "text-light": "#666",   // Secondary text colour
    "border": "#ddd"        // Border colour
  },
  "fonts": {
    "body": "...",          // Body text font stack
    "heading": "..."        // Heading font stack
  },
  "paths": {
    "header": "header.njk", // Header template path
    "footer": "footer.njk"  // Footer template path
  }
}
```

These values are exposed as CSS custom properties:
- `var(--theme-colors-primary)`
- `var(--theme-colors-accent)`
- `var(--theme-fonts-body)`
- etc.

### How to Override Other Data Files

The Data Override System tries to stike a balance between ease of use and flexibility.
**Its main benefits are:**

**No merge conflicts** - User customizations separate from template
**Partial overrides** - Only specify what you want to change
**Type support** - Works with `.js` and `.json` files
**Automatic** - No manual configuration needed
**Consistent** - Same pattern as layouts/includes override system

The Data Override System works for **any** data file, not just `theme.js`:

**Override Site Configuration:**

```json
// src/_user/data/site.json
{
  "title": "My Custom Site Title",
  "description": "My custom description"
}
```

**Create New Data Files:**

You can also create entirely new data files in `src/_user/data/`:

```javascript
// src/_user/data/myCustomData.js
module.exports = {
  "setting": "value",
  "items": ["one", "two", "three"]
};
```

Access in templates: `{{ myCustomData.setting }}`

**Override Collection Data:**

```javascript
// src/_user/data/collectionData.json
[
  {
    "name": "blog",
    "displayName": "My Blog",
    "color": "#custom-color"
  }
]
```

### How to Add Custom CSS

1. Open `src/_user/assets/css/custom.css`.
2. Add any new CSS rules or overrides. This stylesheet is loaded after the main template stylesheet.

**Example: `src/_user/assets/css/custom.css`**
```css
.custom-hero {
  background: var(--theme-colors-primary);
  padding: 4rem 1rem;
  color: white;
}

/* Override existing styles */
body {
  font-family: var(--theme-fonts-body);
  background-color: var(--theme-colors-background);
}
```

**CSS Custom Properties from Theme:**

All theme values are automatically available as CSS custom properties:

```css
/* Use theme colors */
.my-element {
  color: var(--theme-colors-primary);
  background: var(--theme-colors-background);
  border-color: var(--theme-colors-border);
}

/* Use theme fonts */
.my-text {
  font-family: var(--theme-fonts-body);
}

.my-heading {
  font-family: var(--theme-fonts-heading);
}
```


### How to Override Layouts

This template allows you to override any of the default layouts (e.g., `post.njk`, `base.njk`) without modifying the core template files.

1. **Identify the layout** you want to override. The default layouts are in `src/_layouts/`.
2. **Create a new file** with the *same name* in the `src/_user/layouts/` directory.
3. **Add your custom content** to the new file. You can either write a completely new layout or extend the original.

**Example: Customizing the Post Layout**

```nunjucks
{# src/_user/layouts/post.njk #}
{% extends "theme/post.njk" %}

{% block post_header %}
  <div class="share-on-x">
    <a href="https://twitter.com/intent/tweet?text={{ title | urlencode }}">Share on X</a>
  </div>
  {{ super() }}
{% endblock %}
```

### How to Add Custom Includes

Create reusable template components in `src/_user/includes/`:

1. Create `src/_user/includes/my-component.njk`
2. Use it in any template with `{% include "my-component.njk" %}`

### How to Modify the Header

1. Create `src/_user/includes/my-header.njk`
2. Extend the base header and override specific blocks
3. Update `src/_user/data/theme.js`:

```javascript
module.exports = {
  "paths": {
    "header": "my-header.njk"
  }
};
```

### ⚙️ How to Add an Eleventy Plugin

1. Install: `npm install @11ty/eleventy-plugin-rss`
2. Edit `src/_user/config.js`:

```javascript
module.exports = {
  plugins: [
    {
      plugin: require("@11ty/eleventy-plugin-rss"),
      options: {}
    }
  ]
};
```

## Advanced Features

### Media Galleries

Add image galleries to posts:

```markdown
---
mediaGallery:
  - type: image
    url: /media/gallery/image1.jpg
    alt: Image description
    caption: Image caption
  - type: video
    url: /media/gallery/video.mp4
    poster: /media/gallery/video-poster.jpg
---
```

### Related Content

The template automatically suggests related posts based on collections and tags. You can also manually specify related pages:

```markdown
---
relatedPages: [about, contact]
---
```

### Custom Sections

Add custom sections to the homepage:

```markdown
---
customSections:
  - title: Custom Section
    class: my-custom-class
    content: |
      <p>Your custom HTML content here</p>
---
```

## Built-in Features

- **SEO Optimization**: Meta tags, Open Graph, Twitter Cards
- **Social Sharing**: Built-in sharing buttons for major platforms
- **Search Functionality**: Basic search implementation ready
- **Newsletter Signup**: Configurable newsletter form
- **Contact Forms**: Ready-to-use contact form with validation
- **Table of Contents**: Automatic TOC generation for long content
- **Reading Time**: Automatic reading time calculation
- **Breadcrumbs**: Navigation breadcrumbs for better UX
- **Image Carousel**: Flexible, accessible carousel component powered by Datastar

## 🎠 Image Carousel Component

The template includes a powerful and flexible image carousel component that can be added to any page type (posts, pages, collections, etc.).

### Features

- **Responsive Design**: Automatically adapts to all screen sizes
- **Touch Support**: Swipe gestures on mobile devices
- **Keyboard Navigation**: Arrow keys, Home, and End keys for navigation
- **Accessibility**: Full ARIA support for screen readers
- **Customizable**: Configure autoplay, intervals, dots, arrows, and more
- **Flexible Data Sources**: Define carousels in front matter, inline, or global data
- **Multiple Carousels**: Add multiple carousels to the same page
- **Image Metadata**: Support for captions, titles, alt text, and links

### Basic Usage

#### 1. Define Carousel in Front Matter

Add a carousel configuration to your page's front matter:

```yaml
---
title: My Page
carousels:
  hero:
    autoplay: true
    interval: 5000
    showDots: true
    showArrows: true
    height: 500px
    images:
      - src: /assets/images/slide1.jpg
        alt: First slide
        title: Welcome
        caption: This is the first slide
      - src: /assets/images/slide2.jpg
        alt: Second slide
        title: Features
        caption: Check out our features
---
```

#### 2. Use the Carousel Shortcode

In your page content, reference the carousel by its ID:

```markdown
{% carousel "hero" %}
```

### Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `autoplay` | boolean | `false` | Automatically advance slides |
| `interval` | number | `5000` | Time between slides (ms) when autoplay is enabled |
| `showDots` | boolean | `true` | Show indicator dots at the bottom |
| `showArrows` | boolean | `true` | Show previous/next navigation arrows |
| `height` | string | `'auto'` | Fixed height (e.g., `'500px'`) or `'auto'` |
| `aspectRatio` | string | `null` | Aspect ratio (e.g., `'16/9'`, `'4/3'`, `'1/1'`, `'21/9'`) |

### Image Properties

Each image in the carousel can have the following properties:

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `src` | string | Yes | Path to the image file |
| `alt` | string | Yes | Alternative text for accessibility |
| `title` | string | No | Title displayed as overlay |
| `caption` | string | No | Caption text displayed below title |
| `link` | string | No | URL to navigate to when image is clicked |
| `linkTarget` | string | No | Link target (e.g., `'_blank'`) |

### Usage Examples

#### Simple Carousel (Just Image Paths)

```yaml
carousels:
  simple:
    images:
      - /assets/images/img1.jpg
      - /assets/images/img2.jpg
      - /assets/images/img3.jpg
```

#### Full-Featured Carousel

```yaml
carousels:
  gallery:
    autoplay: false
    showDots: true
    showArrows: true
    aspectRatio: 4/3
    images:
      - src: /assets/images/product1.jpg
        alt: Product showcase
        title: Our Latest Product
        caption: Available now in stores
        link: /products/latest/
      - src: /assets/images/product2.jpg
        alt: Feature highlight
        title: Amazing Features
        caption: Discover what makes us different
```

#### Inline Carousel (No Front Matter)

You can also define a carousel inline:

```markdown
{% carousel images=[
  {src: "/assets/images/img1.jpg", alt: "Description 1"},
  {src: "/assets/images/img2.jpg", alt: "Description 2"}
] %}
```

### Multiple Carousels on One Page

You can add multiple carousels to the same page:

```yaml
---
carousels:
  hero:
    autoplay: true
    images: [...]
  gallery:
    autoplay: false
    images: [...]
  testimonials:
    showArrows: false
    images: [...]
---

{% carousel "hero" %}

## Our Gallery

{% carousel "gallery" %}

## What People Say

{% carousel "testimonials" %}
```

### Keyboard Navigation

When a carousel has focus:
- **Left Arrow**: Previous slide
- **Right Arrow**: Next slide
- **Home**: First slide
- **End**: Last slide

### Accessibility

The carousel component includes:
- ARIA labels and roles for screen readers
- Keyboard navigation support
- Focus indicators for interactive elements
- Reduced motion support for users with motion sensitivity
- Semantic HTML structure

### Customization

#### Custom Styling

Override carousel styles in your CSS:

```css
.carousel {
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
}

.carousel-control {
  background: var(--brand-primary);
}

.carousel-indicator.active {
  background: var(--brand-accent);
}
```

#### Custom Aspect Ratios

The carousel supports common aspect ratios:
- `16/9` - Widescreen (default for video content)
- `4/3` - Standard
- `1/1` - Square
- `21/9` - Ultra-wide

Or set a custom height:

```yaml
carousels:
  custom:
    height: 600px
    images: [...]
```

### Best Practices

1. **Always provide alt text** for accessibility
2. **Optimize images** before adding them to carousels
3. **Use appropriate aspect ratios** for your content
4. **Limit autoplay** to avoid annoying users
5. **Test keyboard navigation** to ensure accessibility
6. **Keep captions concise** for better readability

## Development

### Available Scripts

- `npm run serve` - Start development server with live reload
- `npm run build` - Build for production
- `npm run debug` - Build with debug information
- `npm run build:github` - Build for GitHub Pages deployment
- `npm run copy:docs` - Copy _site contents to docs folder
- `npm run deploy:github` - Build and prepare for GitHub Pages

### Adding Custom Filters

Add custom Nunjucks filters in `.eleventy.js`:

```javascript
eleventyConfig.addFilter("myFilter", function(value) {
  return value.toUpperCase();
});
```

### Custom Collections

Create custom collections in `.eleventy.js`:

```javascript
eleventyConfig.addCollection("featured", function(collectionApi) {
  return collectionApi.getFilteredByGlob("src/posts/**/*.md")
    .filter(post => post.data.featured);
});
```

## Deployment

The template generates static files in the `_site` directory, making it compatible with any static hosting service:

### GitHub Pages Deployment

This template is optimized for GitHub Pages deployment using the docs folder approach:

1. **Configure the repository name** in `package.json`:
   ```bash
   # Update the path prefix in package.json build:github script
   # Replace 'explosive-11ty' with your repository name
   "build:github": "ELEVENTY_PATH_PREFIX=/your-repo-name/ eleventy"
   ```

2. **Build and deploy**:
   ```bash
   npm run deploy:github
   ```

3. **Commit and push**:
   ```bash
   git add docs/
   git commit -m "Deploy to GitHub Pages"
   git push origin main
   ```

4. **Configure GitHub Pages**:
   - Go to your repository settings
   - Navigate to "Pages" section
   - Set source to "Deploy from a branch"
   - Select "main" branch and "/docs" folder
   - Save the settings

Your site will be available at `https://username.github.io/repository-name/`

### Other Deployment Options

- **Netlify**: Connect your Git repository for automatic deployments
- **Vercel**: Import your project for instant deployment
- **Traditional Hosting**: Upload the `_site` folder via FTP

### Local Development vs Production

- **Local development**: All paths work from root (`/`)
- **GitHub Pages**: Paths are automatically prefixed with repository name
- **Custom domain**: Update `ELEVENTY_PATH_PREFIX` to `/` for root domain deployment

### Important: Local Server After GitHub Build

**Note**: After running `npm run build:github`, your local development server will not work correctly because the `_site` folder contains GitHub Pages paths (e.g., `/repo-name/assets/css/main.css`) instead of local paths (e.g., `/assets/css/main.css`).

**Recommended Development Workflow**:

```bash
# 1. Develop locally
npm run serve

# 2. When ready to deploy to GitHub Pages
npm run deploy:github

# 3. Commit and push to GitHub
git add docs/
git commit -m "Deploy to GitHub Pages"
git push origin main

# 4. To continue local development, rebuild for local paths
npm run build
npm run serve
```

This ensures your local development environment works correctly while maintaining proper GitHub Pages deployment.

## Example Content

The template includes sample content to demonstrate all features:

- **Welcome Post**: Introduction to the template with feature overview
- **11ty Tutorial**: Comprehensive beginner's guide to 11ty
- **CSS Grid Guide**: Advanced tutorial with code examples
- **About Page**: Template for company/personal information
- **Contact Page**: Contact form and information template

## Contributing

This repository does not accept any pull requests, at least for the time being.

## Someday Maybe

### Search & Discovery
- [ ] **Full-text search**: Implement client-side search with Lunr.js or Fuse.js
- [ ] **Advanced filtering**: Add tag-based filtering and sorting options
- [ ] **Search suggestions**: Auto-complete search with popular queries
- [ ] **Related content algorithm**: Improve related posts using content similarity

### Analytics & Performance
- [ ] **Analytics integration**: Add Google Analytics 4 or privacy-focused alternatives
- [ ] **Performance monitoring**: Implement Core Web Vitals tracking
- [ ] **Image optimization**: Add responsive images with `@11ty/eleventy-img`
- [ ] **Bundle optimization**: Implement CSS/JS minification and bundling

### Enhanced UI/UX
- [ ] **Dark mode toggle**: Add user-controlled theme switching
- [ ] **Reading progress**: Show reading progress bar for long posts
- [ ] **Infinite scroll**: Implement pagination with infinite scroll
- [ ] **Print styles**: Add optimized print CSS for better printing

### Progressive Web App
- [ ] **Service Worker**: Add offline functionality and caching
- [ ] **Web App Manifest**: Make the site installable as a PWA
- [ ] **Push notifications**: Implement web push for new content alerts
- [ ] **Offline reading**: Cache posts for offline access

### Content Management
- [ ] **CMS integration**: Connect with Netlify CMS, Forestry, or Sanity
- [ ] **Draft system**: Add draft post functionality with preview
- [ ] **Content scheduling**: Implement future post publishing
- [ ] **Multi-author support**: Enhanced author profiles and management

### Internationalization
- [ ] **Multi-language support**: Add i18n with language switching
- [ ] **RTL support**: Right-to-left language compatibility
- [ ] **Localized URLs**: Language-specific URL structures
- [ ] **Translation management**: Workflow for content translation

### Communication Features
- [ ] **Newsletter integration**: Connect with Mailchimp, ConvertKit, or similar
- [ ] **Comment system**: Add Disqus, Utterances, or custom comments
- [ ] **Contact form backend**: Integrate with Netlify Forms or Formspree
- [ ] **Social media integration**: Auto-posting to social platforms

### Security & Privacy
- [ ] **Content Security Policy**: Implement CSP headers
- [ ] **Privacy compliance**: Add GDPR/CCPA compliance features
- [ ] **Spam protection**: Add reCAPTCHA or similar to forms
- [ ] **Security headers**: Implement security best practices

### SEO & Marketing
- [ ] **Advanced SEO**: Add JSON-LD structured data
- [ ] **Sitemap generation**: Automatic XML sitemap creation
- [ ] **RSS feeds**: Generate RSS feeds for collections
- [ ] **Social media cards**: Enhanced Open Graph and Twitter Card support

### Developer Experience
- [ ] **Storybook integration**: Component documentation and testing
- [ ] **Testing framework**: Add unit and integration tests
- [ ] **GitHub Actions**: Automated testing and deployment workflows
- [ ] **Documentation site**: Comprehensive documentation with examples

### Content Features
- [ ] **Series support**: Multi-part article series with navigation
- [ ] **Bookmarking**: User bookmarking functionality
- [ ] **Content ratings**: Star ratings or like/dislike system
- [ ] **Content recommendations**: AI-powered content suggestions

### Advanced Analytics
- [ ] **Heatmap tracking**: User interaction heatmaps
- [ ] **A/B testing**: Built-in A/B testing framework
- [ ] **Conversion tracking**: Goal and conversion analytics
- [ ] **User journey mapping**: Track user paths through content

## License

This template is open source and available under the MIT License.

---

**Built using 11ty (Eleventy), Datastar, and Open Props**
