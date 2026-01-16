# AGS Processor - Quick Start Guide

## Installation

1. **Clone or download the repository**
   ```bash
   cd AGS-Processor
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

## Running the Application

Start the Streamlit app:
```bash
streamlit run streamlit_app.py
```

The application will automatically open in your browser at `http://localhost:8501`

## Quick Workflow

### 1. Convert AGS Files to Excel

1. Navigate to **"AGS to Excel"** in the sidebar
2. Upload one or more `.ags` files
3. Enter a GIU (unique identifier) number
4. Click **"Concatenate Files"**
5. Download the resulting Excel file

### 2. Combine Multiple Excel Files

1. Navigate to **"Combine Data"** in the sidebar
2. Upload Excel files (from step 1)
3. Optionally select specific groups to combine
4. Click **"Combine Data"**
5. Download the combined Excel file

### 3. Extract Information

Navigate to **"Information Extraction"** and choose:

#### Search Keyword
- Upload combined data
- Add keywords to search for
- Click **"Search"**
- Download results

#### Match Soil & Grain Size
- Upload combined data
- Add soil types and grain sizes
- Click **"Match"**
- Download results

#### Query Depths
- Upload combined data and depth query file
- Select query type (single/range)
- Click **"Search Depths"**
- Download results

### 4. Advanced Analysis

#### Calculate Rockhead
1. Navigate to **"Calculate Rockhead"**
2. Upload combined data
3. Configure parameters:
   - Core run length (default: 1m)
   - TCR threshold (default: 85%)
   - Continuous length (default: 5m)
4. Click **"Calculate Rockhead"**
5. Download summary or detailed results

#### Calculate Q-value
1. Navigate to **"Calculate Q-value"**
2. Upload combined data with RQD values
3. Configure Q-value parameters:
   - Jn (Joint set number, default: 9)
   - Jr (Joint roughness, default: 1)
   - Ja (Joint alteration, default: 1)
   - Jw (Water factor, default: 1)
   - SRF (Stress factor, default: 1)
4. Click **"Calculate Q-value"**
5. View statistics and download results

## Tips

- **Always start with "AGS to Excel"** to ensure data is properly formatted
- **Use "Combine Data"** before extraction to merge multiple files
- **Preview data** before downloading to verify results
- **Save parameters** for repeated analyses with similar data

## Common Issues

### Import Errors
If you get import errors, ensure all dependencies are installed:
```bash
pip install streamlit pandas numpy openpyxl
```

### File Upload Issues
- Ensure AGS files have `.ags` extension
- Excel files should be outputs from "AGS to Excel" function
- Check file size limits (Streamlit default: 200MB)

### Port Already in Use
If port 8501 is busy, specify a different port:
```bash
streamlit run streamlit_app.py --server.port 8502
```

## Need Help?

- Check the main README.md for detailed documentation
- Review the example workflow above
- Contact the development team for support

## What's New in v2.0?

- ✨ Modern web-based interface (no installation of desktop app needed)
- 🚀 All original PySimpleGUI features maintained
- 📊 New: Rockhead calculation with customizable parameters
- 📐 New: Q-value calculation for rock mass classification
- 💾 Direct download of all results
- 🔄 Real-time progress feedback
- 📱 Responsive design (works on tablets)

Enjoy using AGS Processor v2.0!
