---
layout: home.njk
title: Explosive Website Template's Documentation
description: Demonstrating and Documenting the use of the Explosive Websit Template
showPageTitle: false
showFeaturedPosts: true
featuredPostsTitle: Latest Posts
featuredPostsCount: 6
showFeaturedCollections: true
featuredCollectionsTitle: Dig Deeper
featuredCollectionsCount: 2
hero:
  title: Welcome to Explosive Website
  subtitle: Flexible websites with minimum hassale
  description: |
    <p>Explosive is a flexible 11ty website template that makes it easy to create beautiful, organized content with collections and static pages.</p>
    <p>Perfect for blogs, documentation sites, portfolios, and more!</p>
  backgroundImage: /assets/images/hero-bg.jpg
  ctaButton:
    text: Get Started
    url: /about/
carousels:
  hero:
    autoplay: true
    interval: 5000
    showDots: true
    showArrows: true
    height: 500px
    images:
      - src: /assets/images/carousel/hero/hero1.svg
        alt: Modern web development
        title: Build Fast Websites
        caption: Create lightning-fast static sites with 11ty
        link: /pages/about/
      - src: /assets/images/carousel/hero/hero2.svg
        alt: Responsive design
        title: Mobile-First Design
        caption: Beautiful on every device
      - src: /assets/images/carousel/hero/hero3.svg
        alt: Easy to use
        title: Simple & Powerful
        caption: Markdown-based content creation
customSections:
  - title: Why Choose Explosive?
    class: features-section
    content: |
      <div class="features-grid">
        <div class="feature-item">
          <h3>🚀 Fast & Modern</h3>
          <p>Built with 11ty for lightning-fast static site generation and modern web standards.</p>
        </div>
        <div class="feature-item">
          <h3>📱 Responsive Design</h3>
          <p>Looks great on all devices with mobile-first responsive design principles.</p>
        </div>
        <div class="feature-item">
          <h3>🎨 Flexible Collections</h3>
          <p>Organize your content with powerful collections that can be nested and cross-referenced.</p>
        </div>
        <div class="feature-item">
          <h3>⚡ Easy to Use</h3>
          <p>Simple Markdown-based content creation with powerful templating capabilities.</p>
        </div>
      </div>
---

{% carousel "hero" %}

## Welcome to Your New Website

This is the home page of your Explosive Website template. You can customize this content by editing the `src/index.md` file.

### What You Can Do

- **Create Static Pages**: Add pages to the `src/pages/` directory
- **Write Posts**: Add blog posts and articles to `src/posts/`
- **Organize with Collections**: Use the `collections` front matter to categorize your posts
- **Customize Templates**: Modify the templates in `src/_layouts/` and `src/_includes/`

### Getting Started

1. Edit this home page by modifying `src/index.md`
2. Create your first post in `src/posts/`
3. Add static pages to `src/pages/`
4. Customize the site configuration in `src/_data/site.json`

Enjoy building with the Explosive Website template!
