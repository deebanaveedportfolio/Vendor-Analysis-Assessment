# Vendor Analysis Assessment – Deeba

## Overview
This Claude Code project contains the work performed for the Vendor Spend Strategy Assessment. All vendor analysis, classification, and recommendations were completed using a combination of Claude Code prompts, rules-based logic, and manual validation.

The master spreadsheet `Vendor Analysis Assessment - Deeba.xlsx` is included separately (hosted on GitHub/Drive) and contains the full vendor list, departmental assignments, and strategic recommendations.

---

## Methodology

**1. Data Preparation:**
- Uploaded vendor spend spreadsheet to Claude Code (for reference).
- Standardized vendor names and removed duplicates.
- Created a master table including vendor names, departments, and spend.

**2. Vendor Classification & Strategic Recommendations:**
- Developed rules-based criteria to classify vendors into departments (Engineering, Marketing, G&A, Finance, Support).
- Generated strategic recommendations for each vendor (Terminate, Consolidate, Optimize) using Claude Code prompts.
- Manual validation was performed on a sample (~10-15%) to ensure accuracy and correct assignments.

**3. Top Opportunities Identification:**
- Analyzed vendor spend and recommendations to identify three highest-impact cost-saving opportunities:
  - Consolidation of overlapping SaaS tools (CRM, Marketing, IT)
  - Elimination of non-critical travel, office, and hospitality vendors
  - Optimization of redundant IT/cloud infrastructure services
- Estimated annual savings calculated by aggregating vendor spend for each opportunity.

**4. Tools & Prompts Used:**
- **Claude Code:** For AI-assisted vendor classification and recommendation generation.
- **Excel/Spreadsheet Functions:** For spend aggregation, validation, and final formatting.
- Iterative prompt refinement ensured consistency and minimized errors.

**5. Quality Control & Validation:**
- Sampled vendors manually to confirm department and recommendation accuracy.
- Cross-checked recommendations against business context and spend magnitude.
- Reviewed final outputs for clarity before creating executive memo.

---

## Example Prompts Used in Claude Code

**Prompt for Department Classification:**
```
"Analyze this vendor list and classify each vendor into one of these departments:
Engineering, Marketing, G&A, Finance, Legal, or Support. Use vendor name patterns,
common business types, and industry knowledge. Output as a markdown table."
```

**Prompt for Strategic Recommendations:**
```
"For each vendor, provide a strategic recommendation (Terminate, Consolidate, or Optimize)
based on business criticality, potential redundancy, and cost optimization opportunities.
Consider industry best practices for SaaS, infrastructure, and operational vendor management."
```

**Prompt for Top Opportunities Analysis:**
```
"Identify the top 3 cost-saving opportunities by analyzing vendor spend patterns,
overlapping tool functionality, and non-critical services. Calculate estimated
annual savings for each opportunity."
```

---

## Top 3 Opportunities (Summary Table)

| Opportunity | Explanation | Estimated Annual Savings (USD) |
|-------------|-------------|-------------------------------|
| CRM & Marketing Tool Consolidation | Consolidate overlapping SaaS platforms (Salesforce, Hubspot, Cognism, LinkedIn) to reduce redundancy and license costs | $150,000 |
| Non-Critical Travel & Hospitality Vendors | Terminate hotels, restaurants, catering, events, and office move vendors that are not mission-critical | $120,000 |
| Redundant IT & Cloud Infrastructure | Optimize duplicate cloud services, SaaS subscriptions, and IT consulting platforms (AWS, Azure, Workato, Zapier, Smartsheet) | $100,000 |

**Total Estimated Annual Savings: $370,000**

---

## Executive Memo

**Audience:** CEO & CFO
**Summary:**
After a thorough vendor spend analysis, we identified key opportunities to reduce costs and streamline operations without impacting mission-critical services. Our recommendations focus on consolidating overlapping SaaS tools, terminating non-essential travel/office/hospitality vendors, and optimizing IT/cloud infrastructure spend. Estimated annual savings across these initiatives total **$370,000**.

**Key Recommendations:**
1. Consolidate overlapping CRM & marketing platforms to reduce redundancy and subscription costs.
2. Terminate non-critical travel, hospitality, and office-related vendors.
3. Optimize redundant IT/cloud infrastructure services to reduce costs without affecting business operations.

**Next Steps:**
- Approve strategic recommendations and communicate with relevant department heads.
- Implement vendor consolidation and termination initiatives in phases.
- Monitor actual savings and report quarterly to finance leadership.

---

**Note:** The master Excel file with detailed vendor assignments and recommendations is available in this repository: `Vendor Analysis Assessment - Deeba.xlsx`

---

## Repository Contents

### Files
- **`Vendor Analysis Assessment - Deeba.xlsx`** - Master spreadsheet with full vendor analysis, classifications, and recommendations
- **`classify_vendors.py`** - Python script for automated vendor classification by department
- **`read_excel.py`** - Utility script for reading and exploring Excel data
- **`README.md`** - This file (project overview and methodology)
- **`CLAUDE.md`** - Technical documentation for AI assistants working on this codebase

### Scripts

#### `classify_vendors.py`
Automated vendor classification script that:
- Reads vendor data from Excel spreadsheet
- Applies rules-based logic to assign departments
- Outputs markdown table with vendor-to-department mappings
- Handles 6 department categories with keyword-based matching

**Usage:**
```bash
python3 classify_vendors.py
```

#### `read_excel.py`
Simple data exploration utility that:
- Loads the Excel workbook
- Prints all rows for manual inspection
- Useful for debugging and data validation

**Usage:**
```bash
python3 read_excel.py
```

---

## Department Classification Criteria

### Engineering
Cloud services (AWS, Azure), developer tools (GitHub, GitLab), IT infrastructure, software platforms, SaaS development tools

### Marketing
CRM systems (Salesforce, HubSpot), advertising platforms, marketing automation, social media tools, lead generation services

### G&A (General & Administrative)
HR services, recruiting, travel management, office facilities, catering, telecommunications, general business services

### Finance
Accounting firms, insurance providers, tax services, financial consultants, payroll services

### Legal
Law firms, legal services, notaries, compliance services

### Support
Customer support platforms (Zendesk, Intercom), help desk software, customer success tools

---

## Key Deliverables

1. **Vendor Classification Table** - All vendors assigned to appropriate departments
2. **Strategic Recommendations** - Each vendor tagged with Terminate/Consolidate/Optimize
3. **Top 3 Cost-Saving Opportunities** - Detailed analysis with estimated annual savings
4. **Executive Summary Memo** - Business-facing document summarizing findings and recommendations

---

## Technical Implementation

### Dependencies
```bash
pip install openpyxl
```

### Classification Logic
The `classify_vendors.py` script uses sequential pattern matching with precedence rules:
1. Legal keywords checked first (highest specificity)
2. Finance keywords (before generic patterns)
3. Marketing keywords (before tool overlap with Engineering)
4. Engineering keywords (broad tech/cloud/dev tools)
5. Support keywords (customer-facing platforms)
6. G&A keywords (default catch-all for general services)

This precedence ensures accurate classification when vendors could match multiple categories.

---

## Results Summary

**Total Vendors Analyzed:** [Varies based on dataset]
**Departments Covered:** 6 (Engineering, Marketing, G&A, Finance, Legal, Support)
**Classification Method:** Rules-based with manual validation
**Validation Sample Size:** 10-15% of total vendors
**Estimated Cost Savings Identified:** [Calculated from top 3 opportunities]

---

## How This Project Used Claude Code

1. **Initial Classification:** Claude Code analyzed vendor names and suggested department assignments
2. **Rules Development:** Iteratively refined keyword patterns based on Claude's business knowledge
3. **Script Generation:** Claude Code wrote the Python classification scripts
4. **Validation Support:** Claude Code helped identify edge cases and classification conflicts
5. **Documentation:** Claude Code generated this README and technical documentation

---

## Future Enhancements

- **Machine Learning Classification:** Train ML model on validated dataset for improved accuracy
- **Spend Threshold Rules:** Incorporate vendor spend amounts into recommendation logic
- **Contract Analysis:** Add contract term analysis for renewal optimization
- **Automated Reporting:** Generate executive summaries and dashboards programmatically
- **Multi-File Support:** Batch process multiple vendor lists for portfolio-level analysis

---

## Notes for Users

- The Excel file path in scripts is hardcoded - update if file location changes
- Classification rules are based on common business patterns - review outputs for your specific context
- Manual validation recommended for high-spend or critical vendors
- Strategic recommendations are framework suggestions - final decisions require business judgment

---

## Contact & Attribution

**Project:** Vendor Spend Strategy Assessment
**Analyst:** Deeba
**Tools:** Claude Code, Python, Excel
**Repository:** https://github.com/deebanaveedportfolio/Vendor-Analysis-Assessment

---

**Last Updated:** 2025-11-29
**Claude Code Version:** Sonnet 4.5
