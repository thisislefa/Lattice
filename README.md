# Lattice — Blog & Articles Grid Component

## Overview

Lattice is a responsive blog/article grid component designed for content-heavy websites, portfolios, and digital publications. Featuring a clean, card-based layout with subtle hover effects and optimized image handling, it presents content in an accessible, visually engaging format.

## Live Preview

[View Demo](https://thisislefa.github.io/lattice)  

## Technology Specifications

### Core Stack
- **HTML5**: Semantic markup with accessibility features
- **CSS3**: Grid layout, CSS custom properties, responsive design
- **No JavaScript**: Pure CSS implementation
- **Google Fonts**: Inter font family (400-800 weights)
- **Unsplash API**: High-quality placeholder images

### Performance Features
- **Optimized images**: Object-fit cover with fixed dimensions
- **Responsive images**: Served at appropriate sizes
- **CSS Grid**: Efficient layout rendering
- **No external dependencies**: Self-contained component


## Architecture

### File Structure
```
lattice/
├── index.html          # Semantic HTML with image fallbacks
├── style.css           # CSS Grid, variables, responsive design
└── README.md
```

### CSS Architecture
- **CSS Custom Properties**: Theme variables for easy customization
- **CSS Grid**: Three-column desktop, two-column tablet, single-column mobile
- **Mobile-first responsive**: Media queries for progressive enhancement
- **Transition-based animations**: Smooth hover effects using transform and box-shadow
- **BEM-inspired naming**: Clear, maintainable class structure

### Image Handling Strategy
1. **Unsplash CDN**: High-performance image delivery
2. **Object-fit cover**: Consistent image cropping
3. **Fixed aspect ratio**: Maintains layout stability
4. **Fallback mechanism**: Placeholder images on error
5. **Alt text implementation**: Accessibility compliance

---

## Integration Guide

### Basic Implementation
```html
<!-- Add to head -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" rel="stylesheet">

<!-- Add to body -->
<link rel="stylesheet" href="path/to/lattice/style.css">
```

### Content Management System Integration

#### WordPress (PHP Template)
```php
<?php if (have_posts()) : ?>
<div class="article-container">
    <header class="section-header">
        <h2 class="section-title"><?php echo get_the_title(get_option('page_for_posts')); ?></h2>
    </header>
    
    <section class="article-grid">
        <?php while (have_posts()) : the_post(); ?>
        <a href="<?php the_permalink(); ?>" class="article-card" aria-label="Read <?php the_title(); ?>">
            <div class="article-image-wrapper">
                <?php if (has_post_thumbnail()) : ?>
                    <?php the_post_thumbnail('medium_large', ['class' => 'article-image', 'alt' => get_the_title()]); ?>
                <?php else : ?>
                    <img src="https://images.unsplash.com/photo-1497366754035-f200968a6e72?q=80&w=1169&auto=format&fit=crop" class="article-image" alt="<?php the_title(); ?>">
                <?php endif; ?>
            </div>
            <div class="article-content">
                <div class="article-meta">
                    <span><?php echo get_the_date(); ?></span>
                    <span class="meta-divider">•</span>
                    <span><?php echo reading_time(); ?> minute read</span>
                </div>
                <h3 class="article-title"><?php the_title(); ?></h3>
            </div>
        </a>
        <?php endwhile; ?>
    </section>
</div>
<?php endif; ?>
```

#### React/Next.js Component
```jsx
import './lattice.css';

export default function LatticeGrid({ posts }) {
  return (
    <div className="article-container">
      <header className="section-header">
        <h2 className="section-title">Blog & Articles</h2>
      </header>
      
      <section className="article-grid">
        {posts.map((post) => (
          <a 
            key={post.id} 
            href={post.url} 
            className="article-card"
            aria-label={`Read ${post.title}`}
          >
            <div className="article-image-wrapper">
              <img 
                src={post.image} 
                alt={post.title}
                className="article-image"
                loading="lazy"
                decoding="async"
              />
            </div>
            <div className="article-content">
              <div className="article-meta">
                <span>{post.date}</span>
                <span className="meta-divider">•</span>
                <span>{post.readTime} minute read</span>
              </div>
              <h3 className="article-title">{post.title}</h3>
            </div>
          </a>
        ))}
      </section>
    </div>
  );
}
```

## Customization

### Theming System
Modify CSS variables in `:root`:
```css
:root {
    --font-primary: 'Inter', sans-serif;
    --color-background: #f8f9fa; /* Lighter background */
    --color-text-dark: #212529; /* Darker text */
    --color-text-muted: #6c757d;
    --color-divider: #adb5bd;
    --color-card-bg: #ffffff;
    --card-radius: 24px; /* More rounded corners */
    --image-radius: 16px;
}
```

### Layout Configuration
Adjust grid behavior:
```css
.article-grid {
    grid-template-columns: repeat(4, 1fr); /* 4 columns on desktop */
    gap: 2rem; /* Increased spacing */
}

/* Custom breakpoints */
@media (max-width: 1200px) {
    .article-grid {
        grid-template-columns: repeat(3, 1fr);
    }
}

@media (max-width: 768px) {
    .article-grid {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 480px) {
    .article-grid {
        grid-template-columns: 1fr;
    }
}
```

### Interactive Enhancements
Add focus states for accessibility:
```css
.article-card:focus {
    outline: 3px solid var(--color-primary, #4dabf7);
    outline-offset: 2px;
    box-shadow: 0 0 0 4px rgba(77, 171, 247, 0.2);
}
```

## Advanced Features Implementation

### Lazy Loading Images
Add native lazy loading:
```html
<img 
    src="image.jpg" 
    alt="Description" 
    class="article-image"
    loading="lazy"
    decoding="async"
    width="400"
    height="250"
>
```

### CSS Container Queries (Advanced)
For component-level responsiveness:
```css
.article-card {
    container-type: inline-size;
}

@container (min-width: 300px) {
    .article-title {
        font-size: 1.5rem;
    }
}
```

### Dark Mode Support
Add media query for dark mode:
```css
@media (prefers-color-scheme: dark) {
    :root {
        --color-background: #1a1a1a;
        --color-text-dark: #ffffff;
        --color-text-muted: #aaaaaa;
        --color-card-bg: #2d2d2d;
    }
    
    .article-card {
        box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
    }
    
    .article-card:hover {
        box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
    }
}
```

### Performance Optimizations

#### Critical CSS Inlining
Extract above-the-fold styles:
```html
<style>
.article-container {
    max-width: 1200px;
    width: 100%;
    margin: 0 auto;
}

.section-header {
    text-align: center;
    margin-bottom: 4rem;
}

/* Additional critical styles */
</style>
```

#### Font Loading Optimization
```html
<link rel="preload" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" rel="stylesheet">
</noscript>
```

## Accessibility Compliance

### WCAG 2.1 AA Standards
- **Semantic HTML**: Proper heading hierarchy and landmark roles
- **Keyboard navigation**: All cards are keyboard accessible
- **Focus indicators**: Visible focus states (can be enhanced)
- **Color contrast**: Minimum 4.5:1 ratio for text
- **Screen reader support**: Descriptive alt text and aria labels

### ARIA Implementation
```html
<section class="article-grid" role="feed" aria-label="Blog articles">
    <article role="article" aria-labelledby="article-1" class="article-card">
        <!-- Content -->
    </article>
</section>
```

## Browser Support

| Browser | Version | Support | Notes |
|---------|---------|---------|-------|
| Chrome  | 58+     | Full    | Optimal performance |
| Firefox | 52+     | Full    | CSS Grid support |
| Safari  | 10.1+   | Full    | Requires prefix for some features |
| Edge    | 16+     | Full    | Modern Edge versions |
| Mobile  | iOS 10.3+/Android 8+ | Full | Touch-optimized |

### Fallbacks for Older Browsers
```css
.article-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
}

@supports (display: grid) {
    .article-grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
    }
}
```

## SEO Optimization

### Best Practices Implemented
1. **Semantic markup**: Proper use of headings and sections
2. **Structured data ready**: Schema.org Article markup can be added
3. **Image optimization**: Alt text, dimensions, and lazy loading
4. **Performance**: Fast loading impacts SEO positively

### Article Schema Implementation
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "ItemList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "item": {
        "@type": "Article",
        "headline": "5 design principles that elevate your projects",
        "datePublished": "2025-07-13",
        "timeRequired": "PT10M",
        "image": "https://images.unsplash.com/photo-1497366754035-f200968a6e72"
      }
    }
  ]
}
</script>
```

## Troubleshooting

### Common Issues and Solutions

#### Image Aspect Ratio Problems
**Issue**: Images stretching or cropping incorrectly.
**Solution**: Ensure consistent aspect ratio:
```css
.article-image-wrapper {
    aspect-ratio: 16/9; /* Modern browsers */
    padding-bottom: 56.25%; /* Fallback for older browsers */
    height: auto;
}

.article-image {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}
```

#### Grid Layout Breaking
**Issue**: Cards not aligning properly in older browsers.
**Solution**: Add flexbox fallback:
```css
.article-grid {
    display: flex;
    flex-wrap: wrap;
}

.article-card {
    flex: 1 1 300px;
    margin: 0.5rem;
}

@supports (display: grid) {
    .article-grid {
        display: grid;
        margin: 0;
    }
    
    .article-card {
        margin: 0;
    }
}
```

#### Font Loading Issues
**Issue**: FOUT (Flash of Unstyled Text) or font not loading.
**Solution**: Implement font-display strategy:
```css
@font-face {
    font-family: 'Inter';
    font-display: swap;
    /* Font face definitions */
}
```

#### Hover Effects on Touch Devices
**Issue**: Hover states persisting after touch.
**Solution**: Use hover media query:
```css
@media (hover: hover) and (pointer: fine) {
    .article-card:hover {
        transform: translateY(-5px);
        box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
    }
}
```

## Performance Monitoring

### Lighthouse Audit Checklist
- [ ] **Performance**: >90 score (image optimization, font loading)
- [ ] **Accessibility**: >90 score (contrast, semantics, labels)
- [ ] **Best Practices**: >90 score (HTTPS, modern APIs)
- [ ] **SEO**: >90 score (structured data, meta tags)

### Web Vitals Targets
- **LCP (Largest Contentful Paint)**: <2.5s
- **FID (First Input Delay)**: <100ms
- **CLS (Cumulative Layout Shift)**: <0.1

## Maintenance

### Regular Updates
1. **Content rotation**: Update article cards quarterly
2. **Image optimization**: Compress and convert to WebP
3. **Dependency updates**: Monitor Google Fonts changes
4. **Browser testing**: Quarterly compatibility checks

### Version History
- **v1.0**: Initial release with 3-column grid
- **v1.1**: Added responsive improvements and fallbacks
- **v1.2**: Enhanced accessibility and performance

## License

MIT License — Free for personal and commercial use with attribution.

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/enhancement`)
3. Commit changes (`git commit -m 'Add enhancement'`)
4. Push to branch (`git push origin feature/enhancement`)
5. Open a Pull Request


## Support

For technical issues:
1. Check the troubleshooting section
2. Test with browser developer tools
3. Open an issue with specific details

---

**Lattice** — Structured content presentation for modern digital experiences.﻿

