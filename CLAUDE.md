# CLAUDE.md - AI Assistant Guide for Vendor Analysis Assessment

## Project Overview

This repository contains a vendor analysis and classification system that processes vendor data from Excel spreadsheets and categorizes vendors by department. The project is designed to streamline vendor management by automatically classifying vendors into appropriate departments based on business type and services provided.

### Purpose
- Analyze vendor data from Excel files
- Classify vendors into departments: Legal, Finance, Marketing, Engineering, Support, and G&A (General & Administrative)
- Generate structured reports for vendor categorization

## Repository Structure

```
Vendor-Analysis-Assessment/
├── Vendor Analysis Assessment - Deeba.xlsx  # Source data file
├── read_excel.py                            # Excel data reader utility
├── classify_vendors.py                      # Main classification script
└── CLAUDE.md                                # This file
```

### File Descriptions

#### `Vendor Analysis Assessment - Deeba.xlsx`
- **Type**: Excel workbook (xlsx format)
- **Purpose**: Contains vendor data to be analyzed
- **Location**: `/home/user/Vendor-Analysis-Assessment/Vendor Analysis Assessment - Deeba.xlsx`
- **Structure**: First row contains headers, subsequent rows contain vendor information
- **Column Layout**:
  - Column A (index 0): Vendor Name
  - Column B (index 1): Department
  - Column C (index 2): Last 12 months Cost (USD)
  - Column D (index 3): 1-line Description on what the Vendor does
  - Column E (index 4): **Suggestions (Consolidate / Terminate / Optimize costs)** - This is where classifications should be written

#### `read_excel.py`
- **Purpose**: Simple utility script to read and display Excel data
- **Usage**: Iterates through all rows in the Excel file and prints them
- **Dependencies**: openpyxl
- **Execution**: `python3 read_excel.py`

#### `classify_vendors.py`
- **Purpose**: Main classification engine for vendor categorization
- **Functionality**:
  - Loads vendor data from Excel file (reads vendor names from Column A)
  - Applies keyword-based classification rules
  - Categorizes vendors into departments
  - Outputs results as markdown table to console
- **Current Behavior**: Reads from Column A, prints to console
- **Expected Behavior**: Should write classifications to Column E (Suggestions column) in the Excel file
- **Output Format**: Markdown table with columns: Vendor Name, Department
- **Execution**: `python3 classify_vendors.py`

## Classification System

### Department Categories

The classification system uses the following departments with priority order (checked top to bottom):

1. **Legal** - Law firms, solicitors, notaries
   - Keywords: law, legal, solicitor, odvjetnicko, notary, pinsent masons, kilgannon & partners

2. **Finance** - Insurance, accounting, tax services
   - Keywords: insurance, osiguranje, bdo, rsm, grant thornton, pwc, chartered accountant, finance, sage, planful, tax, cigna, bupa, etc.

3. **Marketing** - Marketing tools, CRM, analytics
   - Keywords: salesforce, linkedin, hubspot, cognism, uberflip, google ireland, semrush, lusha, cision

4. **Engineering** - Cloud services, IT, software development
   - Keywords: aws, cloud, microsoft, github, gitlab, adobe, jetbrains, atlassian, docusign, smartsheet

5. **Support** - Customer support and help desk
   - Keywords: peakon, zendesk, freshdesk, intercom, support

6. **G&A (General & Administrative)** - HR, travel, office, facilities, recruiting
   - Keywords: navan, office, recruitment, hotel, travel, telecom, parking, gym, catering
   - **Default Category**: If no other category matches

### Classification Logic

- **Case-insensitive matching**: All vendor names are converted to lowercase for comparison
- **Priority-based**: Categories are checked in order; first match wins
- **Keyword-based**: Uses Python's `any()` with substring matching
- **Comprehensive keywords**: Each category has extensive keyword lists to ensure accurate classification

## Development Workflow

### Git Branching Strategy

1. **Branch Naming Convention**: `claude/<description>-<session-id>`
   - Example: `claude/classify-vendors-by-dept-01TPyvdY64sPE5W73hm1gsR4`
   - All branches MUST start with `claude/` prefix
   - Session ID should match the current Claude session

2. **Development Process**:
   ```bash
   # Create and switch to feature branch
   git checkout -b claude/<feature-name>-<session-id>

   # Make changes and commit
   git add .
   git commit -m "Descriptive commit message"

   # Push to remote
   git push -u origin claude/<feature-name>-<session-id>
   ```

3. **Pull Request Workflow**:
   - Create PR from feature branch to main branch
   - PRs are merged via GitHub pull requests
   - Clean merge history preferred

### Commit Message Guidelines

- Use clear, descriptive commit messages
- Focus on the "why" rather than the "what"
- Examples:
  - "Add vendor classification script"
  - "Update classification rules for finance department"
  - "Fix keyword matching for legal vendors"

## Key Conventions for AI Assistants

### When Working with This Repository

1. **Excel File Handling**:
   - Always use openpyxl library for Excel operations
   - Excel file path: `/home/user/Vendor-Analysis-Assessment/Vendor Analysis Assessment - Deeba.xlsx`
   - First row (row 1) is header; data starts at row 2
   - Column A (index 0): Vendor names - READ from here
   - Column E (index 4): Suggestions - WRITE classifications here
   - When writing to Excel, always save the workbook after modifications

2. **Classification Updates**:
   - When adding new classification rules, maintain priority order
   - Add specific keywords before generic ones (e.g., "pinsent masons" before "llp")
   - Keep keywords in lowercase for consistency
   - Test new rules don't conflict with existing categories

3. **Code Style**:
   - Use Python 3 shebang: `#!/usr/bin/env python3`
   - Follow PEP 8 style guidelines
   - Use descriptive variable names
   - Add inline comments for complex classification logic

4. **Testing Changes**:
   - Run the script to verify classification accuracy
   - Check output format (markdown table)
   - Verify all vendors are categorized appropriately

5. **File Paths**:
   - Use absolute paths in scripts: `/home/user/Vendor-Analysis-Assessment/`
   - Maintain consistency across all scripts

### Common Tasks

#### Adding a New Department Category

1. Update `classify_vendors.py`
2. Add new classification function block in priority order
3. Add comprehensive keyword list
4. Test with existing vendors
5. Document keywords in this file

#### Adding Keywords to Existing Categories

```python
# Add to the appropriate keyword list in classify_vendor() function
if any(keyword in vendor_lower for keyword in [
    'existing_keyword', 'new_keyword', 'another_keyword'
]):
    return 'Department'
```

#### Running Classification

```bash
cd /home/user/Vendor-Analysis-Assessment
python3 classify_vendors.py
```

#### Viewing Excel Data

```bash
cd /home/user/Vendor-Analysis-Assessment
python3 read_excel.py
```

#### Writing Classifications to Excel File

To write classifications back to Column E (Suggestions column):

```python
from openpyxl import load_workbook

# Load workbook
wb = load_workbook('/home/user/Vendor-Analysis-Assessment/Vendor Analysis Assessment - Deeba.xlsx')
ws = wb.active

# Write to column E (index 5 when using cell notation: A=1, B=2, C=3, D=4, E=5)
for row_num in range(2, ws.max_row + 1):  # Start from row 2 (skip header)
    vendor_name = ws.cell(row=row_num, column=1).value  # Column A
    if vendor_name:
        classification = classify_vendor(vendor_name)
        ws.cell(row=row_num, column=5).value = classification  # Column E

# Save the workbook
wb.save('/home/user/Vendor-Analysis-Assessment/Vendor Analysis Assessment - Deeba.xlsx')
```

## Dependencies

### Python Libraries

- **openpyxl**: Excel file reading and manipulation
  ```bash
  pip install openpyxl
  ```

### System Requirements

- Python 3.x
- Linux environment (current: Linux 4.4.0)
- Git for version control

## Data Flow

```
Excel File (Vendor Analysis Assessment - Deeba.xlsx)
    ↓
openpyxl.load_workbook()
    ↓
Extract vendor names (Column A, starting row 2)
    ↓
For each vendor:
    classify_vendor(vendor_name)
        ↓
    Keyword matching (case-insensitive)
        ↓
    Return department category
    ↓
[Current] Generate markdown table output → Print to console
[Desired] Write classification to Column E (Suggestions) → Save workbook
```

## Important Notes for AI Assistants

1. **Preserve Classification Order**: The order of classification checks matters. Legal and Finance are checked before generic patterns to avoid false positives.

2. **Keyword Conflicts**: Be aware of potential keyword conflicts:
   - "llp" appears in both legal and finance contexts
   - Cloud tools might be used by both Engineering and Marketing
   - Prioritization is key

3. **G&A as Default**: G&A serves as the catch-all category for vendors that don't fit other categories, especially facilities and general services.

4. **Case Sensitivity**: All comparisons are case-insensitive. Always use `.lower()` when adding new keywords.

5. **Excel File Structure**: Column A contains vendor names (source data). Column E is the "Suggestions" column where classifications should be written.

6. **Output Format**: Maintain markdown table format for outputs to ensure compatibility with documentation and reporting tools.

7. **Branch Naming**: Always verify branch names start with `claude/` and include session ID before pushing to remote.

## Troubleshooting

### Common Issues

1. **ModuleNotFoundError: No module named 'openpyxl'**
   - Solution: `pip install openpyxl`

2. **FileNotFoundError: Excel file not found**
   - Verify file path: `/home/user/Vendor-Analysis-Assessment/Vendor Analysis Assessment - Deeba.xlsx`
   - Check file exists with: `ls -la /home/user/Vendor-Analysis-Assessment/`

3. **Git push fails with 403**
   - Verify branch name starts with `claude/`
   - Ensure branch name includes correct session ID

4. **Incorrect classification**
   - Check keyword list for the expected department
   - Verify priority order (earlier categories take precedence)
   - Add more specific keywords if needed

## Future Enhancements

Potential improvements for AI assistants to consider:

1. **Machine Learning Classification**: Replace keyword-based system with ML model
2. **Multi-criteria Classification**: Use multiple columns from Excel (not just vendor name)
3. **Confidence Scores**: Add classification confidence levels
4. **Export Options**: Support CSV, JSON, or database output
5. **Configuration File**: Externalize classification rules to YAML/JSON
6. **Vendor Analytics**: Add spending analysis, vendor count by department, etc.
7. **Duplicate Detection**: Identify and merge duplicate vendors
8. **Interactive Mode**: Allow manual classification overrides

## Version History

- **v1.0** (2025-11-29): Initial vendor classification system
  - Basic Excel reading functionality
  - Keyword-based classification for 6 departments
  - Markdown table output

## Contact & Support

This is a portfolio project by Deeba Naveed. For questions or issues, refer to the GitHub repository issues page.

---

**Last Updated**: 2025-11-29
**AI Assistant**: Claude (Anthropic)
**Purpose**: Guide for AI assistants working with this codebase
