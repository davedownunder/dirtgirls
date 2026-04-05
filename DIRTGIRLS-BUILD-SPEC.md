# Dirt Girls Gardening — Website Build Spec

## Project Overview

Rebuild dirtgirlsgardening.com.au (currently WordPress at dirtgirls.com.au) as a modern static site. The site is for a garden maintenance business on the Mornington Peninsula, Victoria. The audience skews older (60+), so accessibility, readability, and simplicity are paramount.

**Live domain:** dirtgirls.com.au (also dirtgirlsgardening.com.au)
**Current host:** WordPress.com
**New stack:** Astro + Vercel + GitHub

---

## Tech Stack

| Layer | Choice | Notes |
|-------|--------|-------|
| Framework | **Astro 5.x** | Static site generation, Markdown blog, zero JS by default |
| Hosting | **Vercel** (Hobby/free tier) | Auto-deploy from GitHub |
| Repo | **GitHub** | Private repo |
| CSS | **Vanilla CSS** with CSS custom properties | No Tailwind — keep it simple and fast |
| Forms | **Formspree** (free tier) | Sends submissions to julie.goldie@bigpond.com |
| Newsletter | **Buttondown** (free tier, up to 100 subs) | RSS-to-email for auto-send of blog posts |
| Analytics | **Vercel Web Analytics** (free tier, 50K events/mo) | Privacy-friendly, no cookies, no banner needed |
| Images | **Local** — all images downloaded and committed to repo | No external dependencies on WordPress CDN |
| Fonts | **Google Fonts** — loaded via `<link>` | Two fonts max |

---

## Design Direction

### Aesthetic: Warm, Earthy, Clean

**NOT** a startup website. NOT minimalist-tech. Think: a well-kept garden noticeboard crossed with a country magazine. Warm, approachable, slightly rustic but clean and readable.

### Design Tokens

```css
:root {
  /* Earthy greens and warm neutrals */
  --color-primary: #4A7C59;       /* Garden green */
  --color-primary-dark: #3A5F46;  /* Deeper green for hover/active */
  --color-accent: #D4A574;        /* Warm terracotta/tan */
  --color-accent-light: #F0E0CC;  /* Light warm background */
  --color-bg: #FDFBF7;            /* Off-white, warm paper */
  --color-bg-alt: #F5F0E8;        /* Slightly darker warm bg */
  --color-text: #2D2A26;          /* Near-black brown */
  --color-text-light: #6B6560;    /* Muted brown for secondary text */
  --color-white: #FFFFFF;
  --color-border: #E0D8CC;        /* Warm border */

  /* Typography */
  --font-display: 'Playfair Display', Georgia, serif;
  --font-body: 'Source Sans 3', 'Segoe UI', sans-serif;

  /* Sizing — generous for older eyes */
  --text-base: 1.125rem;    /* 18px base */
  --text-lg: 1.375rem;      /* 22px */
  --text-xl: 1.75rem;       /* 28px */
  --text-2xl: 2.25rem;      /* 36px */
  --text-3xl: 3rem;         /* 48px */
  --line-height: 1.7;
  --line-height-heading: 1.2;

  /* Spacing */
  --space-xs: 0.5rem;
  --space-sm: 1rem;
  --space-md: 1.5rem;
  --space-lg: 2.5rem;
  --space-xl: 4rem;
  --space-2xl: 6rem;

  /* Layout */
  --max-width: 1100px;
  --border-radius: 8px;
}
```

### Typography Rules

- **Minimum body text: 18px.** No exceptions.
- **Line height: 1.7** for body copy (generous for readability).
- **Headings:** Playfair Display (serif) — warm and traditional.
- **Body:** Source Sans 3 (sans-serif) — highly legible, slightly humanist.
- **Maximum line length:** 70 characters (~40rem). Prevents eye strain.
- **Link styling:** Underlined, high contrast. No colour-only differentiation.
- **Button text:** At least 16px, generous padding (16px 32px minimum).

### Accessibility Requirements (Non-negotiable)

- WCAG AA contrast ratios everywhere
- All text scalable, no fixed px on body text
- Focus states visible on all interactive elements
- Skip-to-content link
- Semantic HTML throughout (nav, main, article, aside, footer)
- Alt text on all images
- No hamburger menu — full horizontal nav visible at all times (stacks vertically on mobile)
- Phone number is a clickable `tel:` link
- Email is a clickable `mailto:` link
- No animations that can't be disabled (respect `prefers-reduced-motion`)
- No infinite scroll, no lazy-loaded content, no "read more" truncation

---

## Site Structure

### Navigation (always visible)

```
[Logo]  Home  |  Services  |  Garden Tips  |  Contact     📞 0412 222 251
```

On mobile, the nav items stack vertically (NOT a hamburger menu). Phone number always visible.

### Page 1: Home (`/`)

**Sections in order:**

1. **Hero**
   - Large heading: "Mornington Peninsula Garden Maintenance"
   - Subheading: "Passionate about gardens on the Mornington Peninsula since [year]"
   - Large photo (use existing hero image from WordPress site)
   - CTA button: "Get a Free Quote" (links to contact page)
   - Phone number displayed prominently: "Call Julie: 0412 222 251"

2. **How It Works** (3-step process from existing site)
   - Walk the Garden → "We'll drop by, walk the garden, find out your likes and dislikes."
   - Free Quote → "How often we should come, for how long, and what's needed."
   - Happy Garden → "This is a personalised service, so ensuring you're happy is critical."
   - Each step gets a simple icon (use SVG, not icon font)

3. **Services Summary**
   - Brief list: Weeding, Pruning, Planting, Mulching, Garden Maintenance, Rose Care, Garden Cleanups
   - Explicit note: "Please note we don't do lawn mowing or large hedging."
   - Link to full Services page

4. **Testimonials Carousel** (simple, auto-advances slowly, manual prev/next)
   - Testimonial 1 (from existing site): "Definitely consider Dirt Girls for your Gardening needs. Very very happy with my garden makeover and tidy. Reliable, nothing was too much trouble and definitely hard working. They got in there and got the job done. Awesome ladies… Thank you" — May Williams
   - Leave 2-3 placeholder testimonial slots for Julie to fill in (with obvious placeholder text like "Your review here — ask Julie to add")
   - Source more from Facebook page if accessible

5. **Latest Blog Posts**
   - Show 3 most recent posts with title, date, and first sentence
   - "More Gardening Tips →" link

6. **Newsletter Signup**
   - Heading: "Free Gardening Tips for the Peninsula"
   - Subtext: "Seasonal advice delivered straight to your inbox. No spam, just gardening."
   - Single email field + "Subscribe" button
   - Powered by Buttondown (form action to Buttondown endpoint)

7. **Contact Strip**
   - Phone: 0412 222 251 (clickable)
   - Email: julie.goldie@bigpond.com (clickable)
   - "Servicing the Mornington Peninsula"

### Page 2: Services (`/services`)

**Sections:**

1. **Page heading:** "Our Garden Services"
2. **Service blocks** — each with a heading, short description, and relevant photo:
   - Garden Maintenance (regular visits)
   - Weeding
   - Pruning (roses, shrubs, fruit trees)
   - Planting & Garden Design
   - Mulching
   - Garden Cleanups & Makeovers
   - Seasonal Care

3. **"We don't do" note:** "We specialise in garden care, not lawn mowing or large-scale hedging. For the best results, we focus on what we love."

4. **Service area:** "We service the Mornington Peninsula including Mornington, Mount Martha, Frankston, Somerville, Hastings, Rosebud, Rye, Sorrento, Portsea, Dromana, Safety Beach, and surrounding areas."

5. **CTA:** "Ready to get started? Call Julie on 0412 222 251 or get a free quote."

### Page 3: Garden Tips / Blog (`/blog`)

- Simple chronological list (newest first)
- Each entry shows: title, date, description, tags
- Clean single-column article pages
- At the bottom of every blog post:
  - "Get tips like this in your inbox" → newsletter signup
  - "Need help with your garden? Call us: 0412 222 251"
- Pagination if >10 posts per page
- Tag pages for filtering (optional, nice-to-have)

### Page 4: Contact (`/contact`)

1. **Heading:** "Let's Talk About Your Garden"
2. **Contact details** (large, prominent):
   - 📞 0412 222 251 (clickable tel: link)
   - ✉️ julie.goldie@bigpond.com (clickable mailto: link)
   - 📍 Servicing the Mornington Peninsula, Victoria
3. **Contact form** (Formspree):
   - Name (required)
   - Phone (required)
   - Email (required)
   - Message (textarea, required)
   - "Send Message" button
   - Success message: "Thanks! Julie will be in touch soon."
   - Form action: Formspree endpoint → delivers to julie.goldie@bigpond.com
4. **Map embed** — Google Maps showing Mornington Peninsula area (not a specific address)

---

## Images to Download from WordPress

Download ALL of these and commit to `/public/images/` in the repo:

```
https://dirtgirls.com.au/wp-content/uploads/2019/12/dirt-girls-gardening.jpg            → logo/brand image
https://dirtgirls.com.au/wp-content/uploads/2019/12/2_gzzkzb.jpg                       → hero/header image
https://dirtgirls.com.au/wp-content/uploads/2019/12/dirt-girls-gardening-mornington-peninsula-5.jpg  → garden photo 1
https://dirtgirls.com.au/wp-content/uploads/2019/12/dirt-girls-gardening-mornington-peninsula-6.jpg  → garden photo 2
https://dirtgirls.com.au/wp-content/uploads/2019/12/dirt-girls-gardening-mornington-peninsula-8-450x599-1.jpg → garden photo 3
https://dirtgirls.com.au/wp-content/uploads/2019/12/garden.jpg                          → garden photo 4
https://dirtgirls.com.au/wp-content/uploads/2019/12/cropped-dirt-girls.jpg              → favicon/small logo
```

**Image processing during build:**
- Convert all images to WebP format with JPEG fallback
- Optimize to reasonable sizes (hero: 1200px wide max, thumbnails: 400px)
- Use Astro's built-in `<Image>` component for automatic optimization
- All images must have descriptive alt text

---

## Blog Setup

### Content Collection (Astro)

```
src/
  content/
    blog/
      01-keeping-your-garden-alive-in-a-peninsula-heatwave.md
      02-summer-feeding-what-your-plants-are-hungry-for.md
      ... (all 24 posts)
    config.ts  → defines blog schema
```

### Frontmatter Schema

```typescript
import { defineCollection, z } from 'astro:content';

const blog = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    description: z.string(),
    pubDate: z.date(),
    tags: z.array(z.string()).optional(),
    image: z.string().optional(),
  }),
});

export const collections = { blog };
```

### Auto-Publishing

Posts with future `pubDate` values should NOT appear on the site until that date. Filter in the blog index:

```javascript
const posts = (await getCollection('blog'))
  .filter(post => post.data.pubDate <= new Date())
  .sort((a, b) => b.data.pubDate.valueOf() - a.data.pubDate.valueOf());
```

---

## Newsletter (Buttondown)

### Setup

- Create Buttondown account (free tier)
- Enable RSS-to-email: point it at the site's RSS feed (`/rss.xml`)
- Buttondown will automatically send new blog posts to subscribers

### Signup Form

Simple HTML form on homepage and blog post footers:

```html
<form
  action="https://buttondown.com/api/emails/embed-subscribe/dirtgirlsgardening"
  method="post"
  target="popupwindow"
>
  <label for="bd-email">Get free gardening tips:</label>
  <input type="email" name="email" id="bd-email" placeholder="Your email address" required />
  <button type="submit">Subscribe</button>
</form>
```

---

## Contact Form (Formspree)

### Setup

- Create Formspree account (free tier: 50 submissions/month — plenty)
- Create form endpoint
- Form submissions deliver to julie.goldie@bigpond.com

### Form HTML

```html
<form action="https://formspree.io/f/{FORM_ID}" method="POST">
  <label for="name">Name</label>
  <input type="text" id="name" name="name" required />

  <label for="phone">Phone</label>
  <input type="tel" id="phone" name="phone" required />

  <label for="email">Email</label>
  <input type="email" id="email" name="email" required />

  <label for="message">How can we help?</label>
  <textarea id="message" name="message" rows="5" required></textarea>

  <button type="submit">Send Message</button>
</form>
```

Form fields should be large (min height 48px), with generous spacing. Labels above fields, not beside them. Large submit button, full-width on mobile.

---

## Analytics (Vercel Web Analytics)

### Setup

```bash
npm install @vercel/analytics
```

In the Astro root layout:

```astro
---
import Analytics from '@vercel/analytics/astro';
---
<html lang="en-AU">
  <head>...</head>
  <body>
    <slot />
    <Analytics />
  </body>
</html>
```

### What This Gives You (Free)

- Page views per page
- Unique visitors
- Top referrers
- Top pages
- Country/region
- Device type (mobile vs desktop)
- 50K events/month included

**No cookies. No consent banner needed. GDPR/Privacy Act compliant.**

Dashboard at: vercel.com → Project → Analytics tab

---

## SEO

### Technical SEO

- **Sitemap:** Auto-generated via `@astrojs/sitemap`
- **RSS feed:** Auto-generated via `@astrojs/rss` (also feeds Buttondown)
- **robots.txt:** Allow all
- **Canonical URLs** on every page
- **Meta descriptions** on every page (from frontmatter for blog posts)
- **Open Graph tags** on every page (title, description, image)

### Schema Markup (JSON-LD)

**LocalBusiness schema on every page:**

```json
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "Dirt Girls Gardening",
  "description": "Professional garden maintenance service on the Mornington Peninsula, Victoria.",
  "telephone": "+61412222251",
  "email": "julie.goldie@bigpond.com",
  "url": "https://dirtgirls.com.au",
  "areaServed": {
    "@type": "Place",
    "name": "Mornington Peninsula, Victoria, Australia"
  },
  "serviceType": ["Garden Maintenance", "Pruning", "Weeding", "Mulching", "Planting"],
  "priceRange": "$$"
}
```

**BlogPosting schema on each blog post** (auto-generated from frontmatter).

### Page Titles Format

- Home: `Dirt Girls Gardening — Mornington Peninsula Garden Maintenance`
- Services: `Our Services — Dirt Girls Gardening`
- Blog index: `Garden Tips — Dirt Girls Gardening`
- Blog post: `{Post Title} — Dirt Girls Gardening`
- Contact: `Contact Us — Dirt Girls Gardening`

---

## Project File Structure

```
dirtgirls-website/
├── public/
│   ├── images/
│   │   ├── dirt-girls-gardening.jpg       ← logo
│   │   ├── hero.jpg                       ← hero image
│   │   ├── garden-1.jpg                   ← garden photos
│   │   ├── garden-2.jpg
│   │   ├── garden-3.jpg
│   │   ├── garden-4.jpg
│   │   └── favicon.jpg                    ← small logo
│   ├── robots.txt
│   └── favicon.ico
├── src/
│   ├── components/
│   │   ├── Header.astro
│   │   ├── Footer.astro
│   │   ├── Nav.astro
│   │   ├── ContactStrip.astro
│   │   ├── NewsletterSignup.astro
│   │   ├── TestimonialCarousel.astro
│   │   ├── ServiceCard.astro
│   │   ├── BlogPostCard.astro
│   │   └── SEO.astro
│   ├── content/
│   │   ├── blog/
│   │   │   ├── 01-keeping-your-garden-alive-in-a-peninsula-heatwave.md
│   │   │   ├── ... (all 24 posts)
│   │   │   └── 24-hydrangeas.md
│   │   └── config.ts
│   ├── layouts/
│   │   ├── BaseLayout.astro
│   │   └── BlogPost.astro
│   ├── pages/
│   │   ├── index.astro
│   │   ├── services.astro
│   │   ├── contact.astro
│   │   ├── blog/
│   │   │   ├── index.astro
│   │   │   └── [...slug].astro
│   │   └── rss.xml.ts
│   └── styles/
│       └── global.css
├── astro.config.mjs
├── package.json
├── tsconfig.json
└── README.md
```

---

## Astro Config

```javascript
// astro.config.mjs
import { defineConfig } from 'astro/config';
import sitemap from '@astrojs/sitemap';
import vercel from '@astrojs/vercel';

export default defineConfig({
  site: 'https://dirtgirls.com.au',
  output: 'static',
  adapter: vercel({
    webAnalytics: { enabled: true }
  }),
  integrations: [sitemap()],
});
```

---

## Deployment

### GitHub Repo Setup

1. Create private repo: `github.com/{dave-username}/dirtgirls-website`
2. Commit all code + images + blog posts

### Vercel Setup

1. Connect Vercel to the GitHub repo
2. Framework preset: Astro
3. Enable Web Analytics in Vercel dashboard
4. Add custom domain: `dirtgirls.com.au`
5. Vercel will provision SSL automatically

### DNS Cutover

Update DNS records for dirtgirls.com.au:
- `A` record → Vercel IP (76.76.21.21)
- `CNAME` for `www` → `cname.vercel-dns.com`

If dirtgirlsgardening.com.au should also work, add it as an alias in Vercel and update its DNS too.

---

## Dave's Todo List (The Only Manual Steps)

| # | Task | Time | Notes |
|---|------|------|-------|
| 1 | Create Formspree account, get form ID | 5 min | formspree.io → free tier → copy endpoint |
| 2 | Create Buttondown account, get embed URL | 5 min | buttondown.com → free tier → enable RSS-to-email |
| 3 | Update DNS for dirtgirls.com.au | 5 min | Point A record to 76.76.21.21 |
| 4 | Verify Vercel domain | 2 min | Vercel dashboard → Domains |
| 5 | Ask Julie for 2–3 more testimonials | When convenient | Text/names to add to the testimonials section |

**Total Dave effort: ~20 minutes + a text to Mum**

---

## Build Checklist for Claude Code

- [ ] Scaffold Astro project
- [ ] Install dependencies (`@vercel/analytics`, `@astrojs/sitemap`, `@astrojs/rss`)
- [ ] Download all 7 images from WordPress and save to `/public/images/`
- [ ] Create global CSS with design tokens above
- [ ] Build BaseLayout with Header, Nav, Footer, Analytics
- [ ] Build Homepage with all sections
- [ ] Build Services page
- [ ] Build Contact page with Formspree form
- [ ] Build Blog index page
- [ ] Build Blog post template
- [ ] Copy all 24 blog posts into `src/content/blog/`
- [ ] Create RSS feed
- [ ] Add sitemap integration
- [ ] Add JSON-LD schema markup
- [ ] Add SEO component (meta, OG tags)
- [ ] Add newsletter signup component
- [ ] Add testimonial display
- [ ] Test mobile responsiveness
- [ ] Test accessibility (contrast, focus states, semantic HTML)
- [ ] Verify all images have alt text
- [ ] Init git repo, commit all, push to GitHub

---

## Annual Refresh Process

Once a year (in a single Claude chat session):

1. Write 24 new blog posts with updated seasonal content
2. Commit to the repo
3. They auto-publish on schedule and auto-send via Buttondown RSS

No other maintenance required. No WordPress updates, no plugin patches, no database backups. It just sits there and works.
