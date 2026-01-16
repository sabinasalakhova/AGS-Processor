# Data Quality and Validation Guide

## Overview

The AGS Processor has been enhanced with comprehensive data quality tracking and validation to ensure data integrity throughout the processing pipeline. This guide explains the new features and how to interpret warnings and metrics.

## Key Features

### 1. No Silent Failures
All data processing operations now generate warnings when:
- Data is skipped or dropped
- Required columns are missing
- Depth intervals are invalid
- Files cannot be processed

### 2. Data Quality Warnings
The system tracks three levels of warnings:
- **ERROR**: Critical issues that prevent processing
- **WARNING**: Important issues that may affect results
- **INFO**: Informational messages about processing

### 3. Processing Metrics
After each operation, you'll see metrics including:
- Files processed vs. total files
- Boreholes processed vs. skipped
- Depth intervals created
- Groups found and used

## Warning Categories

### File Processing
- **FILE_PROCESSING_ERROR**: File couldn't be read or parsed
- **MISSING_GROUPS**: Expected data groups not found in file
- **MISSING_HOLE_DATA**: No borehole information available

### Data Validation
- **BOREHOLE_PROCESSING**: Issues with specific boreholes
- **MISSING_COLUMN**: Required columns not present
- **INSUFFICIENT_DEPTH_POINTS**: Less than 2 depth points (minimum required)
- **DEPTH_ORDERING_ERROR**: Depths not in correct order
- **ZERO_LENGTH_INTERVALS**: Intervals with no thickness

### Data Loss Tracking
- **DATA_LOSS_SUMMARY**: Summary of how much data was skipped
- **BOREHOLE_DATA_SOURCE**: Which groups contributed data for each borehole
- **EMPTY_GROUPS**: Groups with no data

## Using the Warning System

### In Streamlit Interface
When you process data, warnings appear in expandable sections:
```
⚠️ Errors (2)
  Click to see critical issues

⚡ Warnings (15)
  Click to see important notices

ℹ️ Information (42)
  Click to see processing details
```

### In Mock UI (HTML)
The status panel shows:
- **Metrics**: Visual cards with key statistics
- **Warnings Container**: Scrollable list of all issues found
  - Red badges for errors
  - Orange badges for warnings
  - Blue badges for information

### In Python Code
```python
from ags_core import concat_ags_files, combine_ags_data

# Process files
result_df, data_warnings = concat_ags_files(files, giu_number)

# Check for warnings
if data_warnings.has_warnings():
    warnings = data_warnings.get_warnings()
    for w in warnings:
        print(f"[{w['severity']}] {w['category']}: {w['message']}")

# View metrics
metrics = data_warnings.get_metrics()
print(f"Processed {metrics['files_processed']} files")
```

## Common Warning Scenarios

### Scenario 1: Borehole with Insufficient Data
```
WARNING: Borehole 12345_BH1: Insufficient depth points (1 found, minimum 2 required) - SKIPPED
```
**Cause**: Borehole has only one depth measurement  
**Impact**: Borehole excluded from combined data  
**Action**: Check source data for missing depth information

### Scenario 2: Missing Required Columns
```
WARNING: Group CORE: Missing column 'CORE_TOP' for borehole 12345_BH2
```
**Cause**: Expected column not in source data  
**Impact**: Data from this group won't be included  
**Action**: Verify AGS file format and column naming

### Scenario 3: Negative Depth Interval
```
WARNING: Borehole 12345_BH3: Depth ordering error at 5.0 > 3.0
```
**Cause**: DEPTH_TO is less than DEPTH_FROM  
**Impact**: May indicate data entry error  
**Action**: Review and correct depth values in source

### Scenario 4: Zero-Length Intervals
```
WARNING: Borehole 12345_BH4: Found 3 zero or negative length intervals
```
**Cause**: Adjacent depths are identical  
**Impact**: Intervals with no thickness  
**Action**: Review depth logging precision

## Best Practices

### 1. Always Review Warnings
- Check the warning summary after each operation
- Investigate ERROR-level warnings immediately
- Review WARNING-level issues for data quality

### 2. Monitor Metrics
- Compare processed vs. total counts
- Check skipped borehole percentages
- Verify expected groups are found

### 3. Validate Source Data
- Ensure AGS files are properly formatted
- Check depth values are sequential
- Verify required columns exist

### 4. Document Issues
- Note recurring warnings in your workflow
- Track problematic boreholes or files
- Maintain a log of data quality issues

## Validation Checks Performed

### AGS File Loading
- ✅ File format validation
- ✅ Encoding detection
- ✅ Group extraction
- ✅ Column name handling

### Data Concatenation
- ✅ File count verification
- ✅ Group presence checking
- ✅ Empty group detection
- ✅ Non-standard group identification

### Data Combination
- ✅ Borehole identification
- ✅ Depth point collection
- ✅ Interval creation validation
- ✅ Depth ordering verification
- ✅ Zero-length interval detection
- ✅ Column existence checking
- ✅ Data source tracking

### Depth Intervals
- ✅ Minimum 2 depth points required
- ✅ Consecutive duplicate detection
- ✅ Ordering validation
- ✅ Negative interval flagging

## Technical Details

### DataQualityWarning Class
The `DataQualityWarning` class provides:
- Warning collection and categorization
- Metric tracking
- Severity levels (INFO, WARNING, ERROR)
- Structured output for display

### Helper Functions
- `_create_depth_intervals()`: Creates intervals with validation
- `_extract_depths_from_group()`: Extracts depths with error checking
- Enhanced exception handling throughout

## Migration Notes

### Breaking Changes
Functions now return tuple: `(result, warnings)`
```python
# Old:
result = concat_ags_files(files, giu)

# New:
result, warnings = concat_ags_files(files, giu)
```

### Backward Compatibility
For scripts expecting single return value, update to:
```python
result, _ = concat_ags_files(files, giu)  # Ignore warnings
```

## Support

For questions about data quality warnings:
1. Review this guide
2. Check the warning category and message
3. Inspect the source data
4. Contact support with warning details and sample data

---

**Version**: 2.1  
**Last Updated**: January 2026  
**Documentation**: This guide covers data quality features added in v2.1
