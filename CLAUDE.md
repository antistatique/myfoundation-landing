# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Hugo static site for "My Foundation" - a philanthropic platform that supports a different charitable project each month through recurring donations. The site features a single-page layout showcasing three support tiers (Bedrock, Builders, Sponsors) and displays projects aligned with International Days throughout the year.

## Development Commands

```bash
# Start development server with live reload
hugo server

# Build the site for production
hugo

# Create a new project page
hugo new content/projects/project-name.md
```

The built site is output to `public/` (gitignored).

## Architecture

### Theme Structure

This project uses a custom Hugo theme called "mf" located in `themes/mf/`. The theme follows Hugo's standard directory structure:

```
themes/mf/
├── layouts/
│   ├── _default/baseof.html    # Base HTML template with <head> and AOS setup
│   ├── index.html              # Homepage layout (header → content → support → projects → footer)
│   ├── partials/               # Reusable components
│   │   ├── header.html         # Logo and support button
│   │   ├── footer.html         # Newsletter, support, social sections
│   │   ├── projects.html       # Projects grid display
│   │   ├── support.html        # Support options (Bedrock/Builders/Sponsors)
│   │   ├── logo.html           # Site logo SVG
│   │   ├── social.html         # Social media icons
│   │   └── svg.html            # SVG icon renderer
│   └── shortcodes/             # Content building blocks
│       ├── hero.html           # Hero section with optional project logos
│       ├── box.html            # Content box with optional icon
│       ├── multibox.html       # Container for multiple boxes
│       ├── quote.html          # Quote block with author attribution
│       ├── button.html         # Call-to-action button
│       ├── why.html            # "Why" section wrapper
│       ├── how.html            # "How it works" section
│       └── icon.html           # Icon renderer
└── static/
    ├── css/
    │   ├── style.css           # Main stylesheet
    │   └── aos.css             # Animate On Scroll library
    ├── fonts/                  # Web fonts
    ├── images/                 # Static images
    └── js/
        └── aos.js              # Animate On Scroll library (only JS dependency)
```

### Content Structure

**Homepage** (`content/_index.md`): Uses shortcodes to build the single-page layout with hero, why, how, vision/mission sections.

**Projects** (`content/projects/*.md`): Each project has front matter with:
- `title`: Project name
- `description`: Brief description
- `project_logo`: SVG/image filename (stored in static/images/)
- `project_link`: External project URL
- `project_category`: Category (child, health, water, virus)
- `project_date`: Month in YYYY-MM format
- `draft`: Boolean for visibility

The hero shortcode displays up to 12 most recent project logos when `show-project-logo="true"`.

### Configuration

**hugo.toml**: Contains all site configuration including:
- `params.supportOptions`: Array defining Bedrock, Builders, and Sponsors tiers (title, subtitle, icon, description, button config)
- `params.footer`: Newsletter, support, and social media configuration
- Footer credits link to Antistatique
- `markup.goldmark.renderer.unsafe = true` allows HTML in markdown

**.pages.yml**: Configuration for CMS integration (pages and projects collections with field definitions).

### JavaScript Philosophy

**Keep JavaScript minimal for optimal performance.** The site only uses:
1. AOS (Animate On Scroll) for fade animations on scroll
2. Lazy loading opacity animation for images
3. Optional service worker (currently commented out)
4. Plausible analytics script

Do not add additional JavaScript libraries unless absolutely necessary. Prefer CSS-only solutions.

### Layout Flow

1. `baseof.html` defines the HTML structure, includes CSS, and initializes AOS
2. `index.html` defines the page flow: header → content → support → projects → footer
3. Homepage content uses shortcodes to compose sections
4. Support section renders three cards from `params.supportOptions`
5. Projects section displays project grid from `content/projects/`

### Styling Approach

The site uses a custom CSS file (`themes/mf/static/css/style.css`) with no CSS framework. Styling is minimal and performance-focused. AOS provides scroll animations via data attributes (`data-aos`, `data-aos-duration`).

## Important Notes

- No build dependencies (no npm, no package.json)
- Hugo extended version required for potential SCSS processing
- Site is optimized for speed and user experience
- All project logos should be SVG format when possible for scalability
- The site uses a single-page architecture with anchor navigation
