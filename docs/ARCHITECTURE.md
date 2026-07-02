# Technical Architecture

## Overview

The Banking Performance Dashboard is a client-side single-page application (SPA) that requires no backend infrastructure. All rendering, charting, and interactivity occur directly in the browser.

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    User's Web Browser                        │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌────────────────────────────────────────────────────────┐  │
│  │              HTML Document (index.html)               │  │
│  │  - Semantic structure                                 │  │
│  │  - Accessibility markup                               │  │
│  │  - Meta tags for SEO                                  │  │
│  └────────────────────────────────────────────────────────┘  │
│                           │                                   │
│                           ▼                                   │
│  ┌────────────────────────────────────────────────────────┐  │
│  │           CSS Styling (embedded in <style>)           │  │
│  │  - CSS Variables for theming                          │  │
│  │  - Flexbox & Grid layouts                             │  │
│  │  - Responsive design patterns                         │  │
│  │  - Print-friendly styles                              │  │
│  └────────────────────────────────────────────────────────┘  │
│                           │                                   │
│                           ▼                                   │
│  ┌────────────────────────────────────────────────────────┐  │
│  │        JavaScript Engine (Vanilla JS)                 │  │
│  │  - Data aggregation                                   │  │
│  │  - Chart.js integration                               │  │
│  │  - Event handling                                     │  │
│  │  - Color calculations                                 │  │
│  └────────────────────────────────────────────────────────┘  │
│                           │                                   │
│                           ▼                                   │
│  ┌────────────────────────────────────────────────────────┐  │
│  │        Chart.js Library (v4.4.0 via CDN)             │  │
│  │  - 8 interactive bar charts                           │  │
│  │  - Tooltips and legends                               │  │
│  │  - Responsive rendering                               │  │
│  └────────────────────────────────────────────────────────┘  │
│                           │                                   │
│                           ▼                                   │
│  ┌────────────────────────────────────────────────────────┐  │
│  │            Canvas Rendering (Browser)                 │  │
│  │  - Interactive charts                                 │  │
│  │  - Hardware-accelerated graphics                      │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

## Technology Stack

### Frontend
- **HTML5**: Semantic markup, accessibility (WCAG 2.1)
- **CSS3**: Variables, Grid, Flexbox, Media queries
- **JavaScript (ES6+)**: Vanilla JS (no frameworks)
- **Chart.js 4.4.0**: Charting library via CDN

### Typography
- **Display**: Space Grotesk (Google Fonts)
- **Body**: Inter (Google Fonts)

### Browser APIs
- **Canvas API**: Chart.js rendering
- **CSS Custom Properties**: Dynamic theming
- **DOM API**: Element manipulation

## Component Structure

### 1. **Header Component**
```html
<header>
  - Brand eyebrow (accent text)
  - Main title (h1)
  - Subtitle description
  - Metadata panel (right-aligned)
</header>
```

**Purpose**: Establishes context and data source information

### 2. **KPI Row Component** (5 columns)
```html
<div class="kpi-row">
  - Total Applications
  - Overall Default Rate (highlighted with warning color)
  - Avg. Credit Score
  - Avg. Debt-to-Income
  - Avg. Loan Amount
</div>
```

**Purpose**: Executive summary of key metrics

### 3. **Chart Panels**

**Primary Grid (2 columns)**:
- Credit Utilization Chart (tall)
- Delinquencies Chart (tall)

**Secondary Grid (3 columns)**:
- Credit Score Band Chart
- Debt-to-Income Chart
- Age Demographic Chart

**Tertiary Grid (3 columns)**:
- Loan Purpose Chart (horizontal)
- Home Ownership Chart
- Loan Term Chart

### 4. **Legend Component**
```html
<div class="legend-strip">
  - Risk severity indicators
  - Color-coded (Low → Moderate → Elevated → Severe)
</div>
```

### 5. **Footer Component**
- Project attribution
- Methodology note

## Design Patterns

### Color System (CSS Variables)
```css
:root {
  --low: #2F9E62;        /* Green - 0-25% severity */
  --mid: #E8A93B;        /* Amber - 25-50% severity */
  --high: #D6603D;       /* Orange - 50-75% severity */
  --severe: #B23A3A;     /* Red - 75%+ severity */
  --accent: #1F6F5C;     /* Teal - accent/highlight */
}
```

### Risk Color Algorithm
```javascript
function riskColor(value, max) {
  const ratio = value / max;  // Normalize to 0-1
  
  if (ratio < 0.25) return --low;
  if (ratio < 0.50) return --mid;
  if (ratio < 0.75) return --high;
  return --severe;
}
```

### Layout System

**Responsive Grid Breakpoints**:
```
Desktop (1024px+):   5-col KPIs, 2-col grid, 3-col grid
Tablet (768-1024px):  3-col KPIs, 1-col grid
Mobile (<768px):      1-col KPIs, 1-col grid
```

## Data Flow

```
1. HTML Page Loads
   ↓
2. CSS Variables Initialized
   ↓
3. Chart.js Library Loaded (CDN)
   ↓
4. JavaScript Executes
   ├─ Defines riskColor() function
   ├─ Defines makeBar() function
   └─ Calls makeBar() 8 times with hardcoded data
   ↓
5. Chart.js Creates Canvas Elements
   ↓
6. Renders Interactive Charts
   ↓
7. User Interaction
   └─ Hover → Tooltip displays
```

## Performance Considerations

### Optimization Strategies
1. **Single HTTP Request**: Only Chart.js library loaded from CDN
2. **Minimal JavaScript**: ~300 lines, no dependencies
3. **CSS Variables**: Reduced CSS size, no duplicate values
4. **Hardware Acceleration**: Canvas rendering optimized by browser
5. **Lazy Loading**: No external images or resources

### Load Time Estimate
- **HTML Parse**: ~5ms
- **CSS Parse**: ~3ms
- **Chart.js CDN**: ~200-400ms (cached on repeat visits)
- **JavaScript Execution**: ~50ms
- **Chart Rendering**: ~100ms
- **Total**: ~400-500ms on first load

## Scalability

### Current Limitations
- **Data**: Hardcoded into JavaScript (8 datasets)
- **Records**: Supports portfolio sizes up to 100k+ (conceptual)
- **Charts**: 9 interactive charts (easily extensible)

### Future Enhancements
1. **CSV Import**: Read data from uploaded files
2. **API Integration**: Connect to live data source
3. **Data Caching**: LocalStorage for performance
4. **Aggregation**: Server-side data summarization
5. **Real-time Updates**: WebSocket integration

## Browser Support

| Browser | Version | Support |
|---------|---------|----------|
| Chrome | 90+ | ✅ Full |
| Firefox | 88+ | ✅ Full |
| Safari | 14+ | ✅ Full |
| Edge | 90+ | ✅ Full |
| IE 11 | N/A | ❌ Not supported |

## Accessibility (a11y)

### WCAG 2.1 Compliance
- **Color Contrast**: AA level (4.5:1 minimum)
- **Semantic HTML**: Proper heading hierarchy, landmarks
- **Keyboard Navigation**: All interactive elements accessible
- **Screen Readers**: Proper ARIA labels, alt text
- **Focus Indicators**: Visible focus states

### Accessibility Features
- Semantic `<header>`, `<footer>` landmarks
- Descriptive chart titles and notes
- Color + text for data representation
- High contrast text on background

## Security Considerations

### Current Security Posture
- **No external data sources**: Reduces injection risks
- **No user input**: No XSS vulnerabilities
- **Hardcoded data**: No database queries
- **Static files only**: Can be served from CDN

### Security Best Practices
1. **Content Security Policy** (recommended headers):
   ```
   script-src 'self' cdnjs.cloudflare.com fonts.googleapis.com
   style-src 'self' fonts.googleapis.com
   ```

2. **No sensitive data**: Dashboard contains only aggregated, anonymized metrics

3. **HTTPS**: Deploy only over HTTPS in production

## Deployment Architecture

### Static Hosting (Recommended)
```
GitHub Pages → index.html, LICENSE, docs/
Cloudflare CDN → Global distribution
```

### Traditional Server
```
Nginx/Apache → Serve static files
├─ index.html
├─ LICENSE
└─ /docs/
```

### Docker (Optional)
```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/
COPY docs/ /usr/share/nginx/html/docs/
EXPOSE 80
```

## Monitoring & Analytics

### Recommended Integrations
- **Google Analytics**: Traffic and user behavior
- **Sentry**: Error tracking
- **New Relic**: Performance monitoring

## Code Quality Standards

### Linting
- **ESLint**: JavaScript code quality
- **Stylelint**: CSS validation
- **HTMLHint**: HTML best practices

### Testing
- **Manual testing**: Cross-browser validation
- **Lighthouse**: Performance and accessibility audits
- **E2E Testing**: User interaction flows

## Future Architecture Improvements

1. **Component-based approach** (Web Components)
2. **State management** (for real-time data)
3. **Build pipeline** (webpack/rollup)
4. **Testing framework** (Jest/Cypress)
5. **TypeScript**: Type safety
6. **Progressive Web App** (PWA) capabilities

---

**Last Updated**: July 2, 2024
**Author**: Vishnu Prasad