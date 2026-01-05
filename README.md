# Redd-Archiver Demo Archive

A static HTML demo archive showcasing the output of [Redd-Archiver v1.0.0](https://github.com/19-84/redd-archiver), a PostgreSQL-backed archive generator for multi-platform link aggregator data.

**What's Included:** Browsable static HTML with nested comments, user pages, multiple sort options, and responsive dark/light themes.

**What's Not Included:** Full-text search (requires Docker + PostgreSQL deployment).

## Archive Contents

| Metric | Value |
|--------|-------|
| **Communities** | 3 (Reddit, Voat, Ruqqus) |
| **Total Posts** | 9,592 |
| **Total Comments** | 65,719 |
| **Unique Users** | 15,977 |
| **Content Span** | 2014-2024 (10 years) |

### Communities Included

| Community | Platform | Posts | Comments | Users | Last Archived |
|-----------|----------|-------|----------|-------|---------------|
| **[v/programming](https://online-archives.github.io/redd-archiver-example/v/programming/index.html)** | Voat | 4,003 | 17,719 | 4,875 | April 2020 |
| **[r/RedditCensors](https://online-archives.github.io/redd-archiver-example/r/RedditCensors/index.html)** | Reddit | 1,998 | 40,188 | 9,915 | February 2024 |
| **[g/technology](https://online-archives.github.io/redd-archiver-example/g/technology/index.html)** | Ruqqus | 3,591 | 7,812 | 1,187 | October 2021 |

## Features

**Static HTML Archive**
- Pure HTML + CSS (no JavaScript required)
- Responsive design with automatic dark/light theme
- Nested comment threads with collapsible `<details>` elements
- Per-user activity pages with pagination
- Multiple sorting options (score, date, comments)

**Privacy & Offline**
- Zero external resources (system fonts only)
- No tracking, cookies, or analytics
- Browse offline or over Tor
- Future-proof preservation format

**Performance**
- Sub-second page loads
- Minified CSS (96KB)
- Works on low-bandwidth connections

## How This Archive Was Generated

```bash
# Import Voat data (v/programming)
python reddarc.py /data \
  --output ./demo-archive \
  --subverse programming \
  --platform voat \
  --import-only

# Import Reddit data (r/RedditCensors)
python reddarc.py /data \
  --output ./demo-archive \
  --subreddit RedditCensors \
  --comments-file RedditCensors_comments.zst \
  --submissions-file RedditCensors_submissions.zst \
  --import-only

# Import Ruqqus data (g/technology)
python reddarc.py /data \
  --output ./demo-archive \
  --guild technology \
  --platform ruqqus \
  --import-only

# Export to static HTML (no filters applied)
python reddarc.py /data \
  --output ./demo-archive \
  --base-url https://demo.redd-archiver.org \
  --site-name "Redd Archive Demo" \
  --export-from-database
```

**Archive Metrics:**
- Raw posts: 10,068 across 3 communities
- Archived posts: 9,592 (95.3% - 476 posts excluded due to invalid data)
- No score or comment filters applied
- Database size: ~350MB (PostgreSQL)
- Output size: ~45MB (static HTML + CSS)
- Processing time: ~8 minutes

## Technical Stack

| Component | Technology |
|-----------|------------|
| **Generator** | [Redd-Archiver v1.0.0](https://github.com/19-84/redd-archiver) |
| **Processing** | PostgreSQL 12+ with streaming architecture |
| **Templates** | Jinja2 with bytecode caching |
| **Output** | Static HTML/CSS (zero runtime dependencies) |

**Data Flow:**
`.zst`/`.7z`/SQL dumps → PostgreSQL database → Jinja2 rendering → Static HTML

## File Structure

```
demo-archive/
├── index.html                    # Dashboard with statistics
├── r/RedditCensors/              # Reddit subreddit
│   ├── index.html                # Posts sorted by score
│   ├── index-comments.html       # Sorted by comment count
│   ├── index-date.html           # Sorted by date
│   └── comments/{id}/{slug}/     # Individual post pages
├── v/programming/                # Voat subverse
├── g/technology/                 # Ruqqus guild
├── user/{username}/              # User activity pages
│   └── index-{page}.html         # Paginated user history
├── static/
│   └── styles.css                # Minified CSS (96KB)
├── sitemap.xml                   # Sitemap index
├── sitemap-posts-*.xml           # Per-community sitemaps
└── robots.txt                    # Search engine directives
```

## Getting Started

### View Locally
```bash
# Clone and open
git clone <repository-url>
cd demo-archive
open index.html  # macOS
xdg-open index.html  # Linux
start index.html  # Windows
```

### Deploy to Static Host

**GitHub Pages / Codeberg Pages:**
```bash
git push origin gh-pages
```

**Netlify / Vercel / Cloudflare Pages:**
- Drag and drop the output folder, or
- Connect repository and deploy

**Self-Hosted:**
```bash
cp -r output/* /var/www/html/
```

## Build Your Own Archive

Want to create your own archive with **full-text search**?

Deploy with Docker + PostgreSQL for:
- GIN-indexed PostgreSQL full-text search
- Advanced filtering (keywords, author, date, score)
- Streaming architecture for constant memory usage
- Scalable to millions of posts/comments

See the [source repository](https://github.com/19-84/redd-archiver) for:
- Quick Start: `QUICKSTART.md` (5-15 min setup)
- Docker deployment with search server
- Tor hidden service deployment
- API and MCP server integration

## License

**Generator:** [Redd-Archiver](https://github.com/19-84/redd-archiver) (Unlicense - Public Domain)

**Content:** Public posts from Voat (defunct 2020), Ruqqus (defunct 2021), and Reddit (archived under fair use).

---

**Generated by:** Redd-Archiver v1.0.0
**Last Updated:** 2026-01-05
**Note:** This is a static export. Deploy with Docker for full-text search.
