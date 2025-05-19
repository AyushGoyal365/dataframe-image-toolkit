DataFrameImage
A flexible toolkit to convert pandas DataFrames into beautifully styled images with rich formatting and visual enhancements.
Features

Rich Formatting: Apply number formatting for currency, percentages, locale-specific formats (lakh, crore)
Visual Elements: Add progress bars, conditional highlighting, and color-coding
Summary Statistics: Automatically calculate and display grand totals with customizable aggregations
Header & Footer: Include titles, logos, and timestamps
Customizable Styling: Control colors, fonts, and visual appearance

Installation
bashpip install dataframe-image-toolkit
Dependencies:

pandas
numpy
dataframe_image
uuid

Quick Example
pythonimport pandas as pd
from dataframe_image import DataFrameImage

# Create a sample DataFrame
df = pd.DataFrame({
    'Product': ['Widget A', 'Widget B', 'Widget C', 'Widget D'],
    'Sales': [125000, 85000, 196000, 78000],
    'Growth': [12.5, -3.2, 8.7, 15.6],
    'Margin': [28, 32, 25, 30]
})

# Create and configure the DataFrameImage
dfi = DataFrameImage(df)
dfi.set_columns({
    'Product': 'Product Name',
    'Sales': 'Sales (₹)',
    'Growth': 'Growth (%)',
    'Margin': 'Margin (%)'
})
dfi.set_number_formatting({
    'Sales': 'lakh:2',
    'Growth': 'percent:1',
    'Margin': 'percent:0'
})
dfi.add_progress_bar(['Margin'], min_value=0, max_value=50)
dfi.highlight_rows(condition=lambda row: row['Growth'] > 10, 
                   color='#059669', text_color='white')
dfi.add_grand_total_row({'Sales': 'sum', 'Growth': 'mean', 'Margin': 'mean'})
dfi.set_header(title="Q2 2024 Sales Report", logo_url="path/to/logo.png")

# Export to image
result = dfi.export(filename="sales_report.png", dpi=300)
print(result['message'])
Key Functions
Basic Configuration
__init__(df)
Initialize with a pandas DataFrame.
pythondfi = DataFrameImage(df)
set_columns(columns)
Select and rename columns for the output.
pythondfi.set_columns({
    'original_col_name': 'Display Name',
    'price': 'Price (₹)',
    'growth': 'Growth (%)'
})
set_number_formatting(formatting)
Apply number formatting to specific columns.
pythondfi.set_number_formatting({
    'sales': 'comma:2',       # 1,234.56
    'revenue': 'lakh:2',      # 12.34L
    'income': 'crore:2',      # 1.23Cr
    'growth': 'percent:1',    # 12.3%
    'price': 'currency:2'     # ₹1,234.56
})
Visual Enhancements
add_progress_bar(columns, min_value=0, max_value=100)
Visualize numeric columns as progress bars.
pythondfi.add_progress_bar(['score', 'completion'], min_value=0, max_value=100)
highlight_rows(top_n=None, column=None, condition=None, color=None, text_color=None)
Highlight rows based on conditions or top/bottom values.
python# Highlight top 3 rows by sales
dfi.highlight_rows(top_n=3, column='sales', color='#0284C7')

# Highlight rows meeting a condition
dfi.highlight_rows(condition=lambda row: row['growth'] > 15 and row['margin'] > 30,
                   color='#059669', text_color='white')
add_grand_total_row(calculations=None, row_label="Grand Total")
Add a summary row with custom calculations.
pythondfi.add_grand_total_row({
    'sales': 'sum',
    'growth': 'mean',
    'customers': 'sum',
    'score': 'median',
    'custom_calc': "df['price'].sum() / df['quantity'].sum()"
})
Layout and Appearance
set_header(title=None, logo_url=None, title_align='center')
Add a header with title and logo.
pythondfi.set_header(
    title="Q2 2024 Sales Report", 
    logo_url="path/to/company_logo.png"
)
set_footer(title=None, logo_url=None, title_align='center')
Add a footer with title and logo.
pythondfi.set_footer(
    title="Confidential - Internal Use Only", 
    logo_url="path/to/logo.png"
)
set_font_size(header=None, cell=None, total_row=None)
Customize font sizes.
pythondfi.set_font_size(header='24px', cell='16px', total_row='18px')
set_dpi(dpi)
Set image resolution.
pythondfi.set_dpi(400)  # Higher resolution for print quality
Export
export(filepath=None, filename=None, format='png', dpi=None)
Generate and save the image.
python# Basic export with auto-generated filename
result = dfi.export()

# Specify filename and DPI
result = dfi.export(filename="sales_report.png", dpi=300)

# Specify full path
result = dfi.export(filepath="/path/to/sales_report.png")
Styling Customization
The color theme can be customized by directly modifying the color_theme attribute:
pythondfi.color_theme = {
    'header_bg': 'linear-gradient(to bottom, #2563EB, #1E3A8A)',
    'header_text': 'white',
    'id_column_bg': '#E1EFFE',
    'id_column_text': '#1F2937',
    'id_column_border': '#2563EB',
    'total_row_bg': 'linear-gradient(to bottom, #1E3A8A, #2563EB)',
    'total_row_text': 'white',
    'cell_bg': 'white',
    'cell_text': '#1F2937',
    'progress_bar_color': '#0EA5E9',
    'progress_bar_darker': '#0284C7',
    'highlight_color': '#059669',
    'highlight_text': 'white',
    'positive_value': '#059669',
    'negative_value': '#DC2626',
    'row_hover': 'rgba(243, 244, 246, 0.2)',
    'alternate_row': 'rgba(249, 250, 251, 0.5)'
}
Advanced Usage
Conditional Formatting with Lambda Functions
python# Highlight rows where growth is positive but margin is below threshold
dfi.highlight_rows(
    condition=lambda row: row['Growth'] > 0 and row['Margin'] < 25,
    color='#FCD34D',
    text_color='#1F2937'
)
Complex Summary Calculations
python# Complex calculations in the total row
dfi.add_grand_total_row({
    'sales': 'sum',
    'revenue': 'sum',
    'margin': "df['revenue'].sum() / df['sales'].sum() * 100",
    'growth': 'mean'
})
Contributing
Contributions are welcome! Please feel free to submit a Pull Request.
License
This project is licensed under the MIT License - see the LICENSE file for details.