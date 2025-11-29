# CLAUDE.md - AI Assistant Guide

## Repository Overview

**Vendor-Analysis-Assessment** is a Python-based vendor classification system that analyzes vendor data from Excel spreadsheets and categorizes vendors into organizational departments based on business type and naming patterns.

**Repository**: `deebanaveedportfolio/Vendor-Analysis-Assessment`

## Codebase Structure

```
Vendor-Analysis-Assessment/
├── Vendor Analysis Assessment - Deeba.xlsx    # Source data file
├── classify_vendors.py                        # Main classification script
├── read_excel.py                              # Excel data reader utility
└── CLAUDE.md                                  # This file
```

### Key Files

#### 1. `classify_vendors.py` (Main Script)
**Purpose**: Classifies vendors from the Excel file into departments

**Key Components**:
- **Line 5**: Hardcoded file path to Excel workbook
- **Lines 15-77**: `classify_vendor()` function with classification rules
- **Lines 79-89**: Main execution logic and table output

**Department Categories** (in order of precedence):
1. **Legal** (Lines 19-21): Law firms, solicitors, notaries
2. **Finance** (Lines 24-31): Insurance, accounting, tax services
3. **Marketing** (Lines 34-38): CRM, sales tools, advertising platforms
4. **Engineering** (Lines 41-50): Cloud services, dev tools, IT solutions
5. **Support** (Lines 53-56): Customer support platforms
6. **G&A** (Lines 59-74): HR, travel, facilities, recruitment, general admin (default)

**Output Format**: Markdown table with columns `Vendor Name | Department`

#### 2. `read_excel.py` (Utility Script)
**Purpose**: Simple Excel file reader for debugging/exploration
- Reads and prints all rows from the active worksheet
- Uses same Excel file path as main script

### Dependencies

```python
openpyxl  # Excel file manipulation
```

**Installation**: `pip install openpyxl`

## Development Workflows

### Git Branch Conventions

This repository uses **Claude-specific branch naming**:

```
claude/<description>-<session-id>
```

**Examples**:
- `claude/classify-vendors-by-dept-01TPyvdY64sPE5W73hm1gsR4`
- `claude/claude-md-mijzgxbdk1pd5u4j-01AkgQfeiuH757P5hJYGG7Jj`

**Critical Rules**:
1. ✅ All development MUST occur on Claude-prefixed branches
2. ✅ Branch names MUST start with `claude/`
3. ✅ Session ID MUST match the current Claude session
4. ❌ NEVER push to branches without `claude/` prefix
5. ❌ Pushing to incorrectly named branches will fail with 403 errors

### Git Push Protocol

Always use:
```bash
git push -u origin <branch-name>
```

**Retry Logic for Network Failures**:
- Retry up to 4 times with exponential backoff: 2s, 4s, 8s, 16s
- Only retry on network errors, not authentication failures

### Standard Development Flow

1. **Read Before Modify**: Always read files before editing
2. **Branch Check**: Verify you're on the correct Claude branch
3. **Make Changes**: Implement requested features/fixes
4. **Commit**: Use descriptive commit messages
5. **Push**: Push to the Claude-prefixed branch with `-u` flag

## Key Conventions for AI Assistants

### File Path Handling

**Critical**: All file paths in Python scripts are **absolute paths**:
```python
'/home/user/Vendor-Analysis-Assessment/Vendor Analysis Assessment - Deeba.xlsx'
```

**When modifying code**:
- ✅ Preserve absolute paths
- ✅ Maintain the exact spelling/spacing of filenames
- ❌ Do NOT convert to relative paths
- ❌ Do NOT assume file locations

### Excel Data Structure

**Expected Format**:
- **Row 1**: Header row (skipped during processing)
- **Column 0**: Vendor name (primary data field)
- Additional columns may exist but are not currently used

**When working with the Excel file**:
- Use `openpyxl.load_workbook()` for reading
- Access active sheet via `wb.active`
- Iterate from row 2 onwards (skip header)
- Check for null/empty vendor names before processing

### Classification Logic

**Order Matters**: Classification rules are checked sequentially with early returns:
1. Check Legal first (before LLP check)
2. Check Finance next (before generic LLP)
3. Check Marketing (before Engineering to avoid tool overlap)
4. Check Engineering
5. Check Support
6. Default to G&A for everything else

**When adding new classification rules**:
- Add keywords in lowercase (input is converted via `.lower()`)
- Place specific checks BEFORE general checks
- Consider keyword overlap between departments
- Add comments explaining ambiguous cases
- Maintain alphabetical order within keyword lists for readability

**Pattern Matching**:
```python
if any(keyword in vendor_lower for keyword in [list]):
    return 'Department'
```

### Code Style Preferences

1. **Shebang**: Include `#!/usr/bin/env python3` at file start
2. **Comments**: Use inline comments for non-obvious business logic
3. **Variable Names**: Use snake_case (e.g., `vendor_lower`, `classified_vendors`)
4. **String Literals**: Single quotes for keywords, double quotes for output
5. **Iterations**: Prefer `values_only=True` for performance when structure not needed

### Output Conventions

**Table Format**:
```
| Vendor Name | Department |
|-------------|------------|
| Example Co  | Finance    |
```

- Use pipe-delimited markdown tables
- Include header separator row
- Align columns for readability

### Testing & Validation

**Before Committing**:
1. Verify the script runs without errors: `python3 classify_vendors.py`
2. Check output table formatting
3. Validate all vendors are classified (no None values)
4. Ensure Excel file path is accessible

**Manual Testing**:
```bash
python3 classify_vendors.py
python3 read_excel.py  # For data exploration
```

### Common Tasks

#### Adding a New Vendor Classification Rule

1. Read the current `classify_vendors.py`
2. Identify the correct department category
3. Add keyword(s) to the appropriate list (maintain alphabetical order)
4. Consider precedence - place specific checks before general ones
5. Test with the actual Excel file
6. Commit with message: "Add [vendor type] classification to [Department]"

#### Modifying Department Categories

1. Check all existing classification logic for dependencies
2. Update the `classify_vendor()` function
3. Update department list in this documentation
4. Test comprehensively with full dataset
5. Document the change in commit message

#### Debugging Classification Issues

1. Run `python3 read_excel.py` to inspect raw data
2. Check vendor name spelling/formatting
3. Verify keywords are lowercase in classification rules
4. Check precedence order (earlier checks win)
5. Add debug prints: `print(f"Classifying: {vendor_lower}")`

### Performance Considerations

- **Excel Loading**: File is loaded once at script start
- **Iteration**: Single pass through vendor list
- **String Operations**: Case-insensitive matching via `.lower()`
- **Scalability**: Current approach suitable for <10,000 vendors

**For Large Datasets** (future optimization):
- Consider caching compiled regex patterns
- Use pandas for Excel manipulation
- Implement parallel processing for classification

### Security & Data Privacy

**Sensitive Data**:
- Excel file may contain vendor contracts/financial data
- Do NOT commit Excel files to public repositories
- Use `.gitignore` if repository becomes public

**File Paths**:
- Hardcoded paths are acceptable for single-user scripts
- Consider environment variables for multi-user deployments

## Error Handling

**Current State**: Minimal error handling

**When Adding Error Handling**:
- Validate Excel file exists before loading
- Check for empty worksheets
- Handle missing columns gracefully
- Provide meaningful error messages
- Consider logging for production use

**Example**:
```python
import os
if not os.path.exists(filepath):
    raise FileNotFoundError(f"Excel file not found: {filepath}")
```

## Future Enhancement Considerations

**Potential Improvements**:
1. Configuration file for department keywords (YAML/JSON)
2. Command-line arguments for Excel file path
3. Multiple output formats (CSV, JSON, Excel)
4. Confidence scores for classifications
5. Machine learning for ambiguous cases
6. Web interface for non-technical users
7. Batch processing for multiple files
8. Classification override mechanism

**When implementing enhancements**:
- Maintain backward compatibility
- Update this documentation
- Add tests for new features
- Consider configuration over hardcoding

## Documentation Updates

**When to Update This File**:
- New Python scripts added
- Classification rules significantly changed
- New dependencies required
- Workflow processes modified
- New department categories added

**Maintenance Schedule**: Update after major features or monthly reviews

---

## Quick Reference

### Run Classification
```bash
python3 classify_vendors.py
```

### Install Dependencies
```bash
pip install openpyxl
```

### Git Workflow
```bash
git checkout -b claude/feature-name-<session-id>
# Make changes
git add .
git commit -m "Descriptive message"
git push -u origin claude/feature-name-<session-id>
```

### File Locations
- **Excel Data**: `/home/user/Vendor-Analysis-Assessment/Vendor Analysis Assessment - Deeba.xlsx`
- **Scripts**: Root directory
- **Git Config**: `.git/config`

---

**Last Updated**: 2025-11-29
**Maintained By**: AI Assistant (Claude)
**Repository Owner**: deebanaveedportfolio
