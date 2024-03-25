# Jinja2-Tutorial
Jinja2 Python tutorial for those who work with HTML or XML
## Introduction

Jinja2 is a powerful and popular templating engine for Python, commonly used in web development frameworks like Flask and Django. It allows developers to embed Python-like expressions and control structures directly within HTML or other text-based files to generate dynamic content.

## Key Concepts

1. **Variable Substitution**: Variables in Jinja2 are enclosed within double curly braces {{ ... }}. When the template is rendered, these placeholders are replaced with the corresponding values passed from the Python code.

```python
# Example of variable substitution in Jinja2
from jinja2 import Template

name = "John"
template_string = "Hello, {{ name }}!"
template = Template(template_string)
rendered_template = template.render(name=name)
print(rendered_template)
  # Output: Hello, John!
```

2. **Expressions**: Jinja2 supports a range of expressions similar to Python. These expressions can be simple variables, or more complex expressions involving arithmetic, comparisons, etc.
```jinja
{% if user.is_authenticated %}
    <p>Welcome, {{ user.username }}!</p>
{% else %}
    <p>Please log in to continue.</p>
{% endif %}

```

3. **Control Structures**: Jinja2 supports common control structures like if, for, else, elif, etc., which are enclosed within {% ... %}
```jinja
{% for item in shopping_list %}
    <li>{{ item }}</li>
{% endfor %}
```

4. **Filters**: Jinja2 provides filters that modify variables before they're displayed. Filters are appended to variables using the pipe symbol |
```jinja
<p>{{ user_input|capitalize }}</p>

```
Common filters: 
* safe: Marks the value as safe from escaping, useful when you trust the source of the data.
* escape: Escapes special characters in a string.
* capitalize: Capitalizes the first character of a string.
* lower: Converts a string to lowercase.
* upper: Converts a string to uppercase.
* title: Converts the first character of each word to uppercase.
* truncate: Truncates a string to a specified length.
* length: Returns the length of an object.
* default: Sets a default value if the variable is undefined.
* join: Joins a list of strings with a separator.

5. **Comments**: Comments in Jinja2 templates are enclosed within {# ... #}.
```jinja
{# This is a comment #}
```

6. **Template Inheritance**: Jinja2 supports template inheritance, allowing you to define a base template with common elements and then extend or override specific parts in child templates.


```jinja
# Base Template (base_template.html)
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}My Website{% endblock %}</title>
</head>
<body>
    <div id="header">
        <!-- Common header content goes here -->
    </div>
    
    <div id="content">
        {% block content %}
        <!-- Child templates will override this block -->
        {% endblock %}
    </div>
    
    <div id="footer">
        <!-- Common footer content goes here -->
    </div>
</body>
</html>
```
```jinja
# Child Template (child_template.html)

{% extends "base.html" %}

{% block title %}Welcome to My Website{% endblock %}

{% block content %}
    <h1>Welcome to My Website</h1>
    <p>This is some unique content for this page.</p>
{% endblock %}

```
Python Code
```python
from jinja2 import Environment, FileSystemLoader

# Step 1: Create Jinja2 Environment with template folder path
env = Environment(loader=FileSystemLoader('.'))  # Assuming templates are in the current directory

# Step 2: Load the child template
template = env.get_template('child_template.html')

# Step 3: Render the template
rendered_output = template.render()

# Step 4: Output or use the rendered template
print(rendered_output)
```

```html
# Rendered Output (child.html)

<!DOCTYPE html>
<html>
<head>
    <title>Welcome to My Website</title>
</head>
<body>
    <div id="header">
        <!-- Common header content goes here -->
    </div>
    
    <div id="content">
        <h1>Welcome to My Website</h1>
        <p>This is some unique content for this page.</p>
    </div>
    
    <div id="footer">
        <!-- Common footer content goes here -->
    </div>
</body>
</html>
```
<img src="Images/Website_Screenshot.png" alt="isolated" width="600"/>

## Example usage

```jinja
# XML Template
<?xml version="1.0" encoding="UTF-8"?>
<catalog>
    {% for book in books %}
    <book>
        <title>{{ book.title }}</title>
        <author>{{ book.author }}</author>
        <year>{{ book.year }}</year>
        {% if book.publisher %}
        <publisher>{{ book.publisher }}</publisher>
        {% endif %}
        {% if book.price %}
        <price>{{ book.price }}</price>
        {% endif %}
        {% if book.available %}
        <availability>
            {% if book.available %}
            <status>Available</status>
            {% else %}
            <status>Not Available</status>
            {% endif %}
            <quantity>{{ book.quantity }}</quantity>
        </availability>
        {% endif %}
    </book>
    {% endfor %}
</catalog>
```
```python
from jinja2 import Environment, FileSystemLoader
import xml.etree.ElementTree as ET

# Define a Book class
class Book:
    def __init__(self, title, author, year, publisher=None, price=None, available=False, quantity=0):
        self.title = title
        self.author = author
        self.year = year
        self.publisher = publisher
        self.price = price
        self.available = available
        self.quantity = quantity

# Sample data
books_data = [
    Book("Book 1", "Author 1", 2000, publisher="Publisher 1", price=20.99, available=True, quantity=10),
    Book("Book 2", "Author 2", 2010, publisher="Publisher 2", price=15.99, available=False),
    Book("Book 3", "Author 3", 2020, available=True, quantity=5)
]

# Initialize Jinja environment
template_loader = FileSystemLoader(searchpath="./")
env = Environment(loader=template_loader)

# Load the Jinja template
template = env.get_template("template.xml.j2")

# Render the template with data
xml_content = template.render(books=books_data)

# Parse the rendered XML content
root = ET.fromstring(xml_content)

# Print the generated XML
xml_string = ET.tostring(root, encoding="utf-8").decode("utf-8")
print(xml_string)

```
```XML
#Output
<?xml version="1.0" encoding="UTF-8"?>
<catalog>
    <book>
        <title>Book 1</title>
        <author>Author 1</author>
        <year>2000</year>
        <publisher>Publisher 1</publisher>
        <price>20.99</price>
        <availability>
            <status>Available</status>
            <quantity>10</quantity>
        </availability>
    </book>
    <book>
        <title>Book 2</title>
        <author>Author 2</author>
        <year>2010</year>
        <publisher>Publisher 2</publisher>
        <price>15.99</price>
        <availability>
            <status>Not Available</status>
            <quantity>0</quantity>
        </availability>
    </book>
    <book>
        <title>Book 3</title>
        <author>Author 3</author>
        <year>2020</year>
        <availability>
            <status>Available</status>
            <quantity>5</quantity>
        </availability>
    </book>
</catalog>

```
