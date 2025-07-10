# Streamlit Input Widgets: Cheat Sheet

This cheat sheet provides a quick reference for common Streamlit input widgets used to gather user input in your applications. Each widget includes a brief explanation, code examples, and key parameters.

## 1. `st.button()`

`st.button()` displays a button widget. When a button is clicked, it returns `True` for a single script rerun, triggering actions or conditional logic.

**Common Use Cases:**
*   Triggering calculations or processes.
*   Submitting form data (though `st.form_submit_button` is often preferred within forms).
*   Navigating between app states or pages (in conjunction with other logic).

**Code Examples:**

```python
import streamlit as st

st.subheader("Basic Button")
if st.button("Click me!"):
    st.write("Button was clicked!")

st.subheader("Button with Callback")

if 'click_count' not in st.session_state:
    st.session_state.click_count = 0

def increment_counter():
    st.session_state.click_count += 1

st.button("Increment Counter", on_click=increment_counter)
st.write(f"Button clicked {st.session_state.click_count} times.")

st.subheader("Primary and Secondary Buttons")
if st.button("Submit Action", type="primary"):
    st.success("Primary action submitted!")

if st.button("Cancel Action", type="secondary"): # Default type
    st.warning("Secondary action triggered.")

st.subheader("Disabled Button")
st.button("Can't Click Me", disabled=True)

st.subheader("Button Using Container Width")
st.button("Full Width Button", use_container_width=True)

```

**Key Parameters:**
*   `label (str)`: The text displayed on the button.
*   `key (str)`: An optional unique key for the widget.
*   `help (str)`: An optional tooltip that appears on hover.
*   `on_click (callable)`: A callback function to be executed when the button is clicked.
*   `args (tuple)`: Arguments to pass to the `on_click` callback.
*   `kwargs (dict)`: Keyword arguments to pass to the `on_click` callback.
*   `type (str)`: The button type, can be `"secondary"` (default) or `"primary"` (for more prominent actions).
*   `disabled (bool)`: If `True`, the button is deactivated. Defaults to `False`.
*   `use_container_width (bool)`: If `True`, the button will expand to the full width of its container. Defaults to `False`.

## 2. `st.download_button()`

`st.download_button()` displays a button that, when clicked, allows users to download data from your Streamlit app to their local machine.

**Common Use Cases:**
*   Allowing users to download generated reports (CSV, TXT, PDF).
*   Providing datasets for download.
*   Exporting images or other binary files.

**Code Examples:**

```python
import streamlit as st
import pandas as pd

st.subheader("Download Text Data")
text_contents = "This is some text data to download."
st.download_button(
    label="Download Text File",
    data=text_contents,
    file_name="my_text_file.txt",
    mime="text/plain"
)

st.subheader("Download CSV Data")
data = {'col1': [1, 2, 3], 'col2': ['A', 'B', 'C']}
df = pd.DataFrame(data)
csv_data = df.to_csv(index=False).encode('utf-8') # Important: encode to bytes

st.download_button(
    label="Download CSV File",
    data=csv_data,
    file_name="my_data.csv",
    mime="text/csv",
)

st.subheader("Download Button with Callback")
if 'download_count' not in st.session_state:
    st.session_state.download_count = 0

def count_download():
    st.session_state.download_count += 1
    st.toast(f"Download initiated! Total downloads: {st.session_state.download_count}")

st.download_button(
    label="Download & Count",
    data="Some data to download.",
    file_name="counted_download.txt",
    mime="text/plain",
    on_click=count_download
)
st.write(f"Download button clicked {st.session_state.download_count} times.")

st.subheader("Disabled Download Button")
st.download_button("Cannot Download", data="N/A", file_name="na.txt", disabled=True)
```

**Key Parameters:**
*   `label (str)`: The text displayed on the download button.
*   `data (str or bytes or file-like)`: The data to be downloaded. If `str`, it will be encoded as UTF-8. For binary data, use `bytes`.
*   `file_name (str)`: The suggested name for the downloaded file in the user's browser.
*   `mime (str)`: The MIME type of the data (e.g., `"text/plain"`, `"text/csv"`, `"image/png"`). This helps the browser handle the file correctly.
*   `key (str)`: An optional unique key for the widget.
*   `help (str)`: An optional tooltip.
*   `on_click (callable)`: A callback function executed when the button is clicked (before download starts).
*   `args (tuple)`: Arguments for the `on_click` callback.
*   `kwargs (dict)`: Keyword arguments for the `on_click` callback.
*   `type (str)`: Button type (`"secondary"` or `"primary"`).
*   `disabled (bool)`: If `True`, the button is deactivated.
*   `use_container_width (bool)`: If `True`, expands to container width.

## 3. `st.checkbox()`

`st.checkbox()` displays a checkbox widget. It returns `True` if the checkbox is checked, and `False` otherwise.

**Common Use Cases:**
*   Toggling features on or off.
*   Accepting terms and conditions.
*   Controlling the visibility of other elements.

**Code Examples:**

```python
import streamlit as st

st.subheader("Basic Checkbox")
agree = st.checkbox("I agree to the terms and conditions")
if agree:
    st.write("Thank you for agreeing!")
else:
    st.write("Please agree to proceed.")

st.subheader("Checkbox to Show/Hide Content")
show_details = st.checkbox("Show details", value=True) # Initially checked

if show_details:
    st.write("Here are some details you asked for.")
    st.write("- Detail 1")
    st.write("- Detail 2")

st.subheader("Checkbox with Callback")

if 'checked_status' not in st.session_state:
    st.session_state.checked_status = False

def log_checkbox_change():
    st.session_state.checked_status = st.session_state.my_checkbox # Access widget value via key
    st.toast(f"Checkbox status: {st.session_state.checked_status}")

st.checkbox(
    "Enable Feature X",
    key="my_checkbox", # Assign a key to access its value in the callback
    on_change=log_checkbox_change
)
st.write(f"Feature X is currently: {'Enabled' if st.session_state.checked_status else 'Disabled'}")

st.subheader("Disabled Checkbox")
st.checkbox("Cannot change this", value=True, disabled=True)
```

**Key Parameters:**
*   `label (str)`: The text displayed next to the checkbox.
*   `value (bool)`: The initial state of the checkbox (checked or unchecked). Defaults to `False`.
*   `key (str)`: An optional unique key for the widget.
*   `help (str)`: An optional tooltip.
*   `on_change (callable)`: A callback function executed when the checkbox state changes.
*   `args (tuple)`: Arguments for the `on_change` callback.
*   `kwargs (dict)`: Keyword arguments for the `on_change` callback.
*   `disabled (bool)`: If `True`, the checkbox is deactivated. Defaults to `False`.

## 4. `st.radio()`

`st.radio()` displays a set of radio buttons, allowing the user to select exactly one option from a list.

**Common Use Cases:**
*   Choosing a single item from a predefined list (e.g., model type, display mode).
*   Setting a specific configuration option.
*   Simple single-choice questions.

**Code Examples:**

```python
import streamlit as st

st.subheader("Basic Radio Buttons")
options = ["Option A", "Option B", "Option C"]
choice = st.radio("Choose one option:", options)
st.write(f"You selected: {choice}")

st.subheader("Radio Buttons with Default Selection (by index)")
# 'Option B' will be selected initially as it's at index 1
default_choice = st.radio(
    "Choose your preferred contact method:",
    ("Email", "Phone", "Mail"),
    index=1
)
st.write(f"Preferred contact: {default_choice}")

st.subheader("Horizontal Radio Buttons")
horizontal_choice = st.radio(
    "Select your size:",
    ["S", "M", "L", "XL"],
    horizontal=True
)
st.write(f"Selected size: {horizontal_choice}")

st.subheader("Radio with `format_func`")
# Example: Displaying user-friendly names for internal keys
options_dict = {"opt1": "Detailed View", "opt2": "Summary View", "opt3": "Compact View"}
selected_key = st.radio(
    "Select display mode:",
    options=list(options_dict.keys()),
    format_func=lambda key: options_dict[key] # Show values to user, return key
)
st.write(f"Internal key selected: {selected_key}, which is '{options_dict[selected_key]}'")


st.subheader("Radio with Callback")
if 'radio_selection' not in st.session_state:
    st.session_state.radio_selection = None

def log_radio_change():
    st.session_state.radio_selection = st.session_state.my_radio # Access widget value by key
    st.toast(f"Radio selection changed to: {st.session_state.radio_selection}")

st.radio(
    "Choose a delivery option:",
    ("Standard", "Express", "Overnight"),
    key="my_radio",
    on_change=log_radio_change
)
if st.session_state.radio_selection:
    st.write(f"Current delivery: {st.session_state.radio_selection}")

st.subheader("Disabled Radio Buttons")
st.radio("Cannot change this choice", options=["Fixed Option"], index=0, disabled=True)

```

**Key Parameters:**
*   `label (str)`: A short label explaining to the user what this radio group is for.
*   `options (Iterable)`: A list, tuple, or other iterable of options to display.
*   `index (int)`: The index of the initially selected option. Defaults to `0`.
*   `format_func (callable)`: A function to apply to each option to customize how it's displayed. The function should take one argument (the option) and return a string.
*   `key (str)`: An optional unique key for the widget.
*   `help (str)`: An optional tooltip.
*   `on_change (callable)`: A callback function executed when the selection changes.
*   `args (tuple)`: Arguments for the `on_change` callback.
*   `kwargs (dict)`: Keyword arguments for the `on_change` callback.
*   `disabled (bool)`: If `True`, the radio buttons are deactivated. Defaults to `False`.
*   `horizontal (bool)`: If `True`, displays the radio buttons horizontally. Defaults to `False`.
*   `captions (Iterable[str])`: A list of captions to display below each radio button. The length must match the `options`. (Newer Streamlit versions)

## 5. `st.selectbox()`

`st.selectbox()` displays a dropdown menu from which the user can select a single option.

**Common Use Cases:**
*   Selecting an item from a longer list where radio buttons would take too much space.
*   Choosing a parameter value from a predefined set.
*   Filtering data based on a selected category.

**Code Examples:**

```python
import streamlit as st

st.subheader("Basic Selectbox")
options = ["Email", "Phone", "SMS", "Carrier Pigeon"]
selection = st.selectbox("How would you like to be contacted?", options)
st.write(f"You selected: {selection}")

st.subheader("Selectbox with Default Selection (by index)")
# "Phone" will be selected initially (index 1)
default_selection = st.selectbox(
    "Choose your preferred device:",
    ("Laptop", "Desktop", "Tablet", "Mobile"),
    index=1
)
st.write(f"Preferred device: {default_selection}")

st.subheader("Selectbox with `format_func`")
# Similar to st.radio, useful for displaying user-friendly names for internal keys
country_codes = {"US": "United States", "CA": "Canada", "GB": "United Kingdom", "DE": "Germany"}
selected_code = st.selectbox(
    "Select your country:",
    options=list(country_codes.keys()),
    format_func=lambda code: f"{country_codes[code]} ({code})" # Show full name and code
)
st.write(f"You selected: {country_codes[selected_code]} (Code: {selected_code})")

st.subheader("Selectbox with Placeholder")
placeholder_selection = st.selectbox(
    "Choose a color (optional):",
    ["Red", "Green", "Blue", "Yellow"],
    index=None, # Important for placeholder to show
    placeholder="Select a color..."
)
if placeholder_selection:
    st.write(f"Selected color: {placeholder_selection}")
else:
    st.write("No color selected.")


st.subheader("Selectbox with Callback")
if 'selectbox_choice' not in st.session_state:
    st.session_state.selectbox_choice = None

def log_selectbox_change():
    st.session_state.selectbox_choice = st.session_state.my_selectbox
    st.toast(f"Selectbox changed to: {st.session_state.selectbox_choice}")

st.selectbox(
    "Select a framework:",
    ("Streamlit", "Flask", "Django", "FastAPI"),
    key="my_selectbox",
    on_change=log_selectbox_change,
    index=0 # Set an initial value to see changes
)
if st.session_state.selectbox_choice:
    st.write(f"Current framework: {st.session_state.selectbox_choice}")


st.subheader("Disabled Selectbox")
st.selectbox("Cannot change this", options=["Fixed Value"], index=0, disabled=True)

```

**Key Parameters:**
*   `label (str)`: A short label explaining to the user what this selectbox is for.
*   `options (Iterable)`: A list, tuple, or other iterable of options to display.
*   `index (int or None)`: The index of the initially selected option. If `None` and `placeholder` is set, the placeholder is displayed. Defaults to `0`.
*   `format_func (callable)`: A function to apply to each option to customize how it's displayed.
*   `placeholder (str)`: A string to display when no option is selected. Requires `index=None`.
*   `key (str)`: An optional unique key for the widget.
*   `help (str)`: An optional tooltip.
*   `on_change (callable)`: A callback function executed when the selection changes.
*   `args (tuple)`: Arguments for the `on_change` callback.
*   `kwargs (dict)`: Keyword arguments for the `on_change` callback.
*   `disabled (bool)`: If `True`, the selectbox is deactivated. Defaults to `False`.

## 6. `st.multiselect()`

`st.multiselect()` displays a dropdown menu that allows users to select multiple options from a list. It returns a list of the selected options.

**Common Use Cases:**
*   Filtering data by multiple categories.
*   Choosing multiple tags or features.
*   Selecting multiple items for processing.

**Code Examples:**

```python
import streamlit as st

st.subheader("Basic Multiselect")
options = ["Red", "Green", "Blue", "Yellow", "Purple", "Orange"]
selected_colors = st.multiselect("Choose your favorite colors:", options)
st.write("You selected:", selected_colors)

st.subheader("Multiselect with Default Selections")
# "Green" and "Purple" will be pre-selected
default_selected = st.multiselect(
    "Select preferred toppings for your pizza:",
    ["Pepperoni", "Mushrooms", "Onions", "Olives", "Extra Cheese"],
    default=["Mushrooms", "Extra Cheese"]
)
st.write("Selected toppings:", default_selected)

st.subheader("Multiselect with `format_func`")
# Displaying user-friendly names for internal keys
product_skus = {
    "SKU001": "Laptop Pro 15-inch",
    "SKU002": "Wireless Mouse Ergonomic",
    "SKU003": "Mechanical Keyboard RGB",
    "SKU004": "27-inch 4K Monitor"
}
selected_skus = st.multiselect(
    "Select products to add to cart:",
    options=list(product_skus.keys()),
    format_func=lambda sku: product_skus[sku], # Show product names
    default=["SKU001"]
)
if selected_skus:
    st.write("Selected product SKUs:", selected_skus)
    st.write("Which correspond to:")
    for sku in selected_skus:
        st.write(f"- {product_skus[sku]}")
else:
    st.write("No products selected.")

st.subheader("Multiselect with Max Selections")
limited_choices = st.multiselect(
    "Choose up to 2 fruits:",
    ["Apple", "Banana", "Cherry", "Dates", "Elderberry"],
    max_selections=2
)
st.write("Your fruit choices (max 2):", limited_choices)


st.subheader("Multiselect with Callback")
if 'multi_selection' not in st.session_state:
    st.session_state.multi_selection = []

def log_multi_change():
    st.session_state.multi_selection = st.session_state.my_multiselect
    st.toast(f"Multiselect changed to: {st.session_state.multi_selection}")

st.multiselect(
    "Select your skills:",
    ("Python", "Streamlit", "Pandas", "SQL", "Machine Learning"),
    key="my_multiselect",
    on_change=log_multi_change,
    default=["Python", "Streamlit"]
)
st.write(f"Current skills: {st.session_state.multi_selection}")


st.subheader("Disabled Multiselect")
st.multiselect(
    "Cannot change these selections",
    options=["Fixed A", "Fixed B"],
    default=["Fixed A"],
    disabled=True
)

```

**Key Parameters:**
*   `label (str)`: A short label explaining to the user what this multiselect is for.
*   `options (Iterable)`: A list, tuple, or other iterable of options to display.
*   `default (Iterable or None)`: A list of options that should be selected initially. Defaults to `None` (no options selected).
*   `format_func (callable)`: A function to apply to each option to customize how it's displayed.
*   `key (str)`: An optional unique key for the widget.
*   `help (str)`: An optional tooltip.
*   `on_change (callable)`: A callback function executed when the selection changes.
*   `args (tuple)`: Arguments for the `on_change` callback.
*   `kwargs (dict)`: Keyword arguments for the `on_change` callback.
*   `disabled (bool)`: If `True`, the multiselect is deactivated. Defaults to `False`.
*   `max_selections (int or None)`: The maximum number of options that can be selected. If `None`, there is no limit.
*   `placeholder (str)`: Text to display when no options are selected. (Newer Streamlit versions)

## 7. `st.slider()`

`st.slider()` displays a slider widget, allowing the user to select a single numerical value or a range of values from a defined interval.

**Common Use Cases:**
*   Adjusting numerical parameters (e.g., model hyperparameters, filter thresholds).
*   Selecting a year, age, or percentage.
*   Defining a range for filtering or analysis.

**Code Examples:**

```python
import streamlit as st
from datetime import time, date, timedelta

st.subheader("Integer Slider")
age = st.slider("Select your age:", min_value=0, max_value=100, value=25, step=1)
st.write(f"Selected age: {age}")

st.subheader("Float Slider")
temperature = st.slider(
    "Set temperature (°C):",
    min_value=-10.0, max_value=40.0, value=22.5, step=0.1, format="%.1f°C"
)
st.write(f"Selected temperature: {temperature}")

st.subheader("Range Slider (Integers)")
year_range = st.slider(
    "Select a year range:",
    min_value=1990, max_value=2030, value=(2000, 2010) # Tuple for range
)
st.write(f"Selected year range: {year_range[0]} - {year_range[1]}")

st.subheader("Range Slider (Floats)")
price_range = st.slider(
    "Select a price range ($):",
    0.0, 1000.0, (100.0, 500.0), format="$%.2f"
)
st.write(f"Selected price range: {price_range[0]} to {price_range[1]}")

st.subheader("Slider with Time Objects")
appointment_time = st.slider(
    "Schedule your appointment:",
    value=(time(11, 30), time(12, 45)), # Tuple of datetime.time objects
    min_value=time(8,0),
    max_value=time(18,0),
    step=timedelta(minutes=15), # Requires from datetime import timedelta
    format="hh:mm A"
)
st.write(f"Scheduled appointment between: {appointment_time[0].strftime('%I:%M %p')} and {appointment_time[1].strftime('%I:%M %p')}")


st.subheader("Slider with Callback")
if 'slider_val' not in st.session_state:
    st.session_state.slider_val = 50

def log_slider_change():
    st.session_state.slider_val = st.session_state.my_slider # Access by key
    st.toast(f"Slider value changed to: {st.session_state.slider_val}")

st.slider(
    "Adjust sensitivity:", 0, 100, 50,
    key="my_slider",
    on_change=log_slider_change
)
st.write(f"Current sensitivity: {st.session_state.slider_val}")


st.subheader("Disabled Slider")
st.slider("Fixed value", 0, 100, value=75, disabled=True)

```

**Key Parameters:**
*   `label (str)`: A short label explaining to the user what this slider is for.
*   `min_value (number or datetime)`: The minimum allowed value for the slider.
*   `max_value (number or datetime)`: The maximum allowed value for the slider.
*   `value (number or tuple of numbers or datetime or tuple of datetimes)`: The initial value(s). For a range slider, provide a tuple of two values.
*   `step (number or timedelta)`: The increment/decrement step size. Defaults to `1` for integers, `0.01` for floats.
*   `format (str)`: A printf-style format string to customize the display of the value(s). E.g., `"%d%%"` for percentages, `"%.2f"` for two decimal places.
*   `key (str)`: An optional unique key for the widget.
*   `help (str)`: An optional tooltip.
*   `on_change (callable)`: A callback function executed when the slider value changes.
*   `args (tuple)`: Arguments for the `on_change` callback.
*   `kwargs (dict)`: Keyword arguments for the `on_change` callback.
*   `disabled (bool)`: If `True`, the slider is deactivated. Defaults to `False`.

## 8. `st.text_input()`

`st.text_input()` displays a single-line text input field. It returns the current string value entered by the user.

**Common Use Cases:**
*   Entering names, search queries, or short textual data.
*   Inputting API keys or passwords (using `type="password"`).
*   Getting simple string inputs from the user.

**Code Examples:**

```python
import streamlit as st

st.subheader("Basic Text Input")
name = st.text_input("Enter your name:", value="Your Name Here")
st.write(f"Hello, {name}!")

st.subheader("Text Input with Placeholder")
email = st.text_input("Your email address:", placeholder="example@domain.com")
if email:
    st.write(f"Email entered: {email}")

st.subheader("Password Input")
password = st.text_input("Enter your password:", type="password")
if password:
    st.write("Password received (but not displayed for security).") # Don't display actual password

st.subheader("Text Input with Max Characters")
short_code = st.text_input("Enter a 5-digit code:", max_chars=5, value="ABC")
st.write(f"Code: {short_code}")

st.subheader("Text Input with Callback (on change)")

if 'text_val' not in st.session_state:
    st.session_state.text_val = ""

def log_text_change():
    # This callback is triggered on each keystroke if `on_change` is used.
    # For submission-style behavior, often a button is used alongside text_input.
    st.session_state.text_val = st.session_state.my_text_input
    # st.toast(f"Text input changed: {st.session_state.text_val}") # Can be noisy

st.text_input(
    "Live update text:",
    key="my_text_input",
    on_change=log_text_change # Called on each keystroke
)
st.write(f"Current text: {st.session_state.text_val}")


st.subheader("Text Input with Callback (on enter)")
# To trigger callback on enter, you typically manage this with a form or custom JS.
# Streamlit's on_change for text_input is more like "on_input_change".
# For "on_enter" behavior, a common pattern is to use it within an st.form.

with st.form("on_enter_form"):
    entered_text_on_submit = st.text_input("Type something and press Enter (within form):")
    submitted = st.form_submit_button("Submit")
    if submitted:
        st.write(f"You entered and submitted: {entered_text_on_submit}")


st.subheader("Disabled Text Input")
st.text_input("Cannot edit this", value="Fixed Text", disabled=True)

```

**Key Parameters:**
*   `label (str)`: A short label explaining to the user what this input is for.
*   `value (str)`: The initial value of the text input. Defaults to `""`.
*   `max_chars (int or None)`: The maximum number of characters allowed in the input.
*   `key (str)`: An optional unique key for the widget.
*   `type (str)`: Can be `"default"` for a standard text input or `"password"` to obscure the input.
*   `help (str)`: An optional tooltip.
*   `placeholder (str or None)`: A placeholder string to display when the input is empty.
*   `on_change (callable)`: A callback function executed when the text input's value changes (typically on each keystroke or when focus is lost).
*   `args (tuple)`: Arguments for the `on_change` callback.
*   `kwargs (dict)`: Keyword arguments for the `on_change` callback.
*   `disabled (bool)`: If `True`, the text input is deactivated. Defaults to `False`.
*   `label_visibility ("visible", "hidden", or "collapsed")`: Controls the visibility of the label.

## 9. `st.text_area()`

`st.text_area()` displays a multi-line text input field. It returns the current string value entered by the user.

**Common Use Cases:**
*   Entering longer pieces of text like comments, descriptions, or code snippets.
*   Providing a larger input area for user feedback.
*   Inputting multi-line addresses or notes.

**Code Examples:**

```python
import streamlit as st

st.subheader("Basic Text Area")
feedback = st.text_area("Enter your feedback here:")
if feedback:
    st.write("Your feedback:")
    st.write(feedback)

st.subheader("Text Area with Initial Value and Height")
initial_text = "This is some pre-filled text.\nIt can span multiple lines."
description = st.text_area(
    "Product Description:",
    value=initial_text,
    height=200  # Approximate height in pixels
)
st.write("Current description:", description)

st.subheader("Text Area with Placeholder")
notes = st.text_area("Additional Notes (optional):", placeholder="Type any extra notes...")
if notes:
    st.write("Notes recorded.")

st.subheader("Text Area with Max Characters")
short_story = st.text_area("Write a very short story (max 100 chars):", max_chars=100)
st.write(f"Story length: {len(short_story)} characters.")


st.subheader("Text Area with Callback")
if 'long_text' not in st.session_state:
    st.session_state.long_text = ""

def log_text_area_change():
    st.session_state.long_text = st.session_state.my_text_area
    # st.toast("Text area updated!") # Can be noisy

st.text_area(
    "Enter your biography:",
    key="my_text_area",
    on_change=log_text_area_change,
    placeholder="Start writing your bio..."
)
if st.session_state.long_text:
    st.write("### Your Bio Preview:")
    st.write(st.session_state.long_text)


st.subheader("Disabled Text Area")
st.text_area("Cannot edit this content", value="This is read-only.", disabled=True)

```

**Key Parameters:**
*   `label (str)`: A short label explaining to the user what this input is for.
*   `value (str)`: The initial value of the text area. Defaults to `""`.
*   `height (int or None)`: The desired height of the text area in pixels. If `None`, a default height is used.
*   `max_chars (int or None)`: The maximum number of characters allowed in the input.
*   `key (str)`: An optional unique key for the widget.
*   `help (str)`: An optional tooltip.
*   `placeholder (str or None)`: A placeholder string to display when the input is empty.
*   `on_change (callable)`: A callback function executed when the text area's value changes.
*   `args (tuple)`: Arguments for the `on_change` callback.
*   `kwargs (dict)`: Keyword arguments for the `on_change` callback.
*   `disabled (bool)`: If `True`, the text area is deactivated. Defaults to `False`.
*   `label_visibility ("visible", "hidden", or "collapsed")`: Controls the visibility of the label.

## 10. `st.number_input()`

`st.number_input()` displays a field for numerical input, with optional steppers (+/-) to increment or decrement the value.

**Common Use Cases:**
*   Entering quantities, ages, scores, or any numerical data.
*   Setting parameters that require precise numerical values.
*   Inputting integer or floating-point numbers with defined bounds and steps.

**Code Examples:**

```python
import streamlit as st

st.subheader("Basic Integer Input")
quantity = st.number_input("Enter quantity:", value=1, step=1, min_value=0)
st.write(f"Selected quantity: {quantity}")

st.subheader("Float Input with Min/Max and Step")
price = st.number_input(
    "Enter price:",
    min_value=0.0,
    max_value=1000.0,
    value=9.99,
    step=0.01,
    format="%.2f"  # Display value with 2 decimal places
)
st.write(f"Selected price: ${price:.2f}")

st.subheader("Number Input without Steppers (step=None)")
# If you don't want the +/- buttons, you can sometimes omit step or set to a very small float if that's desired behavior.
# However, `step` primarily controls increment. For no steppers, it's more about the visual.
# Streamlit will generally show steppers if a step is implied or provided.
# To truly hide them, you might need CSS, which is beyond st.number_input's direct API.
# The `step` parameter is more about the increment value.
# If `value` is an int, step defaults to 1. If float, step defaults to 0.01.
year = st.number_input("Enter year:", value=2023, step=1, format="%d") # Explicit step for int
st.write(f"Year: {year}")

# If you want to allow any float without a specific step for the buttons:
# (Note: steppers will still appear with default float step e.g. 0.01)
# percentage = st.number_input("Enter percentage (0.0-1.0):", value=0.75, min_value=0.0, max_value=1.0)
# st.write(f"Percentage: {percentage}")

st.subheader("Number Input with Placeholder")
# `value` can be set to `None` to show the placeholder, but it needs a `format` string that can handle None
# or you can use the new `placeholder` parameter if your Streamlit version supports it.
# For older versions, a common pattern is to use a default value and instruct user.
age_input = st.number_input(
    "Enter your age (optional):",
    value=None,  # Allows placeholder to show
    placeholder="Type your age...",
    min_value=0,
    max_value=120,
    step=1
)
if age_input is not None:
    st.write(f"Your age: {age_input}")
else:
    st.write("Age not provided.")


st.subheader("Number Input with Callback")
if 'num_val' not in st.session_state:
    st.session_state.num_val = 0.0

def log_num_change():
    st.session_state.num_val = st.session_state.my_num_input
    st.toast(f"Number input changed to: {st.session_state.num_val}")

st.number_input(
    "Adjust parameter:",
    value=10.5,
    step=0.5,
    key="my_num_input",
    on_change=log_num_change
)
st.write(f"Current parameter value: {st.session_state.num_val}")


st.subheader("Disabled Number Input")
st.number_input("Fixed measurement", value=98.6, disabled=True, format="%.1f")

```

**Key Parameters:**
*   `label (str)`: A short label explaining to the user what this input is for.
*   `min_value (number or None)`: The minimum allowed value.
*   `max_value (number or None)`: The maximum allowed value.
*   `value (number or None or "min")`: The initial value. If `"min"`, it uses `min_value`. If `None` and `placeholder` is set, placeholder shows.
*   `step (number or None)`: The increment/decrement step size. Defaults to `1` for integers and `0.01` for floats.
*   `format (str or None)`: A printf-style format string to customize the display of the value (e.g., `"%d"` for integer, `"%.2f"` for float with 2 decimals).
*   `key (str)`: An optional unique key for the widget.
*   `help (str)`: An optional tooltip.
*   `placeholder (str or None)`: Text to display when `value` is `None`.
*   `on_change (callable)`: A callback function executed when the value changes.
*   `args (tuple)`: Arguments for the `on_change` callback.
*   `kwargs (dict)`: Keyword arguments for the `on_change` callback.
*   `disabled (bool)`: If `True`, the input is deactivated. Defaults to `False`.
*   `label_visibility ("visible", "hidden", or "collapsed")`: Controls the visibility of the label.

## 13. `st.file_uploader()`

`st.file_uploader()` displays a widget that allows users to upload files from their local system. It returns an `UploadedFile` object (or a list of them if `accept_multiple_files=True`), which has attributes like `name`, `type`, `size`, and methods like `read()`.

**Common Use Cases:**
*   Uploading CSV, Excel, or text files for data analysis.
*   Accepting images for processing or display.
*   Allowing users to submit documents or other file types.

**Code Examples:**

```python
import streamlit as st
import pandas as pd
from io import StringIO # To read text files as if they are files

st.subheader("Single File Uploader (e.g., CSV)")
uploaded_file_csv = st.file_uploader("Choose a CSV file", type="csv")

if uploaded_file_csv is not None:
    # To read file as string:
    stringio = StringIO(uploaded_file_csv.getvalue().decode("utf-8"))
    string_data = stringio.read()
    st.write("First 100 characters of the CSV file:")
    st.text(string_data[:100] + "...")

    # To read file as bytes:
    # bytes_data = uploaded_file_csv.read() # or getvalue()
    # st.write("Bytes data:", bytes_data)

    # To convert to a Pandas DataFrame:
    try:
        dataframe = pd.read_csv(uploaded_file_csv) # Reset buffer if already read
        st.write("Uploaded DataFrame:")
        st.dataframe(dataframe.head())
    except Exception as e:
        st.error(f"Error reading CSV: {e}")

    st.write(f"Filename: {uploaded_file_csv.name}")
    st.write(f"File type: {uploaded_file_csv.type}")
    st.write(f"File size: {uploaded_file_csv.size} bytes")


st.subheader("Multiple File Uploader (e.g., Images)")
uploaded_files_img = st.file_uploader(
    "Upload one or more images",
    type=["png", "jpg", "jpeg", "gif"],
    accept_multiple_files=True
)

if uploaded_files_img:
    st.write(f"Number of images uploaded: {len(uploaded_files_img)}")
    for uploaded_file in uploaded_files_img:
        st.image(uploaded_file, caption=f"Uploaded: {uploaded_file.name}", width=200)
        # To access file bytes:
        # bytes_data = uploaded_file.read()
        # st.write(f"Size of {uploaded_file.name}: {len(bytes_data)} bytes")


st.subheader("File Uploader with Callback")
if 'uploaded_filenames' not in st.session_state:
    st.session_state.uploaded_filenames = []

def handle_upload():
    if st.session_state.my_file_uploader is not None:
        # For single file uploader
        # st.session_state.uploaded_filenames = [st.session_state.my_file_uploader.name]
        # For multiple files uploader
        current_files = st.session_state.my_file_uploader
        if current_files: # Check if it's a list and not empty
             st.session_state.uploaded_filenames = [f.name for f in current_files]
        else: # Handles if it's a single file or becomes None after clearing
            st.session_state.uploaded_filenames = []
        st.toast(f"Files uploaded/changed: {st.session_state.uploaded_filenames}")
    else:
        st.session_state.uploaded_filenames = [] # Cleared
        st.toast("File uploader cleared.")


st.file_uploader(
    "Upload some documents (callback example):",
    key="my_file_uploader",
    accept_multiple_files=True,
    on_change=handle_upload,
    type=['txt', 'pdf', 'docx']
)
if st.session_state.uploaded_filenames:
    st.write("Currently tracked uploaded files:", st.session_state.uploaded_filenames)


st.subheader("Disabled File Uploader")
st.file_uploader("Cannot upload files here", disabled=True)

```

**Key Parameters:**
*   `label (str)`: A short label explaining to the user what this file uploader is for.
*   `type (str or list[str] or None)`: A string or list of strings specifying the allowed file extensions (e.g., `"csv"`, `["png", "jpg"]`). If `None`, all file types are accepted.
*   `accept_multiple_files (bool)`: If `True`, allows the user to upload multiple files. Returns a list of `UploadedFile` objects. If `False` (default), only one file can be uploaded, and it returns a single `UploadedFile` object or `None`.
*   `key (str)`: An optional unique key for the widget.
*   `help (str)`: An optional tooltip.
*   `on_change (callable)`: A callback function executed when files are uploaded or removed.
*   `args (tuple)`: Arguments for the `on_change` callback.
*   `kwargs (dict)`: Keyword arguments for the `on_change` callback.
*   `disabled (bool)`: If `True`, the file uploader is deactivated. Defaults to `False`.
*   `label_visibility ("visible", "hidden", or "collapsed")`: Controls the visibility of the label.
*   `use_container_width (bool)`: (Newer versions) If `True`, the uploader will expand to the full width of its container.
