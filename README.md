# stone
Yet another static website generator.

Used by half.systems

# Installation

```
pip install stone-site
```

# Usage
You may define site structures within the **site.json** file. The file should
contain an object that holds a list
of site definitions in the following format:

```json
{
  "sites": [
    {
      "site": "example.com",
      "pages": [
        {
          "page_type": "index",
          "source": "someindex.md",
          "target": "myrenderedindex.html",
        },
        {
          "page_type": "post",
          "source": "mypage.md",
          "target": "myrenderedpage.html",
          "redirects": ["old/location/myrenderedpage.html"]
        }
      ],
      "templates": ["site/templates", "blog/templates"]
    },
      ...
]}
```

## Folder Structure

Site projects can be structured as you wish.

The layout which stone was developed along side is:

* root
  * blog
  * main
  * templates
    * templated HTML for blog and main
  * site.json

As `site.json` is explicit about the location of templates and files, the
structure is flexible. You could locate separate template folder inside each
site or have one giant mess in your project root.

## Pages

The source markdown files should consist of simple markdown with a YAML header
that describe the attributes of the generated page including the page title and
the template it uses. For example:


```
template: base.html
title: TEST

# This is a header

Here is so lovely content.
```

There are additional attributes:

* date - Adds the date the page was create to the page metadata. This is
  currently used when generating indexes for blogs. Format YYYY-MM-DD

## Templates

Templates support **jinja2**, an example:

`base.html`:

```
<html>
  <head>
    {% block head %}
    <title>{{ title }}</title>
    {% endblock %}
  <head>
  <body>
  {% block body %}
    <h1>{{ title }}</title>
    <div id="post">
      <!-- Most likely we are going to pass more html here --->
      {{ content|safe }}
    </div>
  {% endblock %}
  </body>
</html>
```

## Generating

To generate a particular site invoke `stone.py` with the location of the
project's root folder.

```
python stone.py root_folder
```

### Example

An example project that generates an html version of this README can be found in
the example folder.

You can build it by running:

```
python stone.py example
```