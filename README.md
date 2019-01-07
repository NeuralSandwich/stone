# Stone

Yet another static website generator, with plugin support.

Used by (www|blog).half.systems to convert Markdown and Jinja2 into HTML.


## Installation


    pip install stone-site


## Usage

To get started with `stone`:

    # Create template layout
    stone example_site init --site-name 'example.com'
    # Generate site
    stone example_site generate
    # Add a new page
    stone example_site newpage --name "About Us"

By default Stone is configured to turn Markdown + YAML metadata + Jinja2
templates into HTML but as it has support for custom plugins it can generate
anything it has code for.

# Site Configuration

## Configuration File

Stone generates sites based on the contents of [`site.json`](docs/site-json.md).
Each site can specify which backends, generators and renderers to use, custom
versions will be loaded from the `$HOME/.stone` directory.

## Folder Structure

Stone was originally designed to generate the subdomains of half.systems.
The following is the layout of the sites:

    .
    ├── blog
    │   └── ...
    ├── main
    │   └── ...
    ├── site.json
    └── templates
        └── ...

[`site.json` is very flexiable](docs/site-json.md) about the location of
templates and files. As such your not constrained to any particular layout for
your site. You could have separate template folders inside each site or one
shared in the project root. Each site can specify its template directory


## Pages

By default pages are Markdown file, that can have optional YAML metadata that
describe the attributes of the page including the title and which template to
uses. For example:


    template: base.html
    title: Hello, World
    
    # This is a header
    
    Here is some lovely content.

Stone makes all metadata available to page templates. Any data templates may use
can be embedded into a page. For exampled: an author's name, email, the date the
page was written. The behaviour is dependent on the generator and renderers
used. See the [renderers](docs/renderers.md) and
[generators](docs/generators.md) for more details.


## Templates

By default templates are HTML pages with **[jinja2](http://jinja.pocoo.org)**
markup.

`base.html`:

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


## Generating

To generate a particular site invoke `stone` with the location of the project's
root folder.

```
stone root_folder generate
```

### Example

An example project that generates an example landing page and blog is included
in Stone's project source. you can build it by running:

    stone example generate
