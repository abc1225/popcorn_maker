=========
Templates
=========

Templates are the starting point for generating Popcorn projects, they load Butter and allow the user to save their data.


Templates Components
====================

Templates are composed by at least three components:

- An HTML page
- A JSON Butter configuration file
- A JSON with default metadata

Any other media part of the template is optional.

If you want to have a look at an example is available at https://github.com/alfredo/popcorn_gallery_template

Templates can be added via the admin interface or bulk upload. It is recomended to import your template like this if your template has several assets.

These are the instructions to prepare a template for upload:

- A template can be bulk uploaded with a zip file, the files must be contained in a folder with the template name.
- The slugified name of the zip file will be used as the name of the template.
- All the valid assets will be listed and added to the build.
- Any supplied asset with the template must use relative paths.
- Non relative paths is recommended to use absolute URLS.
- There is no need for the ``base`` tag.
- All templates butter initialization must call ``config`` as the default data value.
- The ``config`` URL is automatically populated with the PM site required values
  e.g.::

    {
     "savedDataUrl": "data",
     "baseDir": "/static/",
     "name": "default-butter"
    }


Adding templates
================

Only admins/superusers are allowed to add templates to the gallery.
You must create a superuser as specified in the above step.

Once that has been done, visit::

http://local.mozillapopcorn.org/admin

In your browser and login to the Django Admin interface.

There you can choose to add a template file by file, or import a zip file.
A sample zipped template can be found at https://github.com/alfredo/popcorn_gallery_template

Now you must set each file to the proper type:

- Config
- Default track events
- Static assets
- Template html

Once that has all ben set, click the publish checkbox at the bottom of the page, then save your changes.


Template Assets
===============

Template assets could be hosted in the server, provided as part of the template or hosted somewhere else on the internet.




Template Sanitation
===================

There are two points where the user generated content is added to the PM site, the templates and the projects

At the moment the templates are only added by trusted users, so the rules to do this are fairly relaxed, they are procesed to make sure the media paths are correct and that things fall into place once they are saved in the server.

The HTML and JSON stream served to the templates are preprocessed on save and the sanitation mechanics work pipe-like so there is room sanitize further this content in the future when this is open to more users.

The templates are imported in bulk and filters the bundled files imported by extension, they are saved using a Template specific storage in order to isolate the user generated content.

The butter projects are generated using the templates as a base, and they are populated using user specific metadata.

The metadata saved is sanitized using bleach for any string content. This as well as a pipe like we can add extra validation If we want to.

The final html export of the project is generated using the template and the saved metadata in the server side.
