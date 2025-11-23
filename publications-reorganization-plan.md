# Publications Page Reorganization Plan

## Overview
Reorganize the publications page to display two distinct sections: **Manuscripts** and **Published Work**, with simplified button displays (PDF, HTML, DOI only) and no metrics.

## Current State

### Website Structure
- Theme: al-folio (Jekyll-based academic theme)
- Navigation: Automatically generated from pages with `nav: true` in front matter
- Publications: Currently displayed using jekyll-scholar plugin from `_bibliography/papers.bib`
- Current publications page: `_pages/publications.md`
- Bibliography layout: `_layouts/bib.liquid`

### Navigation Items (by nav_order)
1. About (home) - `/`
2. Publications - `/publications/` (nav_order: 2)
3. Projects - `/projects/` (nav_order: 3)
4. CV - `/cv/` (nav_order: 5)
5. Teaching - `/teaching/` (nav_order: 6)
6. Submenus (dropdown) - (nav_order: 8)
7. Blog - `/blog/` (currently visible, to be hidden)

## Objectives

### 1. Navigation Changes
- **Keep**: About, Publications, Projects, CV, Teaching, People, and other default tabs
- **Hide**: Blog from navigation (keep files, just remove from nav)
- **Action**: Set `nav: false` in `_pages/blog.md`

### 2. Home Page Changes
- **Remove**: Selected publications section from home/about page
- **Action**: Set `selected_papers: false` in `_pages/about.md` (line 16)
- **Rationale**: Publications are accessible through dedicated Publications page; no need for duplication on home page

### 3. Publications Page Structure
Reorganize into two sections:

#### Section 1: Manuscripts (Preprints/Submitted)
Display papers that are not yet published in peer-reviewed venues.

**Papers to include:**
1. **PPD-CPP Paper**
   - Title: "PPD-CPP: Pointwise predictive density calibrated-power prior in dynamically borrowing historical information"
   - Authors: Shixuan Wang, Jing Zhang, Emily L. Kang, Bin Zhang
   - Date: September 30, 2025
   - arXiv: 2509.25688
   - DOI: 10.48550/arXiv.2509.25688
   - URL: https://arxiv.org/abs/2509.25688

#### Section 2: Published Work (Peer-Reviewed Publications)
Display papers in reverse chronological order (latest first).

**Papers to include (in order):**

1. **JRSS-C Paper (November 2025)** - Most Recent
   - Title: "A multivariate spatial statistical model for statistical downscaling of sea surface temperature in the Great Barrier Reef region"
   - Authors: Ayesha Ekanayaka, Emily L Kang, Amy Braverman, Peter Kalmus
   - Journal: Journal of the Royal Statistical Society Series C: Applied Statistics
   - Volume: 74, Issue 4
   - Date: November 2025
   - Pages: 1183–1213
   - DOI: 10.1093/jrsssc/qlaf019
   - URL: https://academic.oup.com/jrsssc/advance-article-abstract/doi/10.1093/jrsssc/qlaf019/8096399

2. **Stat Paper (March 2025)** - Older
   - Title: "A Practical Tool for Visualizing and Measuring Model Selection Uncertainty"
   - Authors: Ren et al. (2025) [Need to verify if Emily L. Kang is co-author]
   - Journal: Stat (Wiley)
   - Date: March 12, 2025
   - DOI: 10.1002/sta4.70056
   - URL: https://onlinelibrary.wiley.com/doi/abs/10.1002/sta4.70056

### 4. Display Requirements

#### Remove:
- Preview images/figures
- Metrics badges (Altmetric, Dimensions, Google Scholar, Inspire HEP)
- Abstract previews
- BibTeX show button

#### Keep Only:
- Paper title
- Authors
- Journal/venue information
- Publication date
- Three buttons (left to right):
  1. **PDF** - Link to PDF version
  2. **HTML** - Link to HTML/online version
  3. **DOI** - Link to DOI

## Implementation Plan

### Step 1: Hide Blog from Navigation
**File to modify:** `emilystat.github.io/_pages/blog.md`

**Change:**
```yaml
nav: true  →  nav: false
```

### Step 2: Create BibTeX Entries
**File to modify:** `emilystat.github.io/_bibliography/papers.bib`

**Add entries for:**
1. PPD-CPP paper (arXiv) - category: manuscript
2. JRSS-C paper - category: published
3. Stat paper - category: published

**BibTeX format with custom fields:**
```bibtex
@article{key,
  title = {...},
  author = {...},
  journal = {...},
  year = {...},
  volume = {...},
  pages = {...},
  doi = {...},
  html = {...},
  pdf = {...},
  category = {published|manuscript},
  selected = {false}
}
```

### Step 3: Modify Publications Page
**File to modify:** `emilystat.github.io/_pages/publications.md`

**Approach:** Use jekyll-scholar filtering with custom category field

**New structure:**
```liquid
---
layout: page
permalink: /publications/
title: publications
description:
nav: true
nav_order: 2
---

## Manuscripts

{% bibliography --query @*[category=manuscript]* %}

## Published Work

{% bibliography --query @*[category=published]* %}
```

### Step 4: Customize Bibliography Display
**File to modify:** `emilystat.github.io/_layouts/bib.liquid`

**Changes needed:**
1. Remove/hide preview images
2. Remove metrics badges sections
3. Keep only PDF, HTML, DOI buttons
4. Ensure buttons appear in correct order (PDF, HTML, DOI from left to right)

**OR**

Create a new simplified layout `bib_simple.liquid` and use it specifically for the publications page.

### Step 5: Configure Button Display Order
Ensure the button display order in the bibliography layout follows:
1. PDF (if `pdf` field exists)
2. HTML (if `html` field exists)
3. DOI (always, from doi field)

### Step 6: Test Locally
1. Build site: `bundle exec jekyll serve`
2. Verify:
   - Blog not in navigation
   - Publications page has two sections
   - Papers appear in correct sections
   - Only PDF, HTML, DOI buttons show
   - No metrics/badges
   - Buttons in correct order

## Files to Create/Modify

### To Modify:
1. `emilystat.github.io/_pages/blog.md` - Hide from nav
2. `emilystat.github.io/_bibliography/papers.bib` - Add new entries
3. `emilystat.github.io/_pages/publications.md` - Reorganize structure
4. `emilystat.github.io/_layouts/bib.liquid` - Simplify display

### Optional to Create:
- `emilystat.github.io/_layouts/bib_simple.liquid` - Simplified bibliography layout (if needed)

## Data Needed

### For Wiley Paper (Stat 2025):
- Full author list (verify if Emily L. Kang is co-author)
- PDF link
- HTML link (already have: https://onlinelibrary.wiley.com/doi/abs/10.1002/sta4.70056)

### For arXiv Paper:
- PDF link: https://arxiv.org/pdf/2509.25688
- HTML link: https://arxiv.org/abs/2509.25688

### For JRSS-C Paper:
- PDF link (need to obtain)
- HTML link: https://academic.oup.com/jrsssc/advance-article-abstract/doi/10.1093/jrsssc/qlaf019/8096399

## Future Additions
Plan allows for easy addition of more papers by:
1. Adding BibTeX entry to `papers.bib` with appropriate category
2. Papers automatically appear in correct section
3. Sorted by year within each section

## Papers Added - Second Batch (2021-2024)

### Key Features:
- **Complete author lists** - No "et al." abbreviations; all authors fully listed
- **Consistent formatting** - All entries follow same BibTeX structure with standardized fields
- **Conditional buttons** - HTML and DOI buttons only appear when links are available
- **All papers categorized as "published"** - Appear in Published Work section in reverse chronological order

### 2024 (2 papers):

1. **Lee et al. - EcoPro: Ecological Projection Digital Twin**
   - Authors: Seungwon Lee, Peter Kalmus, Antonio Ferraz, Alex Goodman, Kyle Pearson, Gary Doran, Flynn Platt, Beichen Hu, Ayesha Ekanayaka, Sudip Chakraborty, Emily L. Kang, Jia Zhang, Sierra Dahiyat, Kyle Cavanaugh (14 authors)
   - Conference: IGARSS 2024 - 2024 IEEE International Geoscience and Remote Sensing Symposium
   - Pages: 2311-2314
   - Location: Athens, Greece
   - DOI: 10.1109/IGARSS53475.2024.10640554
   - HTML: https://ieeexplore.ieee.org/document/10640554
   - Entry type: @inproceedings

2. **Cheng et al. - Recursive nearest neighbor co-kriging models**
   - Authors: Si Cheng, Bledar A. Konomi, Georgios Karagiannis, Emily L. Kang
   - Journal: Environmetrics, 35(4), e2844
   - DOI: 10.1002/env.2844
   - HTML: https://onlinelibrary.wiley.com/doi/abs/10.1002/env.2844
   - Entry type: @article

### 2023 (1 paper):

3. **Konomi et al. - Bayesian Latent Variable Co-kriging Model**
   - Authors: Bledar A. Konomi, Emily L. Kang, Abeer Almomani, Jonathan Hobbs, Claire Gilroy, Sudipto Ghosh
   - Journal: Journal of Agricultural, Biological and Environmental Statistics, 28, 423-441
   - DOI: 10.1007/s13253-023-00530-9
   - HTML: https://link.springer.com/article/10.1007/s13253-023-00530-9
   - Entry type: @article

### 2022 (3 papers):

4. **Yang et al. - Traffic restrictions during the 2008 Olympic Games**
   - Authors: Bo Yang, Hongxing Liu, Emily L. Kang, Song Shu, Min Xu, Bin Wu, Richard A. Beck, Kenneth M. Hinkel, Bailang Yu
   - Journal: Communications Earth & Environment, 3, 105
   - DOI: 10.1038/s43247-022-00427-4
   - HTML: https://www.nature.com/articles/s43247-022-00427-4
   - Entry type: @article

5. **Kalmus et al. - Past the precipice? Projected coral habitability**
   - Authors: Peter Kalmus, Ayesha Ekanayaka, Emily L. Kang, Mark Baird, Michelle Gierach
   - Journal: Earth's Future, 10, e2021EF002608
   - DOI: 10.1029/2021EF002608
   - HTML: https://agupubs.onlinelibrary.wiley.com/doi/abs/10.1029/2021EF002608
   - Entry type: @article

6. **Ekanayaka et al. - Statistical Downscaling of Sea Surface Temperature**
   - Authors: Ayesha Ekanayaka, Peter Kalmus, Emily L. Kang, Amy Braverman
   - Conference: Workshop on Gaussian Processes, Spatiotemporal Modeling, and Decision-Making Systems (GPSMDMS) at NeurIPS 2022
   - Note: No DOI or HTML link available (workshop paper)
   - Entry type: @inproceedings

### 2021 (5 papers):

7. **Cheng et al. - Hierarchical Bayesian nearest neighbor co-kriging**
   - Authors: Si Cheng, Bledar A. Konomi, Jessica L. Matthews, Georgios Karagiannis, Emily L. Kang
   - Journal: Spatial Statistics, 44, 100516
   - DOI: 10.1016/j.spasta.2021.100516
   - HTML: https://www.sciencedirect.com/science/article/abs/pii/S2211675321000270
   - Entry type: @article

8. **Ma et al. - Computer Model Emulation with High-Dimensional Functional Output**
   - Authors: Pulong Ma, Anirban Mondal, Bledar A. Konomi, Jonathan Hobbs, Joon Jin Song, Emily L. Kang
   - Journal: Technometrics, 64(1), 65-79
   - DOI: 10.1080/00401706.2021.1895890
   - HTML: https://www.tandfonline.com/doi/full/10.1080/00401706.2021.1895890
   - Entry type: @article

9. **Yang et al. - Spatio-temporal Cokriging method**
   - Authors: Bo Yang, Hongxing Liu, Emily L. Kang, Song Shu, Min Xu, Bin Wu, Richard A. Beck, Kenneth M. Hinkel, Bailang Yu
   - Journal: Remote Sensing of Environment, 255, 112190
   - DOI: 10.1016/j.rse.2020.112190
   - HTML: https://www.sciencedirect.com/science/article/abs/pii/S0034425720300122
   - Entry type: @article

10. **Kang et al. - Modeling Large Multivariate Spatial Data**
    - Authors: Emily L. Kang, Miaoqi Li, Kerry Cawse-Nicholson, Amy Braverman
    - Journal: Journal of the Indian Statistical Association, 59(2)
    - Note: No HTML link available
    - Entry type: @article

11. **Shu et al. - Improving Satellite Waveform Altimetry Measurements**
    - Authors: Song Shu, Hongxing Liu, Frédéric Frappart, Emily L. Kang, Bo Yang, Min Xu, Yan Huang, Bo Wu, Bailang Yu, Shuanggen Wang, Richard A. Beck, Kenneth M. Hinkel (12 authors)
    - Journal: IEEE Transactions on Geoscience and Remote Sensing, 59(6), 4733-4748
    - DOI: 10.1109/TGRS.2020.3010184
    - HTML: https://ieeexplore.ieee.org/document/9153046
    - Entry type: @article

### Implementation Notes:
- All papers added with `category={published}` field
- Full author names preserved (no "et al." or name truncation)
- LaTeX special character for Frédéric Frappart: `Fr\'ed\'eric`
- Conference papers use `@inproceedings` with `booktitle` field
- Journal papers use `@article` with `journal`, `volume`, `pages` fields
- HTML and DOI fields only included when valid links are available
- Papers automatically sorted by year (descending) within Published Work section

## Papers Added - Third Batch (2017-2020)

### Summary:
Added 17 papers from 2017-2020 covering:
- 2020: 6 papers (fused GP, ECOSTRESS, hexagonal grids, bus stop analysis, MCEN, MODIS/AMSR-E fusion)
- 2019: 6 papers (randomized ML, spatial downscaling, chlorophyll retrieval, hierarchical Bayesian SST, NNGP, additive GP)
- 2018: 2 papers (Mammoth Mountain ecosystem, MODIS snow products)
- 2017: 2 papers (spatial data fusion for non-Gaussian data, NNGP book chapter)

All entries include complete author lists, DOI and HTML links where available.

## Author Display Configuration

### Current Setting (Updated):
- **max_author_limit: 7** (changed from 3)
- Configuration file: `_config.yml` line 327
- Behavior:
  - Papers with ≤7 authors: All authors displayed
  - Papers with >7 authors: First 7 authors shown, then clickable "X more author(s)" link
  - Clicking the link expands to show all remaining authors with animation
  - Animation delay: 10ms (configurable via `more_authors_animation_delay`)

### Rationale:
- Shows complete author lists for most papers (majority have ≤7 authors)
- Maintains expandable feature for papers with very long author lists (e.g., 12-14 authors)
- Provides better visibility than previous limit of 3 authors
- Balances readability with completeness

### Implementation:
The expandable author feature is built into `_layouts/bib.liquid` (lines 62-96):
- Automatically calculates number of hidden authors
- Generates clickable span with "X more author(s)" text
- Handles singular/plural ("1 more author" vs "2 more authors")
- Shows all hidden authors on click with typewriter animation
- No code changes needed - controlled entirely by config value

## Papers Added - Fourth Batch (2009-2016)

### Summary:
Added 15 papers from 2009-2016 covering climate science, spatial statistics, remote sensing, and computational methods.

### Papers by Year:

#### 2016 (1 paper):
1. **Cressie & Kang - Climate-Change Projections**
   - Journal: Mathematical Geosciences, 48, 107-121
   - DOI: 10.1007/s11004-015-9607-9

#### 2015 (2 papers):
2. **Ren et al. - Metabolomics Data Analysis**
   - Authors: Ren, S., Hinzman, A. A., Kang, Emily L., Szczesniak, R. D., Lu, L. J.
   - Journal: Metabolomics, 11, 1492-1513
   - DOI: 10.1007/s11306-015-0823-6

3. **Zhu et al. - Satellite SST Fixed Rank Kriging**
   - Authors: Zhu, Yuxin, Kang, Emily L., Bo, Yanchen, Tang, Qingxin, Cheng, Jin, He, Yuxiang
   - Journal: IEEE Transactions on Geoscience and Remote Sensing, 53(9), 5021-5035
   - DOI: 10.1109/TGRS.2015.2416351

#### 2013 (3 papers):
4. **Kang & Cressie - NARCCAP Climate ANOVA**
   - Journal: International Journal of Applied Earth Observation and Geoinformation, 22, 3-15
   - DOI: 10.1016/j.jag.2011.12.007

5. **Ojiambo & Kang - Cucurbit Downy Mildew Spatial Frailties**
   - Journal: Phytopathology, 103(3), 216-227
   - DOI: 10.1094/PHYTO-07-12-0152-R

6. **Kang et al. - Regression Models for Turbulent Systems**
   - Authors: Kang, Emily L., Harlim, John, Majda, Andrew J.
   - Journal: Communications in Mathematical Sciences, 11(2), 481-498
   - DOI: 10.4310/cms.2013.v11.n2.a8

#### 2012 (3 papers):
7. **Kang & Harlim - Filtering Nonlinear Spatio-Temporal Chaos**
   - Journal: Physica D: Nonlinear Phenomena, 241(12), 1099-1113
   - DOI: 10.1016/j.physd.2012.03.003

8. **Kang et al. - NARCCAP Bayesian Hierarchical Model**
   - Authors: Kang, Emily L., Cressie, Noel, Sain, Stephan R.
   - Journal: Journal of the Royal Statistical Society Series C, 61(2), 291-313
   - DOI: 10.1111/j.1467-9876.2011.01010.x

9. **Kang & Harlim - Multiscale Filtering with HMM**
   - Journal: Monthly Weather Review, 140, 860-873
   - DOI: 10.1175/MWR-D-10-05067.1

#### 2011 (1 paper):
10. **Kang & Cressie - Spatial Random Effects Model**
    - Journal: Journal of the American Statistical Association, 106(495), 972-983
    - DOI: 10.1198/jasa.2011.tm09680

#### 2010 (3 papers):
11. **Kang et al. - Temporal Variability for Spatial Mapping**
    - Authors: Kang, Emily L., Cressie, Noel, Shi, Tao
    - Journal: Canadian Journal of Statistics, 38, 271-289
    - DOI: 10.1002/cjs.10063

12. **Cressie & Kang - Digital Soil Mapping (Book Chapter)**
    - Book: Proximal Soil Sensing (Progress in Soil Science series)
    - Publisher: Springer, Dordrecht
    - Pages: 49-63
    - DOI: 10.1007/978-90-481-8859-8_4

13. **Cressie et al. - Fixed Rank Filtering**
    - Authors: Cressie, Noel, Shi, Tao, Kang, Emily L.
    - Journal: Journal of Computational and Graphical Statistics, 19(3), 724-745
    - DOI: 10.1198/jcgs.2010.09051

#### 2009 (2 papers):
14. **Kang et al. - Small-Area Data Analysis**
    - Authors: Kang, Emily L., Liu, Desheng, Cressie, Noel
    - Journal: Computational Statistics & Data Analysis, 53(8), 3016-3032
    - DOI: 10.1016/j.csda.2008.07.033

15. **Morton et al. - Smoothing Splines**
    - Authors: Morton, R., Kang, Emily L., Henderson, B. L.
    - Journal: Environmetrics, 20, 249-259
    - DOI: 10.1002/env.925

### Key Features:
- **Consistent author name format**: "Emily L. Kang" throughout all entries
- **Complete author lists**: Full names for all co-authors (no "et al.")
- **Comprehensive bibliographic info**: Volumes, numbers, pages, publishers
- **DOI and HTML links**: Included for all papers
- **Entry types**: Includes @article, @inproceedings, and @incollection for book chapter
- **All categorized as published**: `category={published}` field for reverse chronological display

## Publications Page Display Update (Final)

### Change Made:
Removed separate "Manuscripts" and "Published Work" section headers from publications page. Now displays all publications in a single unified list in reverse chronological order.

### Modified File:
- `_pages/publications.md` - Changed from sectioned display to unified bibliography display

### Previous Structure (Removed):
```liquid
<h2>Manuscripts</h2>
{% bibliography --query @*[category=manuscript]* %}

<h2>Published Work</h2>
{% bibliography --query @*[category=published]* %}
```

### Current Structure:
```liquid
<div class="publications">
{% bibliography %}
</div>
```

### Rationale:
1. **Cleaner presentation** - Single unified list is simpler and more streamlined
2. **Strong publication record** - With 40+ publications from 2009-2025, published work dominates
3. **Reduced maintenance** - No need to recategorize when manuscript status changes
4. **Self-evident status** - Journal names, years, and DOIs indicate publication status
5. **Standard for senior faculty** - Common practice for established researchers to display all work together

### Result:
All publications now appear in reverse chronological order (newest first) without section divisions. The bibliography automatically sorts by year, showing the most recent work at the top.

## People Page Implementation (Completed)

### Objective:
Created a clean people page (`_pages/people.md`, permalink: `/people/`) showcasing:
1. Current students (graduate and undergraduate)
2. Past students with first positions and current positions (if known)
3. Collaborators acknowledgment section

### Implementation Details:

#### File Created:
- Location: `_pages/people.md`
- Layout: `page` (simple markdown)
- Permalink: `/people/`
- Navigation: `nav: true`, `nav_order: 6`

#### Structure Implemented:

**Section 1: Current Students**
- **Graduate Students (Ph.D.)**:
  - Rick Lucas (2021-present, in candidacy)
  - Eric Herrison Gyamfi (2022-present, before candidacy)
  - Hancheng Li (Joint with B. A. Konomi, 2022-present, in candidacy)
  - Ying Zhang (2024-present, before candidacy)
- **Undergraduate Students**:
  - Linh Tran (Fall 2025)

**Section 2: Past Students**
- **Ph.D. Graduates** (table with 4 columns):
  - Name | Graduation Year | First Position | Current Position (if known)
  - 9 graduates from 2017-2024, including:
    - Most recent: Ayesha Kumari Ekanayaka Katugoda Gedara (2024) → Postdoctoral Fellow, UNC Chapel Hill
    - Notable: Pulong Ma (2018) → now TTAP at Iowa State University
- **Undergraduate Alumni**:
  - Ruoqi Song (2020) → Ph.D. program in Biostatistics, The Ohio State University

**Section 3: Collaborators**
- Acknowledgment paragraph expressing gratitude for collaborations
- No specific names listed (per user request to avoid incomplete list)
- Contact email for collaboration inquiries: kangel@ucmail.uc.edu

### Key Features:
- **Clean formatting**: Simple markdown lists and tables
- **Complete information**: All students from provided data properly organized
- **Proper categorization**: Current vs. past students clearly separated
- **Chronological order**: Past students listed from most recent to oldest
- **Typos fixed**: "interested" and "Biostatistics" corrected
- **LaTeX converted**: All `\item` and `\textit{}` formatting converted to markdown
- **Joint advisorship noted**: Students co-advised with B. A. Konomi clearly indicated

### Design Decisions:
- **Simple markdown approach**: Easy maintenance, direct editing
- **No specific collaborator names**: Avoids implicit ranking or incomplete representation
- **Updated contact email**: kangel@ucmail.uc.edu (not OSU email)
- **Four-column table for Ph.D. graduates**: Added "Current Position" column to track career progression

### People Page Navigation Update (Final)

**Date:** November 22, 2025

**Objective:** Move people page from top-level navigation to Research dropdown menu only.

**Changes Made:**
- File: `_pages/people.md`
- Changed: `nav: true` → `nav: false`
- Removed: `nav_order: 6`

**Result:**
- People page NO LONGER appears in top-level navigation bar
- People page ONLY appears in Research dropdown menu
- Research dropdown already configured in `_pages/dropdown.md` with people as child item

**Current Top Navigation (nav_order):**
1. About (home)
2. Publications
3. CV
4. Teaching
5. Research (dropdown menu)
   - people
   - projects
   - repositories

**Rationale:**
- Cleaner top navigation with fewer items
- Logically groups people, projects, and repositories under Research category
- Maintains accessibility while improving navigation organization

**Implementation:**
The Research dropdown is configured in `_pages/dropdown.md`:
```yaml
---
layout: page
title: research
nav: true
nav_order: 5
dropdown: true
children:
  - title: people
    permalink: /people/
  - title: projects
    permalink: /projects/
  - title: repositories
    permalink: /repositories/
---
```

**Deployment:**
- Committed: "Hide people page from top navigation, show only in Research dropdown"
- Pushed to GitHub: Commit 40962b32
- GitHub Actions deployment: Successful (Build: 1m21s, Deploy: 9s)
- Live at: https://emilystat.github.io/

## Projects Page Implementation (Completed)

**Date:** November 23, 2025

### Objective:
Create a projects landing page that can list multiple grants, with individual project pages for each grant. First project added: NSF DMS-2053668.

### Files Created:

#### 1. Projects Landing Page
- **File:** `_pages/projects.md`
- **Permalink:** `/projects/`
- **Layout:** `page`
- **Navigation:** `nav: false` (accessible only via Research dropdown)
- **Purpose:** Lists all current and past research projects/grants with brief summaries and links to individual project pages

#### 2. Individual Project Page
- **File:** `_pages/nsf-dms-2053668.md`
- **Permalink:** `/projects/nsf-dms-2053668/`
- **Layout:** `page`
- **Navigation:** `nav: false`
- **Grant:** NSF DMS-2053668 (2021-2025)
- **Title:** "Inference and Uncertainty Quantification for High Dimensional Systems in Remote Sensing: Methods, Computation, and Applications"

#### 3. Offline Content Draft File
- **File:** `/Users/Emily_1/claude-emily/emily-website/nsf-dms-2053668-content-draft.md`
- **Purpose:** Template for organizing project content offline before adding to website
- **Location:** Outside git repository for user's offline editing

### Project Page Structure:

**Section 1: Overview**
- 3-paragraph description of the project (from NSF abstract)
- Grant Information box with:
  - Funding Agency: National Science Foundation
  - Program: Division of Mathematical Sciences
  - Award Number with link to NSF award page
  - Period: 2021-2025
  - PI: Emily L. Kang
  - Type: Collaborative Research

**Section 2: Publications**
- Placeholder for publications from this grant
- Will use same bibliography display as publications page (HTML/DOI buttons)
- Can reference papers from `_bibliography/papers.bib` by citation key

**Section 3: Software**
- Placeholder for GitHub repositories and software tools
- Ready to add open-source software developed under this grant

**Section 4: Course Materials**
- Placeholder for course information and PDF materials
- Will include syllabus PDFs and other course materials supported by grant

**Section 5: Presentations**
- Placeholder for presentation details and PDF links
- Format: Title - Venue, Location, Date with context and PDF slides

**Section 6: Team**
- PI: Emily L. Kang, University of Cincinnati
- Collaborators: To be added
- Students Supported: To be added

### Navigation Integration:

**Modified File:** `_pages/dropdown.md`

**Change:** Added projects to Research dropdown menu

**Current Research Dropdown Structure:**
```yaml
children:
  - title: people
    permalink: /people/
  - title: projects
    permalink: /projects/
  - title: repositories
    permalink: /repositories/
```

**Navigation Hierarchy:**
- Research (dropdown, nav_order: 5)
  - people
  - projects ← NEW
  - repositories

### Design Features:

1. **Scalable Structure:**
   - Landing page can accommodate multiple projects
   - Each project gets its own dedicated page
   - Consistent structure across all project pages

2. **Content Flexibility:**
   - Sections use HTML comments as placeholders
   - Easy to add content incrementally
   - Compatible with existing bibliography system

3. **Future-Ready:**
   - Template established for adding more grants
   - Offline draft file provides structured content collection
   - Consistent with overall website design (al-folio theme)

### Content Status:

**Completed:**
- ✅ Overview section with full NSF abstract summary
- ✅ Grant information box with NSF award link
- ✅ All section placeholders created

**To Be Added (via offline draft file):**
- Publications list (will reference bibliography entries)
- GitHub repositories/software tools
- Course materials (PDFs to upload to `assets/pdf/`)
- Presentation details (PDFs to upload to `assets/pdf/`)
- Team members (collaborators and students supported)

### Deployment:
- **Committed:** "Add projects page with NSF DMS-2053668 grant"
- **Commit:** 66a784f8
- **Pushed to GitHub:** November 23, 2025
- **Live URL:** https://emilystat.github.io/projects/

### Next Steps:
1. User fills in offline draft file with project content
2. Upload any PDF files to `assets/pdf/` directory
3. Copy organized content from draft file to `_pages/nsf-dms-2053668.md`
4. Add future grants by creating new project pages and adding entries to landing page

## Repository Configuration Update (Completed)

**Date:** November 23, 2025

### Objective:
Fix repository name in repositories configuration and ensure input-dimension-reduction displays on repositories page.

### Issue Identified:
Repository name was using underscores instead of hyphens:
- **Incorrect:** `emilystat/input_dimension_reduction`
- **Correct:** `emilystat/input-dimension-reduction`

### Changes Made:
- **File:** `_data/repositories.yml`
- **Line 8:** Changed `input_dimension_reduction` → `input-dimension-reduction`

### Result:
The input-dimension-reduction repository now displays correctly on the repositories page at https://emilystat.github.io/repositories/

### Current Repository List:
1. MFGP
2. JointSurvivalDurationModeling
3. Downscaling-demo
4. AutoBasisFDA
5. input-dimension-reduction

---

## NSF Project Page Overview Update (Completed)

**Date:** November 23, 2025

### Objective:
Condense the NSF DMS-2053668 project overview to be concise instead of copying lengthy text from the NSF webpage.

### Previous State:
- 3 lengthy paragraphs (approximately 220 words)
- Appeared to be copied directly from NSF abstract
- Repetitive content across paragraphs

### Changes Made:
- **File:** `_pages/nsf-dms-2053668.md`
- **Section:** Overview (lines 9-15)
- **Result:** Condensed from 3 paragraphs to 2 concise sentences

### New Overview Text:
```markdown
This project develops statistical methods and computational tools for inverse problems in high-dimensional systems, with applications to remote sensing and carbon monitoring. The research focuses on building scalable statistical emulators using joint dimension reduction for input and output spaces, enabling efficient uncertainty quantification for remote sensing data products.
```

### Rationale:
1. **More concise** - 2 sentences instead of 3 paragraphs
2. **Clearer focus** - Highlights key objectives and methods
3. **Better user experience** - Readers get essential information quickly
4. **Professional presentation** - Original content, not copy-pasted from NSF

### Deployment:
- **Committed:** "Fix repository name and condense NSF project overview"
- **Commit:** 88b67ee7
- **Pushed to GitHub:** November 23, 2025
- **Live URL:** https://emilystat.github.io/projects/nsf-dms-2053668/

---

## NSF Project Page Team Section Update (Completed)

**Date:** November 23, 2025

### Objective:
Complete the Team section on the NSF DMS-2053668 project page with full collaborator and student information, including affiliations and webpage links.

### Changes Made:

**File Modified:** `_pages/nsf-dms-2053668.md`

**Section Updated:** Team (lines 86-104)

### Team Section Structure:

#### Principal Investigator:
- Emily L. Kang, Associate Professor, Division of Statistics and Data Science, University of Cincinnati
- Link to UC Division of Statistics and Data Science

#### Collaborators (6 listed):
1. **Bledar A. Konomi**
   - Associate Professor, Division of Statistics and Data Science, University of Cincinnati
   - Link: https://researchdirectory.uc.edu/p/konomibr

2. **Amy Braverman**
   - Principal Statistician, Jet Propulsion Laboratory, California Institute of Technology
   - Link: https://dus.jpl.nasa.gov/home/braverman/

3. **Jonathan Hobbs**
   - Data Scientist, Jet Propulsion Laboratory, California Institute of Technology
   - Link: https://dus.jpl.nasa.gov/home/hobbs/

4. **Peter Kalmus**
   - Data Scientist, Jet Propulsion Laboratory, NASA
   - Link: https://higgs.jpl.nasa.gov/people/pkalmus/

5. **Georgios Karagiannis**
   - Associate Professor, Department of Mathematical Sciences, Durham University, UK
   - Link: https://www.durham.ac.uk/staff/georgios-karagiannis/

6. **Kerry Cawse-Nicholson**
   - Scientist, Jet Propulsion Laboratory, NASA
   - Link: https://science.jpl.nasa.gov/people/kcawseni/

#### Students Supported (4 listed):
1. **Ayesha Kumari Ekanayaka Katugoda Gedara**
   - Ph.D. 2024, University of Cincinnati
   - Current Position: Postdoctoral Fellow, UNC Chapel Hill
   - Co-author on project publications (ekanayaka2025multivariate, ekanayaka2022statistical)

2. **Rick Lucas**
   - Ph.D. student (2021-present), University of Cincinnati
   - Started when grant began (2021)

3. **Eric Herrison Gyamfi**
   - Ph.D. student (2022-present), University of Cincinnati

4. **Hancheng Li**
   - Ph.D. student (2022-present, Joint with B. A. Konomi), University of Cincinnati

### Research Sources:
Collaborator information gathered from:
- Amy Braverman: JPL webpage, Google Scholar (Fellow of ASA, Principal Statistician, UQ expert)
- Jon Hobbs: JPL webpage, Iowa State University news (Lew Allen Award winner, data scientist)
- Bledar Konomi: UC Research Directory (Associate Professor, co-advisor)
- Peter Kalmus: JPL/NASA webpage (Data scientist, climate science focus)
- Georgios Karagiannis: Durham University (Associate Professor, Bayesian statistics)
- Kerry Cawse-Nicholson: JPL Science webpage (ECOSTRESS mission lead, remote sensing)

### Key Features:
- **Complete affiliations** - Full titles and institutional affiliations for all collaborators
- **Active webpage links** - All collaborators linked to their professional pages
- **International collaboration** - Collaborators from UC (USA), JPL (USA), Durham (UK)
- **Students tracked** - All students supported during grant period (2021-2025)
- **Career progression** - Current position noted for graduated students

### Deployment:
- **Committed:** "Add complete Team section to NSF project page"
- **Commit:** 01e69f52
- **Pushed to GitHub:** November 23, 2025
- **Deployment Status:** Successful (Build + Deploy completed)
- **Live URL:** https://emilystat.github.io/projects/nsf-dms-2053668/

### Impact:
The NSF project page now provides complete information about the research team, making it easier for:
- Potential collaborators to understand the team structure
- Students to identify research group members
- Funding agencies to see the collaborative nature of the work
- Website visitors to learn about the breadth of expertise involved

---

## NSF Project Page Team Structure Update (Completed)

**Date:** November 23, 2025

### Objective:
Update the Team section on NSF DMS-2053668 project page to reflect correct PI structure, titles, and student roster.

### Changes Made:

**File Modified:** `_pages/nsf-dms-2053668.md`

**Section Updated:** Team (lines 86-108)

### Team Structure Changes:

#### 1. Principal Investigators (Updated to Plural)
**Previous:**
- Section title: "Principal Investigator" (singular)
- Listed: Emily L. Kang only

**Current:**
- Section title: "Principal Investigators" (plural)
- Listed:
  1. **Emily L. Kang**, Professor (updated from Associate Professor), Division of Statistics and Data Science, University of Cincinnati
  2. **Guang Lin** (NEW), Professor, Department of Mathematics, Purdue University
     - Link: https://www.math.purdue.edu/people/bio/lin491

#### 2. Co-Principal Investigator Section (NEW)
- **Bledar A. Konomi** (moved from Collaborators)
  - Associate Professor, Division of Statistics and Data Science, University of Cincinnati
  - Link: https://researchdirectory.uc.edu/p/konomibr

#### 3. Collaborators Section
Remains the same with:
- Amy Braverman (JPL)
- Jonathan Hobbs (JPL)
- Peter Kalmus (JPL/NASA)
- Georgios Karagiannis (Durham University, UK)
- Kerry Cawse-Nicholson (JPL/NASA)

#### 4. Students Supported (Updated)
**Added two new students:**
- **Ying Zhang**, Ph.D. student (2024-present), University of Cincinnati
- **Lloyd Goldstein**, Ph.D. student (2024-present), University of Cincinnati

**Complete student roster (6 total):**
1. Ayesha Kumari Ekanayaka Katugoda Gedara - Ph.D. 2024 (graduated, now postdoc at UNC Chapel Hill)
2. Rick Lucas - Ph.D. student (2021-present)
3. Eric Herrison Gyamfi - Ph.D. student (2022-present)
4. Hancheng Li - Ph.D. student (2022-present, Joint with B. A. Konomi)
5. Ying Zhang - Ph.D. student (2024-present) ← NEW
6. Lloyd Goldstein - Ph.D. student (2024-present) ← NEW

### Research Sources:
- **Guang Lin information** gathered via WebSearch:
  - Position: Associate Dean for Research & Innovation, Professor
  - Departments: Mathematics and Mechanical Engineering (with Statistics affiliation)
  - Institution: Purdue University West Lafayette
  - Profile URL: https://www.math.purdue.edu/people/bio/lin491

### Key Updates Summary:
1. ✅ Changed "Principal Investigator" → "Principal Investigators" (plural)
2. ✅ Updated Emily L. Kang's title: Associate Professor → Professor
3. ✅ Added Guang Lin as co-PI from Purdue University
4. ✅ Created new "Co-Principal Investigator" section
5. ✅ Moved Bledar A. Konomi to Co-PI section (from Collaborators)
6. ✅ Added Ying Zhang and Lloyd Goldstein to Students Supported list

### Deployment:
- **Committed:** "Update NSF project page: add PI Guang Lin, update team structure"
- **Commit:** 165b8a79
- **Pushed to GitHub:** November 23, 2025
- **Deployment Status:** Successful (Build + Deploy completed)
- **Live URL:** https://emilystat.github.io/projects/nsf-dms-2053668/

### Impact:
The NSF project page now accurately reflects:
- The collaborative multi-institutional nature of the project (UC and Purdue)
- Correct faculty titles and roles (PI vs Co-PI)
- Current student roster including newest members (2024 cohort)
- Professional recognition of title promotion (Professor)

---

## Notes
- Keep existing blog files intact, just hidden from navigation
- Maintain jekyll-scholar plugin functionality
- Use existing theme capabilities (no custom plugins needed)
- Papers can have `selected={true}` field for potential featured display elsewhere
- Venue abbreviations can still use `_data/venues.yml` for color coding if desired
- To show all authors always (no limit), set `max_author_limit:` to blank in `_config.yml`
- Category field in BibTeX entries (manuscript/published) no longer affects display but can be retained for reference
