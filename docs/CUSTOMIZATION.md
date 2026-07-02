# Customization Guide

## Overview

This guide explains how to customize the Banking Performance Dashboard for your specific data, branding, and requirements.

## Table of Contents

1. [Data Customization](#data-customization)
2. [Color & Branding](#color--branding)
3. [Layout Changes](#layout-changes)
4. [Adding/Removing Charts](#addingremoving-charts)
5. [Advanced Customization](#advanced-customization)

---

## Data Customization

### Updating Chart Data

All chart data is hardcoded in the JavaScript section at the bottom of `index.html`. Find this pattern:

```javascript
makeBar('chartId', ['label1', 'label2', ...], [value1, value2, ...]);
```

### Example 1: Update Credit Utilization Chart

**Current**:
```javascript
makeBar('utilChart', 
  ['0-20%','20-40%','40-60%','60-80%','80-100%'], 
  [10.0, 22.2, 42.8, 66.7, 100.0]
);
```

**With Your Data**:
```javascript
// Your new default rates by credit utilization
makeBar('utilChart', 
  ['0-20%','20-40%','40-60%','60-80%','80-100%'], 
  [9.5, 18.3, 35.2, 58.1, 92.7]  // Your values here
);
```

### Example 2: Update KPI Values

Find the KPI row section in the HTML:

```html
<div class="kpi">
  <div class="kpi-label">Total Applications</div>
  <div class="kpi-value">5,000</div>  <!-- Change this -->
  <div class="kpi-sub">Full sample portfolio</div>
</div>
```

Update to your values:

```html
<div class="kpi">
  <div class="kpi-label">Total Applications</div>
  <div class="kpi-value">12,450</div>  <!-- Your total -->
  <div class="kpi-sub">Full sample portfolio</div>
</div>
```

### Example 3: Update Default Rate KPI

```html
<div class="kpi warn">
  <div class="kpi-label">Overall Default Rate</div>
  <div class="kpi-value">22.9%</div>  <!-- Change this -->
  <div class="kpi-sub">1,147 of 5,000 defaulted</div>  <!-- And this -->
</div>
```

Update to:

```html
<div class="kpi warn">
  <div class="kpi-label">Overall Default Rate</div>
  <div class="kpi-value">18.5%</div>  <!-- Your rate -->
  <div class="kpi-sub">2,303 of 12,450 defaulted</div>  <!-- Your numbers -->
</div>
```

### Bulk Data Update Process

1. **Export your data** from your database/CSV
2. **Calculate aggregations** (group by each dimension)
3. **Calculate default rates** for each group
4. **Copy-paste** the arrays into the `makeBar()` calls

**Python Example**:

```python
import pandas as pd

# Load your data
df = pd.read_csv('your_data.csv')

# Calculate default rate by credit utilization
utilization_bins = [0, 20, 40, 60, 80, 100]
df['util_bin'] = pd.cut(df['credit_utilization'], bins=utilization_bins)
default_rates = df.groupby('util_bin')['default'].mean() * 100

# Output for JavaScript
print("[", ", ".join(f"{rate:.1f}" for rate in default_rates), "]")
# Output: [10.0, 22.2, 42.8, 66.7, 100.0]
```

---

## Color & Branding

### Changing the Color Scheme

All colors are defined as CSS variables in the `<style>` section:

```css
:root{
  --ink:#132A3A;              /* Main text color */
  --ink-soft:#3B5468;         /* Secondary text */
  --bg:#F3F5F7;              /* Page background */
  --panel:#FFFFFF;           /* Card background */
  --line:#E1E6EA;            /* Border/line color */
  --low:#2F9E62;             /* Low risk - green */
  --mid:#E8A93B;             /* Moderate risk - amber */
  --high:#D6603D;            /* Elevated risk - orange */
  --severe:#B23A3A;          /* Severe risk - red */
  --accent:#1F6F5C;          /* Accent/highlight color */
}
```

### Example: Change Brand Colors to Blue Theme

```css
:root{
  --ink:#0B2E4D;              /* Dark blue */
  --ink-soft:#2E5090;         /* Softer blue */
  --bg:#EFF3F8;              /* Light blue background */
  --panel:#FFFFFF;           /* Keep white panels */
  --line:#D0E0F0;            /* Light blue border */
  --low:#0066CC;             /* Low risk - blue */
  --mid:#FF9900;             /* Moderate risk - amber (unchanged) */
  --high:#FF6600;            /* Elevated risk - orange */
  --severe:#CC0000;          /* Severe risk - red */
  --accent:#0052A3;          /* Dark blue accent */
}
```

### Updating the Logo/Brand Text

Locate the header section:

```html
<div class="brand-eyebrow">AI Credit Risk Decision Engine</div>
<h1>Credit Risk Portfolio Overview</h1>
```

Change to your brand:

```html
<div class="brand-eyebrow">YOUR BANK NAME</div>
<h1>Your Dashboard Title</h1>
```

### Changing Font

Update the font imports and CSS variables:

```css
/* Original */
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=Inter:wght@400;500;600;700&display=swap');

:root{
  --font-display: 'Space Grotesk', 'Segoe UI', sans-serif;
  --font-body: 'Inter', 'Segoe UI', sans-serif;
}

/* Change to Poppins + Open Sans */
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@600;700&family=Open+Sans:wght@400;500;600&display=swap');

:root{
  --font-display: 'Poppins', 'Segoe UI', sans-serif;
  --font-body: 'Open Sans', 'Segoe UI', sans-serif;
}
```

---

## Layout Changes

### Changing Grid Columns

**KPI Row** (currently 5 columns):

```css
/* Current: 5 columns */
.kpi-row {
  grid-template-columns: repeat(5, 1fr);
}

/* Change to 4 columns */
.kpi-row {
  grid-template-columns: repeat(4, 1fr);
}

/* Change to 3 columns */
.kpi-row {
  grid-template-columns: repeat(3, 1fr);
}
```

### Adjusting Chart Heights

```css
/* Tall charts (credit utilization, delinquencies) */
.chart-box.tall {
  height: 260px;  /* Increase for more space */
}

/* Standard charts */
.chart-box {
  height: 230px;  /* Decrease for compact view */
}
```

### Changing Panel Width Ratio

**Primary grid** (currently 1.3fr 1fr = 56% / 44%):

```css
/* Current: slightly favor left panel */
.grid {
  grid-template-columns: 1.3fr 1fr;
}

/* Make equal width */
.grid {
  grid-template-columns: 1fr 1fr;
}

/* Favor right panel */
.grid {
  grid-template-columns: 1fr 1.3fr;
}
```

### Adjusting Spacing/Padding

```css
/* Body padding (outer margins) */
body {
  padding: 28px;  /* Increase for more breathing room */
}

/* Panel padding */
.panel {
  padding: 16px 18px 10px;  /* Increase for more internal space */
}

/* Gap between grid items */
.grid {
  gap: 14px;  /* Increase for more separation */
}
```

---

## Adding/Removing Charts

### Adding a New Chart

1. **Add HTML placeholder** in the appropriate grid:

```html
<!-- Add to grid-3 section -->
<div class="panel">
  <div class="panel-title">Default Rate by New Metric</div>
  <div class="panel-note">Optional description here</div>
  <div class="chart-box"><canvas id="newMetricChart"></canvas></div>
</div>
```

2. **Add JavaScript** call in the script section:

```javascript
// Add after existing makeBar() calls
makeBar('newMetricChart', 
  ['Category A', 'Category B', 'Category C'], 
  [12.5, 18.3, 25.7]
);
```

### Removing a Chart

1. **Delete the HTML panel** from the grid
2. **Delete the corresponding JavaScript** `makeBar()` call

**Example**: Remove the Age chart:

```html
<!-- DELETE THIS ENTIRE SECTION -->
<div class="panel">
  <div class="panel-title">Default Rate by Applicant Age</div>
  <div class="chart-box"><canvas id="ageChart"></canvas></div>
</div>
```

```javascript
// DELETE THIS LINE
makeBar('ageChart', ['18-25','26-35','36-45','46-55','56-65','65+'], [44.0, 28.6, 21.7, 14.0, 9.6, 10.8]);
```

### Changing Chart Type

Current charts are all horizontal/vertical bar charts. To change to other types:

```javascript
// Current: bar chart
new Chart(document.getElementById(id), {
  type: 'bar',  // Change this
  // ...
});

// Options:
// - 'bar' (vertical bars)
// - 'line' (line chart)
// - 'doughnut' (donut/pie)
// - 'radar' (radar chart)
// - 'scatter' (scatter plot)
```

**Example: Line Chart Instead of Bar**:

```javascript
function makeLine(id, labels, data) {
  new Chart(document.getElementById(id), {
    type: 'line',  // Changed from 'bar'
    data: {
      labels: labels,
      datasets: [{
        data: data,
        borderColor: '#0052A3',
        backgroundColor: 'rgba(0,82,163,0.1)',
        tension: 0.4,
        fill: true
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: { display: false }
      }
    }
  });
}

// Usage
makeLine('utilChart', ['0-20%','20-40%','40-60%','60-80%','80-100%'], [10.0, 22.2, 42.8, 66.7, 100.0]);
```

---

## Advanced Customization

### CSV Data Import

Add file input and CSV parsing:

```html
<!-- Add to header section -->
<input type="file" id="csvFile" accept=".csv" />
<button onclick="loadCSV()">Load Data</button>

<script>
function loadCSV() {
  const file = document.getElementById('csvFile').files[0];
  const reader = new FileReader();
  
  reader.onload = function(e) {
    const csv = e.target.result;
    const lines = csv.split('\n');
    
    // Parse CSV and extract aggregations
    const data = parseData(lines);
    
    // Update charts with new data
    updateCharts(data);
  };
  
  reader.readAsText(file);
}
</script>
```

### Real-time Data via API

```javascript
// Fetch data from your backend
async function loadFromAPI() {
  const response = await fetch('/api/credit-risk-data');
  const data = await response.json();
  
  // Update KPIs
  document.querySelector('.kpi-value').textContent = data.totalApplications;
  
  // Update charts
  makeBar('utilChart', data.utilLabels, data.utilRates);
  makeBar('delinqChart', data.delinqLabels, data.delinqRates);
  
  // etc.
}

// Call on page load
window.addEventListener('load', loadFromAPI);
```

### Dark Mode Theme

Add a toggle and conditional CSS:

```javascript
// Toggle dark mode
function toggleDarkMode() {
  document.documentElement.classList.toggle('dark-mode');
  localStorage.setItem('theme', document.documentElement.classList.contains('dark-mode') ? 'dark' : 'light');
}

// Restore saved preference
if (localStorage.getItem('theme') === 'dark') {
  document.documentElement.classList.add('dark-mode');
}
```

```css
/* Dark mode CSS variables */
.dark-mode {
  --ink: #F3F5F7;
  --ink-soft: #D0D5DB;
  --bg: #0B1117;
  --panel: #161B22;
  --line: #30363D;
  --accent: #58A6FF;
}
```

### Export to PDF

Use a library like jsPDF + html2canvas:

```html
<!-- Add button -->
<button onclick="exportPDF()">Export as PDF</button>

<script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
<script>
function exportPDF() {
  const element = document.querySelector('.wrap');
  const opt = {
    margin: 10,
    filename: 'credit-risk-dashboard.pdf',
    image: { type: 'jpeg', quality: 0.98 },
    html2canvas: { scale: 2 },
    jsPDF: { orientation: 'landscape' }
  };
  html2pdf().set(opt).save().toPdf().get('pdf').save();
}
</script>
```

### Mobile Responsiveness Improvements

```css
/* Enhanced mobile breakpoints */
@media (max-width: 768px) {
  .kpi-row {
    grid-template-columns: 1fr;
  }
  
  .grid, .grid-3 {
    grid-template-columns: 1fr;
  }
  
  header {
    flex-direction: column;
  }
  
  .chart-box {
    height: 250px;
  }
}

@media (max-width: 480px) {
  body {
    padding: 12px;
  }
  
  h1 {
    font-size: 18px;
  }
  
  .kpi-value {
    font-size: 20px;
  }
}
```

---

## Testing Your Changes

1. **Save** your edits to `index.html`
2. **Refresh** the browser (Ctrl+Shift+R for hard refresh)
3. **Check** that:
   - Data displays correctly
   - Colors render properly
   - Charts render without errors
   - Layout is responsive
4. **Open DevTools** (F12) and check Console for errors

## Troubleshooting

### Chart Not Rendering
- **Check**: Console for JavaScript errors (F12 > Console)
- **Check**: Chart canvas ID matches the `makeBar()` call
- **Check**: Data array has same length as labels array

### Colors Not Updating
- **Check**: CSS variable names are correct (case-sensitive)
- **Check**: Hard refresh browser (Ctrl+Shift+R)
- **Check**: No conflicting inline styles

### Layout Breaking
- **Check**: Grid column values are valid (positive numbers)
- **Check**: No unmatched closing tags
- **Check**: Browser DevTools > Inspector for layout issues

---

## Best Practices

1. **Keep a backup** of the original `index.html`
2. **Test in multiple browsers** before deploying
3. **Validate your data** before copying into the dashboard
4. **Use consistent number formatting** (e.g., always 1 decimal place)
5. **Document your changes** in comments
6. **Use version control** (Git) to track modifications

---

**Last Updated**: July 2, 2024