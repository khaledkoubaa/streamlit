# Streamlit Data Display Functions: Cheat Sheet

This cheat sheet provides a quick reference for common Streamlit functions used to display data in your applications. Each function includes a brief explanation, code examples, and key parameters.

## 1. `st.write()`

`st.write()` is Streamlit's "Swiss Army knife" for displaying content. It can render text, numbers, Pandas DataFrames, Matplotlib plots, Altair charts, dictionaries, lists, and more. Streamlit inspects the type of data passed to `st.write()` and decides how to best render it.

**Common Use Cases:**
*   Displaying text (including Markdown).
*   Showing numbers and calculations.
*   Quickly rendering Pandas DataFrames (though `st.dataframe()` offers more control).
*   Displaying variables for debugging.
*   Can handle multiple arguments, which will be printed out sequentially.

**Code Examples:**

```python
import streamlit as st
import pandas as pd
import numpy as np

# Displaying text (Markdown is supported)
st.write("Hello, Streamlit!")
st.write("## This is a Markdown Header")
st.write("You can use *italic*, **bold**, and `code`.")

# Displaying numbers
st.write(1234)
st.write(3.14159)

# Displaying a Pandas DataFrame
data = {'col1': [1, 2, 3], 'col2': ['A', 'B', 'C']}
df = pd.DataFrame(data)
st.write("Here's a DataFrame:", df)

# Displaying multiple arguments
st.write("The value of pi is approximately", 3.14159, "and here's a list:", [1, 2, 3])

# Displaying a dictionary
my_dict = {"name": "Streamlit", "type": "Framework"}
st.write("My dictionary:", my_dict)

# Displaying a list
my_list = ["apple", "banana", "cherry"]
st.write("My list:", my_list)

# You can also pass magic commands (though st.markdown is often preferred for explicit Markdown)
# st.write("---") # Horizontal rule
```

**Key Behavior:**
*   Automatically determines how to render various data types.
*   Supports Markdown syntax within strings.
*   Can accept multiple arguments.

## 2. `st.dataframe()`

`st.dataframe()` is used to display Pandas DataFrames as interactive tables in your Streamlit app. Users can scroll, sort by columns, and view the data in a well-formatted way.

**Common Use Cases:**
*   Displaying tabular data from CSV files, databases, or API responses.
*   Allowing users to explore and understand datasets.
*   Presenting structured information in a clean, interactive format.

**Code Examples:**

```python
import streamlit as st
import pandas as pd
import numpy as np

# Sample DataFrame
data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'David', 'Eve'],
    'Age': [25, 30, 35, 40, 28],
    'City': ['New York', 'London', 'Paris', 'Tokyo', 'Berlin'],
    'Salary': np.random.randint(50000, 100000, 5)
}
df = pd.DataFrame(data)

st.subheader("Basic DataFrame Display")
st.dataframe(df)

st.subheader("DataFrame with Width and Height")
st.dataframe(df, width=600, height=200)

st.subheader("DataFrame with Index Hidden")
st.dataframe(df, hide_index=True)

st.subheader("DataFrame Using Container Width")
st.dataframe(df, use_container_width=True)

# You can also use it with Pandas Styler objects for custom styling
st.subheader("DataFrame with Pandas Styler (e.g., highlighting max)")
def highlight_max(s):
    is_max = s == s.max()
    return ['background-color: yellow' if v else '' for v in is_max]

styled_df = df.style.apply(highlight_max, subset=['Salary'])
st.dataframe(styled_df)
```

**Key Parameters:**
*   `data`: The Pandas DataFrame or Styler object to display.
*   `width (int)`: Desired width of the table in pixels.
*   `height (int)`: Desired height of the table in pixels. If `None`, height will be calculated based on the number of rows.
*   `hide_index (bool)`: If `True`, hides the DataFrame's index. Defaults to `False`.
*   `use_container_width (bool)`: If `True`, the table will expand to the full width of its container. Defaults to `False`.
*   `column_config (dict)`: Allows customization of individual columns (e.g., titles, formatting, visibility). See Streamlit documentation for more details.
*   `column_order (list[str])`: Specifies the order of columns.

## 3. `st.table()`

`st.table()` displays data in a static, non-interactive table. It's suitable for small, simple tables where interactivity like sorting or scrolling is not needed. The entire table is rendered on the page.

**Common Use Cases:**
*   Displaying small, fixed datasets.
*   Presenting summary information or configuration parameters.
*   When a simple, non-interactive tabular view is sufficient.

**Differences from `st.dataframe()`:**
*   **Static:** `st.table()` is static; the entire table is drawn. No scrolling within the table itself.
*   **Non-interactive:** No sorting by clicking column headers.
*   **Styling:** Less direct control over styling compared to `st.dataframe()` with Pandas Styler.

**Code Examples:**

```python
import streamlit as st
import pandas as pd

# Sample data (can be a DataFrame, list of lists, dict, etc.)
data_list = [
    {"Item": "Apple", "Price": 1.0, "Quantity": 100},
    {"Item": "Banana", "Price": 0.5, "Quantity": 150},
    {"Item": "Cherry", "Price": 2.0, "Quantity": 75}
]
df_for_table = pd.DataFrame(data_list)

st.subheader("Basic Table Display (from DataFrame)")
st.table(df_for_table)

# You can also pass other iterable types
data_dict = {
    "Metric": ["Accuracy", "Precision", "Recall"],
    "Score": [0.92, 0.88, 0.95]
}
st.subheader("Table Display (from Dictionary)")
st.table(data_dict)

# List of lists
list_of_lists_data = [
    ["Name", "Role"],
    ["Alice", "Engineer"],
    ["Bob", "Designer"]
]
st.subheader("Table Display (from List of Lists)")
st.table(list_of_lists_data) # First list is treated as header
```

**Key Parameters:**
*   `data`: The data to display. Can be a Pandas DataFrame, NumPy array, list of lists, dictionary, or other iterable.

## 4. `st.metric()`

`st.metric()` is used to display a single key performance indicator (KPI) or metric. It prominently shows a label, a value, and an optional delta (change) with an indicator for positive or negative change.

**Common Use Cases:**
*   Displaying business metrics like revenue, user count, or error rates.
*   Showing model performance scores.
*   Highlighting important statistics with their recent changes.

**Code Examples:**

```python
import streamlit as st

st.subheader("Basic Metric")
st.metric(label="Temperature", value="22°C", delta="1.5°C")

st.subheader("Metric with Negative Delta")
st.metric(label="Stock Price", value="$150.00", delta="-$2.50")

st.subheader("Metric with No Delta")
st.metric(label="Active Users", value="1,234")

st.subheader("Metric with Delta Color Customization")
st.metric(label="Humidity", value="65%", delta="-5%", delta_color="inverse") # Negative delta shown as "good" (green)
st.metric(label="Error Rate", value="2%", delta="0.5%", delta_color="normal") # Positive delta shown as "bad" (red)

# Using columns for layout
col1, col2, col3 = st.columns(3)
with col1:
    st.metric(label="Sales Q1", value="1.2M", delta="150K")
with col2:
    st.metric(label="Sales Q2", value="1.5M", delta="300K")
with col3:
    st.metric(label="Sales Q3 (Projected)", value="1.4M", delta="-100K", delta_color="inverse")

```

**Key Parameters:**
*   `label (str)`: The title or label for the metric.
*   `value (str, int, float)`: The main value to display.
*   `delta (str, int, float, None)`: The change in the metric. If provided, it's displayed with an arrow indicating direction (up for positive, down for negative).
*   `delta_color (str)`: Controls the color of the delta indicator.
    *   `"normal"` (default): Green for positive delta, red for negative.
    *   `"inverse"`: Red for positive delta, green for negative.
    *   `"off"`: No color styling for the delta.
*   `help (str)`: An optional tooltip that appears when the user hovers over the metric's label.

## 5. `st.json()`

`st.json()` displays a JSON string or a Python dictionary/list as an interactive JSON object. Users can expand/collapse nodes in the JSON tree.

**Common Use Cases:**
*   Displaying API responses.
*   Showing complex configuration objects.
*   Debugging or inspecting dictionary or list structures.

**Code Examples:**

```python
import streamlit as st

st.subheader("Displaying a Python Dictionary as JSON")
my_dict = {
    "name": "Streamlit App",
    "version": "1.2.0",
    "settings": {
        "theme": "dark",
        "features": ["charts", "tables", "forms"],
        "users": None
    },
    "active": True
}
st.json(my_dict)

st.subheader("Displaying a JSON String")
json_string = """
{
  "book": {
    "title": "Streamlit for Data Science",
    "author": "Awesome Coder",
    "chapters": [
      {"id": 1, "title": "Introduction"},
      {"id": 2, "title": "Displaying Data"},
      {"id": 3, "title": "Interactive Widgets"}
    ]
  }
}
"""
st.json(json_string)

st.subheader("Collapsed JSON View (by default for large objects)")
# For large objects, expanded=False is often the default.
# Here, we explicitly set it for a smaller object to demonstrate.
st.json(my_dict, expanded=False)

st.subheader("Expanded JSON View (explicitly)")
st.json(my_dict, expanded=True) # Good for smaller JSON or when you want it open initially
```

**Key Parameters:**
*   `body (object or str)`: The JSON string, dictionary, or list to display.
*   `expanded (bool)`: If `True` (default for small objects), all nodes in the JSON tree are expanded. If `False` (default for large objects), all nodes are collapsed.
