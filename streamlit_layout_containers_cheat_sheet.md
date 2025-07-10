# Streamlit Layout and Containers: Cheat Sheet

This cheat sheet provides a quick reference for common Streamlit functions and objects used to organize the layout of your applications and group elements. Each feature includes a brief explanation, code examples, and key parameters or behaviors.

## 1. `st.sidebar`

`st.sidebar` is an object that allows you to add widgets and content to a dedicated sidebar area, separate from the main content of your Streamlit app. It's useful for navigation, filters, settings, or inputs that should persist across different views in your app.

**Common Use Cases:**
*   Placing navigation links (e.g., using `st.page_link` or radio buttons).
*   Adding input widgets like sliders, selectboxes, or text inputs to control the main app content.
*   Displaying app information, logos, or help text.

**Code Examples:**

```python
import streamlit as st

# Method 1: Calling Streamlit commands directly on st.sidebar
st.sidebar.header("App Controls")
st.sidebar.write("Use the controls below to filter data or change settings.")

selected_option = st.sidebar.selectbox(
    "Choose a data source:",
    ["Source A", "Source B", "Source C"]
)
st.sidebar.slider("Filter by value:", 0, 100, 50)

# Method 2: Using 'with st.sidebar:' context manager
with st.sidebar:
    st.markdown("---") # Horizontal rule
    st.subheader("About")
    st.info("This app demonstrates Streamlit's sidebar functionality.")
    if st.button("Show App Info in Main Area"):
        st.session_state.show_info = True # Using session state to communicate

# Main area content
st.title("Main Application Area")
st.write(f"Selected data source from sidebar: {selected_option}")

if 'show_info' in st.session_state and st.session_state.show_info:
    st.success("App info button in sidebar was clicked!")
    del st.session_state.show_info # Reset after showing

```

**Key Behavior:**
*   Any Streamlit command that can be used in the main app (e.g., `st.write`, `st.slider`, `st.button`) can also be used with `st.sidebar` (e.g., `st.sidebar.write(...)`, `st.sidebar.slider(...)`).
*   Alternatively, use the `with st.sidebar:` block, and any Streamlit commands inside this block will be rendered in the sidebar.
*   The sidebar is collapsible by default.
*   Its state (e.g., input widget values) is preserved across app reruns, just like widgets in the main area.
*   `st.sidebar` does not take parameters itself; you call methods on it or use it as a context manager.

## 2. `st.columns()`

`st.columns()` creates side-by-side containers (columns) to arrange elements horizontally. It returns a list of column objects that can be used with a `with` statement or by calling Streamlit commands directly on them.

**Common Use Cases:**
*   Displaying related pieces of information or controls next to each other.
*   Creating dashboard-like layouts.
*   Arranging input fields and their corresponding outputs side-by-side.

**Code Examples:**

```python
import streamlit as st
import numpy as np

st.subheader("Basic Two Columns (Equal Width)")
col1, col2 = st.columns(2) # Creates two columns of equal width

with col1:
    st.header("Column 1")
    st.write("This is content in the first column.")
    st.button("Button in Col 1")

# You can also call methods directly on the column object
col2.header("Column 2")
col2.write("This is content in the second column.")
if col2.button("Button in Col 2"):
    st.toast("Col 2 button clicked!")


st.subheader("Columns with Different Width Ratios (Weights)")
# Creates three columns: colA is twice as wide as colB, colC is three times as wide as colB
colA, colB, colC = st.columns([2, 1, 3]) # Ratio: 2:1:3

colA.subheader("Column A (Width 2)")
colA.image("https://streamlit.io/images/brand/streamlit-logo-secondary-colormark-darktext.png") # Example image

colB.subheader("Column B (Width 1)")
colB.metric("Temperature", "22°C", "1.2°C")

colC.subheader("Column C (Width 3)")
colC.line_chart(np.random.randn(20, 2))


st.subheader("Columns with Gap Control")
st.write("Columns with a 'large' gap:")
lg_col1, lg_col2 = st.columns(2, gap="large")
lg_col1.success("Content in col 1 (large gap)")
lg_col2.warning("Content in col 2 (large gap)")

st.write("Columns with 'small' gap:")
sm_col1, sm_col2 = st.columns(2, gap="small")
sm_col1.info("Content in col 1 (small gap)")
sm_col2.error("Content in col 2 (small gap)")

st.write("Columns with no gap (`gap=None` introduced in newer versions):")
# This feature (gap=None) was highlighted in recent updates (e.g., 1.46.0)
# Ensure your Streamlit version supports it if you use it.
no_gap_col1, no_gap_col2 = st.columns(2, gap=None) # Or "small", "medium", "large"
if no_gap_col1.button("Button A (No Gap)", use_container_width=True):
    pass
if no_gap_col2.button("Button B (No Gap)", use_container_width=True):
    pass

```

**Key Parameters:**
*   `spec (int or list[int or float])`:
    *   If `int`: The number of columns to create, all with equal width.
    *   If `list[int or float]`: A list specifying the relative widths (weights) of the columns. For example, `[1, 2, 1]` creates three columns where the middle one is twice as wide as the others.
*   `gap (str or None)`: The size of the gap between columns. Can be `"small"`, `"medium"` (default), `"large"`, or `None` (for no gap, in newer Streamlit versions).

## 3. `st.tabs()`

`st.tabs()` creates a set of tabbed sections, allowing you to organize content and let users switch between different views within the same area of the app. It returns a list of tab objects that can be used with a `with` statement.

**Common Use Cases:**
*   Organizing different categories of information or charts.
*   Separating input forms from results or visualizations.
*   Providing different views of the same data (e.g., "Table View", "Chart View").

**Code Examples:**

```python
import streamlit as st
import numpy as np
import pandas as pd

st.subheader("Basic Tabs")

tab1, tab2, tab3 = st.tabs(["📈 Chart", "🗃️ Data", "📝 Details"])

with tab1:
    st.header("A Cool Chart")
    st.line_chart(np.random.randn(50, 3))
    st.write("This tab shows a line chart with random data.")

with tab2:
    st.header("Raw Data Table")
    df = pd.DataFrame(
        np.random.randn(10, 5),
        columns=('col %d' % i for i in range(5))
    )
    st.dataframe(df)
    st.write("This tab displays a Pandas DataFrame.")

with tab3:
    st.header("More Information")
    st.markdown("""
    This section provides additional details about the data and charts presented.
    - **Data Source:** Randomly generated.
    - **Chart Type:** Line chart.
    - **Purpose:** Demonstration of `st.tabs()`.
    """)
    if st.button("Click for more details in Tab 3"):
        st.info("You found the hidden detail!")

# Example of using tabs for different settings
st.subheader("Tabs for Configuration")
settings_tabs = st.tabs(["General Settings", "Advanced Settings", "Profile"])

with settings_tabs[0]: # Accessing by index
    st.write("General application settings:")
    st.checkbox("Enable notifications")
    st.selectbox("Theme", ["Light", "Dark", "System Default"])

with settings_tabs[1]:
    st.write("Advanced configuration options:")
    st.slider("Timeout (seconds)", 10, 120, 30)
    st.text_input("API Key (optional)")

with settings_tabs[2]:
    st.write("User profile information:")
    st.text_input("Username", "streamlit_user")
    st.button("Save Profile")

```

**Key Parameters:**
*   `tabs (list[str])`: A list of strings, where each string is the label for a tab. The order in the list determines the order of the tabs.

**Key Behavior:**
*   `st.tabs()` returns a list of tab objects, corresponding to the labels provided.
*   You use a `with` statement with each tab object to define the content within that tab.
*   Only the content of the currently active tab is rendered and visible.
*   The state of widgets within inactive tabs is generally preserved.

## 4. `st.expander()`

`st.expander()` creates a collapsible container, often referred to as an "accordion" or "details" section. Users can click to expand or collapse the container to show or hide its content.

**Common Use Cases:**
*   Hiding advanced settings or less frequently used options.
*   Showing detailed explanations or help text without cluttering the main UI.
*   Organizing long sections of content.

**Code Examples:**

```python
import streamlit as st
import pandas as pd
import numpy as np

st.subheader("Basic Expander (Collapsed by Default)")
with st.expander("Click to see more details"):
    st.write("This is some detailed content that was initially hidden.")
    st.image("https://streamlit.io/images/brand/streamlit-logo-secondary-lightmark-lighttext.png", width=150)
    st.warning("You've expanded the section!")

st.subheader("Expander Initially Expanded")
with st.expander("Advanced Configuration Options", expanded=True):
    st.write("These options are shown by default because `expanded=True`.")
    st.checkbox("Enable experimental features")
    st.select_slider("Verbosity Level", options=['Low', 'Medium', 'High'], value='Medium')

st.subheader("Expander with Complex Content")
exp = st.expander("Show Data and Chart", expanded=False)
with exp: # Can also assign to a variable and use 'with' later
    st.markdown("Here's a sample DataFrame and a chart inside an expander:")
    df = pd.DataFrame(np.random.randn(5, 3), columns=['A', 'B', 'C'])
    st.dataframe(df)
    st.bar_chart(df['A'])

# You can also add elements to an expander object directly (less common than `with`)
# another_expander = st.expander("Directly added content")
# another_expander.write("Content added via method call.")
# another_expander.button("Button inside expander")

```

**Key Parameters:**
*   `label (str)`: The text label displayed on the expander, which users click to toggle.
*   `expanded (bool)`: If `True`, the expander will be initially expanded. Defaults to `False` (collapsed).
*   `icon (str or None)`: (Newer Streamlit versions) An optional emoji or icon to display next to the label.

**Key Behavior:**
*   Content within an expander is only rendered when the expander is open, which can be useful for performance if the content is complex.
*   The state of widgets inside an expander is preserved even when it's collapsed.

## 5. `st.container()`

`st.container()` creates an invisible container that can be used to group multiple elements. A key feature is that elements can be inserted into a container "out of order" relative to the main script flow. This is useful for dynamically building layouts or inserting elements into a predefined slot later in the script.

**Common Use Cases:**
*   Grouping related elements visually (though often `st.columns` or `st.expander` provide visible grouping).
*   Inserting elements into a specific part of the page after other elements have already been written (out-of-order writing).
*   Creating layouts where the structure is defined first, and content is filled in later.
*   Creating scrollable areas using the `height` parameter.

**Code Examples:**

```python
import streamlit as st

st.subheader("Basic Container for Grouping")
with st.container(): # You can optionally add a border with border=True
    st.write("This is inside a container.")
    st.button("Button inside container")
    st.markdown("---")

st.write("This text is outside the container above.")


st.subheader("Out-of-Order Element Insertion")
# Define a container first
header_container = st.container(border=True) # Added border for visibility

# Write some content to the main page
st.write("Some content written to the main page body.")
st.slider("A slider in the main flow", 0, 10, 5)

# Now, write content into the container defined earlier
with header_container:
    st.header("This Header is Inside the Container")
    st.info("This info message was written into the `header_container` out of order.")

st.write("More content on the main page, after the container's content was inserted.")


st.subheader("Container with Fixed Height and Scrollbar")
with st.container(height=200, border=True): # Height in pixels
    st.write("This container has a fixed height of 200px.")
    for i in range(20):
        st.write(f"Item number {i+1} in a scrollable list.")
    st.success("End of scrollable content.")

```

**Key Parameters:**
*   `height (int or None)`: If set to an integer, the container will have a fixed height in pixels, and a vertical scrollbar will appear if the content exceeds this height. Defaults to `None` (dynamic height).
*   `border (bool)`: If `True`, draws a border around the container. Defaults to `False`. (Available in newer Streamlit versions).

**Key Behavior:**
*   Containers are invisible by default unless `border=True`.
*   Elements are typically added to a container using a `with` statement.
*   The primary power of `st.container()` is enabling elements to be added to it from anywhere in the script after the container has been instantiated.

## 6. `st.empty()`

`st.empty()` creates a special single-element container, often referred to as a "placeholder." This container can hold only one element at a time. You can then replace the content of this placeholder with a new element or clear it entirely.

**Common Use Cases:**
*   Displaying status messages that change over time (e.g., "Processing...", "Complete!").
*   Creating animations or dynamic updates in a specific spot on the page.
*   Replacing an element with another based on user interaction or app state.
*   Clearing previously displayed information.

**Code Examples:**

```python
import streamlit as st
import time

st.subheader("Dynamic Status Updates with `st.empty()`")

# Create a placeholder
status_placeholder = st.empty()

status_placeholder.info("Starting a long process... Please wait.")
time.sleep(2) # Simulate some work

status_placeholder.warning("Still processing... Almost there.")
time.sleep(2) # Simulate more work

status_placeholder.success("Process complete! ✅")
time.sleep(1)

# You can replace it with any other Streamlit element
status_placeholder.markdown("### Final Results:")
# status_placeholder.dataframe({'col1': [1,2], 'col2': [3,4]}) # Example of replacing with a dataframe

# To clear the placeholder (make it empty again)
# status_placeholder.empty()


st.subheader("Replacing Content in a Placeholder")
content_placeholder = st.empty()

if st.button("Show Text"):
    content_placeholder.text("You clicked the Text button!")

if st.button("Show Chart"):
    # When this button is clicked, the text (if any) in content_placeholder will be replaced
    content_placeholder.line_chart(data=[1, 5, 2, 6, 2, 1])

if st.button("Clear Placeholder"):
    content_placeholder.empty() # Removes the element from the placeholder

```

**Key Behavior:**
*   `st.empty()` returns a container object.
*   This container can hold at most one Streamlit element at any time.
*   When you call a Streamlit command (e.g., `.write()`, `.info()`, `.line_chart()`) on an `st.empty()` object, it replaces the current content of that placeholder with the new element.
*   Calling `.empty()` on the placeholder object itself (e.g., `my_placeholder.empty()`) removes any element currently in it, making it visually empty.
*   Unlike `st.container()`, `st.empty()` is designed for single, replaceable elements, not for grouping multiple persistent elements.
