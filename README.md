# PyTable-Formatter

[![PyPI version](https://badge.fury.io/py/pytable-formatter.svg)](https://badge.fury.io/py/pytable-formatter)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python Versions](https://img.shields.io/pypi/pyversions/pytable-formatter.svg)](https://pypi.org/project/pytable-formatter/)

Advanced table formatting for terminal output with support for styling, nested tables, and responsive design.

## Overview

PyTable-Formatter is a powerful yet simple library for creating beautifully formatted tables in the terminal. Unlike other table formatting libraries, PyTable-Formatter provides fine-grained control over individual cell styling while maintaining a simple API for basic use cases.

## Key Features

- 🎨 **Rich Styling**: Apply colors, text styles (bold, italic, underline), and background colors to individual cells
- 📏 **Flexible Layouts**: Control alignment, padding, and column widths
- 📊 **Nested Tables**: Create complex table structures with tables inside cells
- 📝 **Custom Formatters**: Apply custom formatting to cell values
- 🖼️ **Borders and Titles**: Add titles, footers, and customize border styles
- 📱 **Responsive Design**: Tables automatically adapt to terminal width

## Installation

```bash
pip install pytable-formatter
```

## Quick Start

### Basic Usage

Creating a simple table requires just a few lines of code:

```python
from pytable_formatter import Table

# Define headers and data
headers = ["Name", "Age", "Country"]
data = [
    ["John Doe", 30, "United States"],
    ["Jane Smith", 25, "Canada"],
    ["Alice Johnson", 35, "Australia"]
]

# Create and display the table
table = Table(headers=headers, data=data)
print(table)
```

Output:
```
┌─────────────┬─────┬───────────────┐
│ Name        │ Age │ Country       │
├─────────────┼─────┼───────────────┤
│ John Doe    │ 30  │ United States │
│ Jane Smith  │ 25  │ Canada        │
│ Alice Johns │ 35  │ Australia     │
└─────────────┴─────┴───────────────┘
```

### Adding Titles and Footers

```python
table = Table(
    headers=headers,
    data=data,
    title="Employee Information",
    footer="3 records found"
)
print(table)
```

Output:
```
┌─────────────── Employee Information ───────────────┐
│ Name        │ Age │ Country       │
├─────────────┼─────┼───────────────┤
│ John Doe    │ 30  │ United States │
│ Jane Smith  │ 25  │ Canada        │
│ Alice Johns │ 35  │ Australia     │
└──────────────── 3 records found ────────────────────┘
```

## Advanced Features

### Cell Styling

Individual cells can be styled with colors, text styles, and alignment:

```python
from pytable_formatter import Table, Cell, TextAlign, TextStyle, Color

# Create styled headers
headers = [
    Cell("Product", style=TextStyle.BOLD, fg_color=Color.WHITE, bg_color=Color.BLUE, align=TextAlign.CENTER),
    Cell("Price", style=TextStyle.BOLD, fg_color=Color.WHITE, bg_color=Color.BLUE, align=TextAlign.CENTER),
    Cell("In Stock", style=TextStyle.BOLD, fg_color=Color.WHITE, bg_color=Color.BLUE, align=TextAlign.CENTER)
]

# Create data with styled cells
data = [
    [
        Cell("Laptop", fg_color=Color.CYAN),
        Cell(1299.99, align=TextAlign.RIGHT, formatter=lambda x: f"${x:,.2f}"),
        Cell("Yes", fg_color=Color.GREEN)
    ],
    [
        Cell("Headphones", fg_color=Color.CYAN),
        Cell(149.99, align=TextAlign.RIGHT, formatter=lambda x: f"${x:,.2f}"),
        Cell("No", fg_color=Color.RED)
    ]
]

# Create and display the table
table = Table(headers=headers, data=data, title="Inventory")
print(table)
```

### Custom Cell Formatting

You can apply custom formatting to any cell by providing a formatter function:

```python
# Format numbers as currency
price_formatter = lambda x: f"${x:,.2f}"

# Format percentages
percentage_formatter = lambda x: f"{x:.1f}%"

# Format dates
from datetime import datetime
date_formatter = lambda x: x.strftime("%Y-%m-%d")

data = [
    [
        "Product A",
        Cell(1250.75, formatter=price_formatter),
        Cell(0.15, formatter=percentage_formatter),
        Cell(datetime(2023, 5, 10), formatter=date_formatter)
    ]
]

table = Table(
    headers=["Product", "Price", "Discount", "Release Date"],
    data=data
)
print(table)
```

### Nested Tables

You can create complex data presentations with tables inside tables:

```python
from pytable_formatter import Table, Cell, TextStyle

# Create a nested table for sales data
sales_table = Table(
    headers=["Q1", "Q2", "Q3", "Q4"],
    data=[
        [15000, 17500, 16000, 19000]
    ],
    border_style="| + + + + + + + + + -"  # Simple ASCII border
)

# Create the main table
main_table = Table(
    headers=[Cell("Department", style=TextStyle.BOLD), Cell("Quarterly Sales", style=TextStyle.BOLD)],
    data=[
        ["Electronics", sales_table]
    ],
    title="Annual Department Sales"
)

print(main_table)
```

### Responsive Tables

Tables automatically adapt to the terminal width:

```python
import os
from pytable_formatter import Table

# Create a table with many columns
headers = ["Col 1", "Col 2", "Col 3", "Col 4", "Col 5", "Col 6", "Col 7", "Col 8", "Col 9", "Col 10"]
data = [
    ["Value 1", "Value 2", "Value 3", "Value 4", "Value 5", "Value 6", "Value 7", "Value 8", "Value 9", "Value 10"]
]

# The table will adapt to different terminal widths
table = Table(headers=headers, data=data)
print(table)
```

### Custom Border Styles

You can customize the border characters used in your tables:

```python
# Default Unicode borders
unicode_table = Table(headers=headers, data=data)

# ASCII borders (works in all terminals)
ascii_table = Table(
    headers=headers, 
    data=data,
    border_style="| + + + + + + + + + -"
)

# Double-line borders
double_table = Table(
    headers=headers,
    data=data,
    border_style="║ ╔ ╗ ╚ ╝ ╠ ╣ ╦ ╩ ╬ ═"
)
```

## Real-World Examples

### Financial Data Display

```python
from pytable_formatter import Table, Cell, TextAlign, TextStyle, Color

# Stock data
stocks = [
    ["AAPL", 175.25, 2.85, 2.85/175.25*100],
    ["MSFT", 320.45, -0.75, -0.75/320.45*100],
    ["GOOGL", 135.60, 1.20, 1.20/135.60*100],
    ["AMZN", 145.80, -1.50, -1.50/145.80*100]
]

formatted_data = []
for stock in stocks:
    symbol, price, change, change_pct = stock
    change_color = Color.GREEN if change >= 0 else Color.RED
    
    formatted_data.append([
        Cell(symbol, style=TextStyle.BOLD),
        Cell(price, align=TextAlign.RIGHT, formatter=lambda x: f"${x:.2f}"),
        Cell(change, align=TextAlign.RIGHT, fg_color=change_color, 
             formatter=lambda x: f"{'+' if x >= 0 else ''}{x:.2f}"),
        Cell(change_pct, align=TextAlign.RIGHT, fg_color=change_color, 
             formatter=lambda x: f"{'+' if x >= 0 else ''}{x:.2f}%")
    ])

table = Table(
    headers=["Symbol", "Price", "Change", "Change %"],
    data=formatted_data,
    title="Stock Market Overview"
)
print(table)
```

### Data Analysis Results

```python
import random
from pytable_formatter import Table, Cell, TextAlign, Color

# Simulate data analysis results
datasets = ["Dataset A", "Dataset B", "Dataset C"]
metrics = {
    "Accuracy": [random.uniform(0.85, 0.99) for _ in range(3)],
    "Precision": [random.uniform(0.80, 0.95) for _ in range(3)],
    "Recall": [random.uniform(0.75, 0.92) for _ in range(3)],
    "F1 Score": [random.uniform(0.78, 0.96) for _ in range(3)]
}

# Format data for the table
headers = ["Dataset", "Accuracy", "Precision", "Recall", "F1 Score"]
data = []

for i, dataset in enumerate(datasets):
    row = [Cell(dataset, style=TextStyle.BOLD)]
    
    for metric in ["Accuracy", "Precision", "Recall", "F1 Score"]:
        value = metrics[metric][i]
        # Color based on threshold
        color = Color.GREEN if value > 0.9 else (Color.YELLOW if value > 0.8 else Color.RED)
        row.append(Cell(value, align=TextAlign.RIGHT, fg_color=color, formatter=lambda x: f"{x:.2%}"))
    
    data.append(row)

# Create the table
table = Table(
    headers=headers,
    data=data,
    title="Machine Learning Model Evaluation"
)
print(table)
```

## API Reference

### Table Class

```python
Table(
    headers=None,          # List of column headers (strings or Cell objects)
    data=None,             # List of rows, each containing cells (values or Cell objects)
    title=None,            # Optional title to display above the table
    footer=None,           # Optional footer to display below the table
    min_width=None,        # Minimum width for the table (in characters)
    max_width=None,        # Maximum width for the table (in characters)
    padding=1,             # Number of spaces to add around cell content
    border_style="│ ┌ ┐ └ ┘ ├ ┤ ┬ ┴ ┼ ─"  # Border characters
)
```

Methods:
- `add_row(row)`: Add a row to the table
- `render()`: Render the table as a string
- `__str__()`: Return the rendered table (called by print)

### Cell Class

```python
Cell(
    value,                               # Cell content
    align=TextAlign.LEFT,                # Text alignment within the cell
    style=None,                          # Text style
    fg_color=None,                       # Foreground (text) color
    bg_color=None,                       # Background color
    span=1,                              # Number of columns this cell spans
    formatter=None                       # Custom function to format the cell value
)
```

### TextAlign Enum

- `TextAlign.LEFT`: Left-align cell content
- `TextAlign.RIGHT`: Right-align cell content
- `TextAlign.CENTER`: Center cell content

### TextStyle Enum

- `TextStyle.NORMAL`: Normal text
- `TextStyle.BOLD`: Bold text
- `TextStyle.ITALIC`: Italic text
- `TextStyle.UNDERLINE`: Underlined text

### Color Enum

- `Color.BLACK`
- `Color.RED`
- `Color.GREEN`
- `Color.YELLOW`
- `Color.BLUE`
- `Color.MAGENTA`
- `Color.CYAN`
- `Color.WHITE`
- `Color.DEFAULT`

## Comparison with Other Libraries

While there are several table formatting libraries for Python (like PrettyTable, Tabulate, etc.), PyTable-Formatter offers unique advantages:

| Feature | PyTable-Formatter | PrettyTable | Tabulate |
|---------|------------------|-------------|----------|
| Cell-level styling | ✅ | ❌ | ❌ |
| Custom cell formatters | ✅ | ❌ | ❌ |
| Nested tables | ✅ | ❌ | ❌ |
| Auto width adjustment | ✅ | ✅ | ✅ |
| Text alignment per cell | ✅ | ❌ | ❌ |
| Custom border styles | ✅ | ✅ | ✅ |
| Title and footer support | ✅ | ✅ | ❌ |

## Performance Considerations

PyTable-Formatter is designed to handle tables of various sizes efficiently. However, for extremely large tables (thousands of rows), consider:

1. Limiting the number of styled cells (especially with background colors)
2. Using simpler border styles
3. Considering pagination for very large datasets

## Terminal Compatibility

PyTable-Formatter works in most terminal environments:

- Modern terminals with Unicode support will display the default borders correctly
- For older terminals, use ASCII border styles
- Color support is automatically detected and disabled if not available
- Windows, macOS, and Linux are all supported

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Credits

Developed by Biswanath Roul
