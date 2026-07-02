# Banking Performance Dashboard

> An interactive AI-powered credit risk analysis dashboard for portfolio-level decision making and applicant-level default risk assessment.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![HTML5](https://img.shields.io/badge/HTML5-E34C26?style=flat&logo=html5&logoColor=white)](https://html5.org/)
[![Chart.js](https://img.shields.io/badge/Chart.js-FF6B6B?style=flat&logo=javascript&logoColor=white)](https://www.chartjs.org/)

## 📊 Overview

The Banking Performance Dashboard is a comprehensive web-based analytical tool designed to help financial institutions:

- **Analyze credit risk** across portfolios of 1,000+ loan applications
- **Identify key risk drivers** through interactive visualizations
- **Make data-driven decisions** with clear, actionable insights
- **Monitor portfolio composition** with real-time KPI metrics

### Key Features

✨ **9 Interactive Charts** analyzing default risk by:
- Credit utilization patterns
- Historical delinquencies
- Credit score bands
- Debt-to-income ratios
- Applicant age demographics
- Loan purpose categories
- Home ownership status
- Loan term structures

📈 **Executive Dashboard** with 5 critical KPIs:
- Total applications in portfolio
- Overall default rate
- Average credit score
- Average debt-to-income ratio
- Average loan amount

🎨 **Professional Design**:
- Enterprise-grade color scheme with risk-based severity indicators
- Responsive layout with consistent typography
- Clean, data-focused UI optimized for decision makers
- Accessibility-first approach

## 🚀 Quick Start

### Prerequisites
- Any modern web browser (Chrome, Firefox, Safari, Edge)
- No server or dependencies required

### Usage

1. **Clone the repository:**
   ```bash
   git clone https://github.com/vishnuprasad3004/banking-performance-dashboard.git
   cd banking-performance-dashboard
   ```

2. **Open the dashboard:**
   - Double-click `index.html` or
   - Open with your preferred web browser
   - Or use a local server:
     ```bash
     # Using Python
     python -m http.server 8000
     
     # Using Node.js
     npx http-server
     ```
   - Then navigate to `http://localhost:8000`

3. **Explore the data:**
   - Hover over charts for detailed tooltips
   - View severity-coded risk indicators
   - Monitor portfolio-level KPIs at the top

## 📁 Project Structure

```
banking-performance-dashboard/
├── index.html                    # Main dashboard file
├── README.md                     # Project documentation (this file)
├── LICENSE                       # MIT License
├── docs/
│   ├── ARCHITECTURE.md          # Technical architecture & design decisions
│   ├── DATA_DICTIONARY.md       # Field definitions & data sources
│   └── CUSTOMIZATION.md         # Guide for customizing the dashboard
├── assets/
│   └── (Future) images, fonts, logos
└── data/
    └── credit_risk_sample_data.csv  # Sample dataset (5,000 records)
```

## 📊 Dataset

**Sample Data: credit_risk_sample_data.csv**

| Metric | Value |
|--------|-------|
| Total Records | 5,000 loan applications |
| Default Rate | 22.9% (1,147 defaults) |
| Fields | 15 key attributes |
| Source | AI Credit Risk Decision Engine project |

### Key Fields
- **Applicant Demographics**: Age, home ownership status
- **Financial Profile**: Credit score, debt-to-income ratio, credit utilization
- **Loan Details**: Amount, purpose, term length
- **Risk Indicator**: Default status (0 = non-default, 1 = default)
- **Historical Data**: Count of delinquencies in last 2 years

## 🎨 Design & Color System

The dashboard uses a sophisticated color palette to communicate risk:

| Risk Level | Color | Default Rate Range |
|------------|-------|--------------------|
| **Low** | 🟢 Green (#2F9E62) | 0-25% |
| **Moderate** | 🟡 Amber (#E8A93B) | 25-50% |
| **Elevated** | 🟠 Orange (#D6603D) | 50-75% |
| **Severe** | 🔴 Red (#B23A3A) | 75%+ |

## 🔧 Technical Details

### Technology Stack
- **Frontend**: HTML5, CSS3, Vanilla JavaScript
- **Charting**: Chart.js 4.4.0 (CDN)
- **Typography**: Space Grotesk (display), Inter (body) via Google Fonts
- **Architecture**: Single-page application (SPA)
- **No Dependencies**: Pure client-side rendering

### Browser Compatibility
- Chrome/Chromium (90+)
- Firefox (88+)
- Safari (14+)
- Edge (90+)

## 🔄 Data Flow

```
CSV Data
   ↓
[Hardcoded in JavaScript] (demonstrating key insights)
   ↓
Chart.js Rendering
   ↓
Interactive Dashboard
```

**Note**: Current version uses hardcoded data for demonstration. See [CUSTOMIZATION.md](docs/CUSTOMIZATION.md) for integrating live data sources.

## 📈 Key Insights from Data

### Top Risk Drivers (in order of impact)

1. **Credit Utilization** - Most significant single driver
   - 0-20% utilization: 10% default rate
   - 80-100% utilization: 100% default rate

2. **Delinquency History** - Compounds risk sharply
   - 0 delinquencies: 5.2% default rate
   - 6+ delinquencies: 72% default rate

3. **Credit Score** - Inverse correlation with default risk
   - Poor (<580): 38.2% default rate
   - Very Good (740+): 0% default rate

4. **Age** - Younger applicants show higher risk
   - 18-25: 44% default rate
   - 56-65: 9.6% default rate

5. **Debt-to-Income Ratio** - Linear relationship with risk
   - 0-10%: 14.7% default rate
   - 50%+: 43.5% default rate

## 🛠️ Customization

### Modifying Charts

Edit the JavaScript section in `index.html` to change data:

```javascript
// Example: Update credit utilization data
makeBar('utilChart', 
  ['0-20%','20-40%','40-60%','60-80%','80-100%'], 
  [10.0, 22.2, 42.8, 66.7, 100.0]  // Change these values
);
```

### Changing Colors

Modify CSS variables in the `:root` selector:

```css
:root {
  --low: #2F9E62;        /* Low risk color */
  --mid: #E8A93B;        /* Moderate risk */
  --high: #D6603D;       /* Elevated risk */
  --severe: #B23A3A;     /* Severe risk */
}
```

For detailed customization guide, see [CUSTOMIZATION.md](docs/CUSTOMIZATION.md).

## 📚 Documentation

- **[ARCHITECTURE.md](docs/ARCHITECTURE.md)** - Technical architecture and design principles
- **[DATA_DICTIONARY.md](docs/DATA_DICTIONARY.md)** - Complete field definitions
- **[CUSTOMIZATION.md](docs/CUSTOMIZATION.md)** - How to customize for your data

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Make your changes with clear, descriptive commits
4. Test thoroughly in multiple browsers
5. Submit a Pull Request with a detailed description

### Contribution Ideas
- Additional chart types or visualizations
- Data import/export functionality
- PDF export capability
- Dark mode theme
- Mobile responsiveness improvements
- Internationalization (i18n) support
- Backend API integration examples

## 📋 Roadmap

- [ ] Add CSV/Excel data import functionality
- [ ] Implement PDF export feature
- [ ] Create dark mode theme
- [ ] Build REST API backend integration layer
- [ ] Add real-time data streaming
- [ ] Develop mobile app version
- [ ] Create data filter/drill-down capabilities
- [ ] Add multi-language support

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

Permissions:
- ✅ Commercial use
- ✅ Modification
- ✅ Distribution
- ✅ Private use

Conditions:
- ℹ️ License and copyright notice required

## 👤 Author

**Vishnu Prasad**
- GitHub: [@vishnuprasad3004](https://github.com/vishnuprasad3004)
- Email: [Add your contact info]

## 📞 Support

For questions, issues, or suggestions:

1. **GitHub Issues**: [Report a bug or request a feature](https://github.com/vishnuprasad3004/banking-performance-dashboard/issues)
2. **Discussions**: [Join community discussions](https://github.com/vishnuprasad3004/banking-performance-dashboard/discussions)
3. **Email**: [Contact directly]

## 🙏 Acknowledgments

- **Chart.js** - Excellent charting library
- **Google Fonts** - Typography (Space Grotesk, Inter)
- **Credit Risk Community** - Inspiration and best practices

## 📊 Analytics & Metrics

This dashboard demonstrates:
- **Data Visualization**: Best practices in financial dashboards
- **Risk Assessment**: AI-powered credit risk evaluation
- **UX/UI Design**: Professional analytics interface
- **Performance**: Fast, responsive, client-side rendering

---

<div align="center">

**[⬆ back to top](#banking-performance-dashboard)**

Made with ❤️ by [Vishnu Prasad](https://github.com/vishnuprasad3004)

</div>