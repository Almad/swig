# Tags

## Built-in Tags

### extends

Makes the current template extend a parent template. This tag must be the first item in your template.

See [Template inheritance](inheritance.md) for more information.

### block

Defines a block in a template that can be overridden by a template extending this one and/or will override the current template's parent template block of the same name.

See [Template inheritance](inheritance.md) for more information.

### parent

Inject the content from the current `block` in the parent template into the child template.

See [Template inheritance](inheritance.md) for more information.

### include

Includes a template in it's place. The template is rendered within the current context. Does not use and {% endinclude %}.

    {% include template_path %}
    {% include "path/to/template.js" %}

### for

For loops have 4 special context variables accessible inside of the loop:

    {% for x in y %}
        {% if forloop.first %}<ul>{% endif %}
        <li>{% forloop.index %} - {% forloop.key %}: {{ x }}</li>
        {% if forloop.last %}</ul>{% endif %}
    {% endfor %}

* `forloop.index`: the zero-indexed spot in the iterator.
* `forloop.key`: if the iterator is an object, this will be the key of the current item, otherwise it will be the same as the `forloop.index`.
* `forloop.first`: `true` if the current object is the first in the object or array.
* `forloop.last`: `true` if the current object is the last in the object or array.

You can also apply filters to the object that you are iterating over.

    {% for x in y|reverse %}
        The array `y` will first be reversed before looping over it.
    {% endfor %}

#### empty

For loops have a special tag available to them called `{% empty %}`.

If the loop object is empty or `length === 0`, the content following the `empty` tag will be rendered.

    {% for person in people %}
        {{ person }}
    {% empty %}
        There are no people yet!
    {% endfor %}

### if

Supports the following expressions.

    {% if x %}{% endif %}
    {% if !x %}{% endif %}
    {% if x operator y %}
        Operators: ==, !=, <, <=, >, >=, ===, !==, in
        The 'in' operator checks for presence in arrays too.
    {% endif %}
    {% if x == 'five' %}
        The operands can be also be string or number literals
    {% endif %}
    {% if x|length === 3 %}
        You can use filters on any operand in the statement.
    {% endif %}

#### else and else if

Also supports using the `{% else %}` and `{% else if ... %}` tags within an if-block. Using `{% else [if ...] %}` anywhere outside an immediate parent if-block will throw an exception.

    {% if foo %}
        Some content.
    {% else if "foo" in bar %}
        Content if the array `bar` has "foo" in it.
    {% else %}
        Fallback content.
    {% endif %}

### autoescape

The `autoescape` tag accepts one of two controls: `on` or `off` (default is `on` if not provided). These either turn variable auto-escaping on or off for the contents of the filter, regardless of the global setting.

For the following contexts, assume:

    some_html_output = '<p>Hello "you" & \'them\'</p>';

So the following:

    {% autoescape off %}
        {{ some_html_output }}
    {% endautoescape %}

    {% autoescape on %}
        {{ some_html_output }}
    {% endautoescape %}

Will output:

    <p>Hello "you" & 'them'</p>

    &lt;p&gt;Hello &quot;you&quot; &amp; &#39;them&#39; &lt;/p&gt;

### set

It is also possible to set variables in templates.

    {% set foo = [0, 1, 2, 3, 4, 5] %}
    {% for num in foo %}
        <li>{{ num }}</li>
    {% endfor %}

### macro

Macros are custom, reusable methods for content-generation that are defined in templates.


#### Example

One of the most common use-case for macros is form inputs. To start, define your `input` macro somewhere in your template scope (the top of a template, or an included template is a good spot):

    {% macro input type name id label value error %}
        <label for="{{ name }}">{{ label }}</label>
        <input type="{{ type }}" name="{{ name }}" id="{{ id }}" value="{{ value }}"{% if error %} class="error"{% endif %}>
    {% endmacro %}

Somewhere later in your template, invoke the macro using a variable:

    <div>{{ input("text", "fname", "fname", "First Name", fname.value, fname.errors) }}</div>
    <div>{{ input("text", "lname", "lname", "Last Name", lname.value, lname.errors) }}</div>

Your output may look like this:

    <div>
        <label for="fname">First Name</label>
        <input type="text" name="fname" id="fname" value="Paul">
    </div>
    <div>
        <label for="lname">Last Name</label>
        <input type="text" name="lname" id="lname" value="" class="error">
    </div>

## Custom Tags

Swig has support for you to write your own custom tags. For more information, see the [Custom Tags Documentation](custom-tags.md).