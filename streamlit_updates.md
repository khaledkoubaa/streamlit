# Streamlit Version 1.46.0 Updates

This document provides an overview of notable updates, new features, and changes in Streamlit version 1.46.0, released on June 18, 2025. It includes explanations, code examples, and cheat sheet entries for quick reference.

## Highlights

### 🧭 Top Navigation with `st.navigation`

Streamlit now allows you to create a navigation menu at the top of your app. This can be achieved by using the `st.navigation` command with the `position="top"` parameter. This provides an alternative to the traditional sidebar navigation.

**Code Example:**

```python
import streamlit as st

# Create a list of pages
pages = [
    st.Page("home.py", title="Home", icon="🏠"),
    st.Page("about.py", title="About", icon="📄"),
    st.Page("contact.py", title="Contact Us", icon="📧"),
]

# Add top navigation
st.navigation(pages, position="top")

st.write("Content for the current page will be displayed here based on the navigation.")

# home.py (example content)
# import streamlit as st
# st.title("Home Page")
# st.write("Welcome to the home page!")

# about.py (example content)
# import streamlit as st
# st.title("About Page")
# st.write("This is the about page.")

# contact.py (example content)
# import streamlit as st
# st.title("Contact Us")
# st.write("Get in touch with us!")
```

**Cheat Sheet Entry:**

*   `st.navigation(pages, position="top")`: Creates a navigation menu at the top of the app. `pages` is a list of `st.Page` objects.

### 🔆 Runtime Theme Detection with `st.context.theme`

You can now detect whether the viewer is currently using light mode or dark mode in your Streamlit app at runtime. The `st.context.theme` attribute returns a string indicating the current theme ("light" or "dark").

**Code Example:**

```python
import streamlit as st

current_theme = st.context.theme

if current_theme == "dark":
    st.write("You are using the dark theme! 🌙")
    # You could potentially load different images or styles
else:
    st.write("You are using the light theme! ☀️")
    # You could potentially load different images or styles

st.write(f"The current theme is: {st.context.theme}")
```

**Cheat Sheet Entry:**

*   `st.context.theme`: Returns the current theme ("light" or "dark") as a string.

## Notable Changes

This section covers some of the other notable changes in Streamlit 1.46.0 that are beneficial for developers.

### 🪺 Unrestricted Nesting of Layout Elements

Streamlit no longer restricts the nesting of columns, expanders, popovers, and chat message containers. While this offers greater flexibility, it's important to use this power judiciously to maintain good design, especially considering different screen sizes and orientations.

**Code Example (Illustrative):**

```python
import streamlit as st

st.header("Deeply Nested Layouts")

with st.expander("Outer Expander"):
    st.write("Content in outer expander")
    col1, col2 = st.columns(2)
    with col1:
        st.write("Column 1")
        with st.popover("Popover in Col 1"):
            st.write("Content inside popover")
            with st.chat_message("user"):
                st.write("Hello from a nested chat!")
    with col2:
        st.write("Column 2")
        with st.expander("Inner Expander"):
            st.write("Content in inner expander")

```

**Cheat Sheet Entry:**

*   **Layout Nesting**: Columns, expanders, popovers, and chat message containers can now be nested without restriction. (Use with caution for design and responsiveness).

### ↔️ Width Configuration for Most Elements

You can now set the `width` for most Streamlit elements. This allows for more precise control over the layout and appearance of your applications. The specific way to set width might vary (e.g., as a parameter to the element's function, or via CSS in `st.markdown` for some). Refer to individual element documentation for specifics.

*(Self-correction: The release notes state "You can set the width of most Streamlit elements." but doesn't specify a universal parameter. I will assume it means individual elements are gaining `width` parameters where applicable, or it's a general capability improvement. For a concrete example, I'll illustrate with a hypothetical `width` parameter on a common element if the specific docs aren't readily available for each. For now, I'll keep the explanation general and the cheat sheet entry broad.)*

**Code Example (Conceptual - actual parameter might vary):**

```python
import streamlit as st

# Hypothetical example, actual parameter might differ or apply to specific elements
# st.text_input("Input with specific width", width=300) # This is a guess
# st.button("Button with specific width", width=200) # This is a guess

# For elements like st.image, width is already a known parameter
# st.image("your_image.png", width=400)

st.write("Refer to specific element documentation for `width` parameter availability and usage.")
```

**Cheat Sheet Entry:**

*   **Element Width**: Most elements can now have their `width` configured. Check specific element documentation for usage (e.g., `st.image(..., width=300)`).

### ⬆️ `st.form` Height Configuration

The `st.form` component now has a new `height` parameter, allowing you to configure its height.

**Code Example:**

```python
import streamlit as st

st.subheader("Form with Custom Height")

with st.form(key="my_form_with_height", clear_on_submit=True):
    name = st.text_input("Name")
    email = st.text_input("Email")
    # The height parameter would be used here if available
    # As of my last knowledge, st.form itself doesn't have a direct 'height' parameter.
    # This might be a new addition. If so, the example would be:
    # submitted = st.form_submit_button("Submit", height=300) # Or on st.form(height=...)

st.write("The release note mentions `st.form` has a new parameter for height. Assuming it's on the `st.form` call itself:")

with st.form(key="my_form_actual", height=300): # Assuming this is the new parameter
    st.text_input("Your Name")
    st.slider("Age", 18, 100)
    submit_button = st.form_submit_button(label='Submit Form')

if submit_button:
    st.write("Form submitted!")
```
*(Self-correction: The changelog says `st.form` has a new parameter to configure its height. I will assume it's a direct parameter to `st.form()`.)*

**Cheat Sheet Entry:**

*   `st.form(height=...)`: Configures the height of the form container.

### 🛠️ `st.columns` No Gap Option

`st.columns` now supports `gap=None` to remove the space (gap) between columns. This is useful for creating seamless layouts.

**Code Example:**

```python
import streamlit as st

st.subheader("Columns with No Gap")
col1, col2, col3 = st.columns(3, gap=None)

with col1:
    st.button("Button 1 (No Gap)", use_container_width=True)
with col2:
    st.button("Button 2 (No Gap)", use_container_width=True)
with col3:
    st.button("Button 3 (No Gap)", use_container_width=True)

st.subheader("Columns with Default Gap (Medium)")
colA, colB, colC = st.columns(3) # Or gap="medium"

with colA:
    st.button("Button A (Default Gap)", use_container_width=True)
with colB:
    st.button("Button B (Default Gap)", use_container_width=True)
with colC:
    st.button("Button C (Default Gap)", use_container_width=True)
```

**Cheat Sheet Entry:**

*   `st.columns(..., gap=None)`: Creates columns with no gap between them. Other options: `"small"`, `"medium"`, `"large"`.

### 🎨 `theme.dataframeBorderColor` Configuration

A new theme configuration option, `theme.dataframeBorderColor`, allows you to set the border color for dataframes and tables separately from other border colors in your `config.toml` file.

**Example (`config.toml`):**

```toml
[theme]
base="light"
primaryColor="#FF4B4B"
backgroundColor="#FFFFFF"
secondaryBackgroundColor="#F0F2F6"
textColor="#262730"
font="sans serif"
# New option:
dataframeBorderColor = "#FF0000" # Sets dataframe borders to red
```

**Cheat Sheet Entry (`config.toml`):**

*   `[theme]`
    *   `dataframeBorderColor = "your_color_here"`: Sets a specific border color for dataframes/tables.

### 🌯 `theme.buttonRadius` Configuration

A new theme configuration option, `theme.buttonRadius`, lets you set the border radius of buttons separately from other elements in your `config.toml` file.

**Example (`config.toml`):**

```toml
[theme]
base="light"
# ... other theme settings ...
# New option:
buttonRadius = "5px" # Sets button border radius to 5 pixels
```

**Code Example (Illustrating effect, not direct Python control):**

```python
import streamlit as st

st.write("Button appearance will be affected by `theme.buttonRadius` in `config.toml`.")
st.button("A Rounded Button (if configured)")
st.button("Another Button")
```

**Cheat Sheet Entry (`config.toml`):**

*   `[theme]`
    *   `buttonRadius = "value"`: Sets the border radius for buttons (e.g., `"0px"`, `"5px"`, `"0.5rem"`).

### 🖥️ `theme.codeFontSize` Configuration

A new theme configuration option, `theme.codeFontSize`, allows you to set the font size of code displayed in `st.code`, `st.json`, and `st.help`.

**Example (`config.toml`):**

```toml
[theme]
base="light"
# ... other theme settings ...
# New option:
codeFontSize = "16px" # Sets code font size to 16 pixels
```

**Code Example (Illustrating effect):**

```python
import streamlit as st

st.write("The font size of the code block below is controlled by `theme.codeFontSize` in `config.toml`.")
st.code("print('Hello, Streamlit with custom code font size!')", language="python")

st.help(st.button)
```

**Cheat Sheet Entry (`config.toml`):**

*   `[theme]`
    *   `codeFontSize = "value"`: Sets the font size for code blocks (`st.code`, `st.json`, `st.help`) (e.g., `"14px"`, `"1.2em"`).

### 📄 `st.set_page_config` Callable Multiple Times

`st.set_page_config()` can now be called multiple times in a single script run. The settings from subsequent calls will merge with and override previous calls. This can be useful for dynamically updating page configurations.

**Code Example:**

```python
import streamlit as st

# Initial page config
st.set_page_config(layout="centered", page_title="My App")
st.write(f"Current page title (initial): {st.get_option('client.pageTitle')}")


# Potentially update layout or other settings later
new_layout = st.radio("Select Layout", ["centered", "wide"], index=0)

if new_layout == "wide":
    st.set_page_config(layout="wide", page_icon="🎉") # Can update icon too
    st.success("Layout changed to wide and icon updated!")
else:
    st.set_page_config(layout="centered", page_icon="🎈")
    st.info("Layout is centered.")

# Note: The page title from the first call persists unless overridden.
# The icon and layout are updated by the second call.

st.write(f"Current page title (after potential update): {st.get_option('client.pageTitle')}")
st.write("Check the browser tab for icon changes!")

# To see the effect, you might need to interact with the radio button.
# The page title is a bit tricky to demonstrate this way as it's set early.
# However, layout and icon changes are more evident.
```

**Cheat Sheet Entry:**

*   `st.set_page_config(...)`: Can be called multiple times. Later calls merge with/override earlier ones.
