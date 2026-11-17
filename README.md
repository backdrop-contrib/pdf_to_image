PDF to Image
============

PDF to Image creates images from PDF files automatically. This is useful if
you'd like to display a preview of the first page of an uploaded PDF, or
showcase a gallery of all the pages in a PDF file.

This module uses the
[Imagick PHP extension](https://www.php.net/manual/en/book.imagick.php) to
convert PDF files to images, and is inspired by (but not a direct port of)
Drupal's [PDF to ImageField](https://www.drupal.org/project/pdf_to_imagefield)
module.

Requirements
------------

- The [Imagick PHP extension](https://www.php.net/manual/en/book.imagick.php).
- [Ghostscript](https://www.ghostscript.com/). ImageMagick hands PDF rendering
  over to Ghostscript and cannot read PDF files without it.

On a Debian-based system, install everything with:

```
sudo apt-get install imagemagick php-imagick ghostscript
```

Note that support for installing Imagick is not provided by this module or
its maintainers.

Installation
------------

1. Install the requirements listed above.

2. Make sure ImageMagick is allowed to read PDF files. On Debian and Ubuntu,
   the ImageMagick security policy blocks PDF by default. Edit
   `/etc/ImageMagick-6/policy.xml` (or `/etc/ImageMagick-7/policy.xml`) and
   change this line:

   ```
   <policy domain="coder" rights="none" pattern="PDF" />
   ```

   to:

   ```
   <policy domain="coder" rights="read|write" pattern="PDF" />
   ```

   Then restart PHP.

3. Install this module using the official Backdrop CMS instructions at
   https://backdropcms.org/guide/modules.

4. Check the status report at *Reports > Status report*
   (`admin/reports/status`). The *Imagick* entry shows whether the PHP
   extension is enabled.

Configuration
-------------

1. Add an **Image field** to your entity to store the generated image(s). Make
   sure the image format you wish to use is listed under *Allowed file
   extensions*. Set *Number of values* to the maximum number of pages you wish
   to store (fewer images may be generated, depending on how many pages the
   PDF has).

2. Add a **File field** to your entity and set the widget type to *File*.
   Select the image field from step 1 in the *Image field* setting, then
   configure the other settings to your preference. Make sure `pdf` is listed
   under *Allowed file extensions*.

   You can set *Number of values* of the file field to whatever you like, but
   only the first uploaded file will be processed by this module.

3. Create an entity and upload a PDF file. The image(s) will be generated on
   save.

### Settings

The **PDF to image** settings of the file field offer these options:

- *Image field*: The image field that receives the generated images. Select
  *Disabled* to turn the module off for this file field.
- *Hide image field*: Removes the image field from edit forms, so that images
  can't be uploaded there by hand.
- *Image format*: JPG, GIF or PNG.
- *Alt text mode* and *Alt text token pattern*: See
  [Alt text for generated images](#alt-text-for-generated-images).
- *Options > Resolution*: The rendering resolution in dots per inch. It is not
  the image size in pixels. At 150, a page of 8 × 12 inches becomes an image of
  1200 × 1800 pixels.
- *Options > Trim*: Crops the edges of each image that have the same color as
  the background.

Alt text for generated images
-----------------------------

*Available since version
[1.x-1.2.0-beta1](https://github.com/backdrop-contrib/pdf_to_image/releases/tag/1.x-1.2.0-beta1).*

You can configure alternative (alt) text for the generated images to meet
accessibility standards such as WCAG. This is particularly useful when
*Hide image field* is selected, since alt text can't be added manually in that
case.

### Alt text modes

In the **PDF to image** settings of the file field, select an **alt text
mode**:

- *Leave Alone / None* (default): Newly generated images get an empty alt
  attribute, and the alt text of existing images is never changed. Use this if
  you manage alt text by hand.
- *Entity Title / Name*: Uses the title of the parent entity, or its name as
  a fallback.
- *Source PDF Filename*: Uses the file name of the source PDF, without
  extension.
- *Custom Token Pattern*: Uses a pattern of your own with tokens, e.g.
  `[node:title]`. A token browser is provided.

HTML tags are removed from the alt text, and it is shortened to 512
characters.

### Updating existing content

With any mode other than *Leave Alone / None*, resaving an entity updates the
alt text of its existing generated images. The PDF is not converted again, so
this is fast.

To apply a changed alt text mode to all existing content at once, use the
**Update existing image alt text** button on the file field settings form. It
saves the settings and then runs a batch process that resaves each entity
with a file in this field. Keep in mind:

- The button appears once an image field has been selected and saved, and it
  is hidden while the mode is *Leave Alone / None*.
- The entities are really saved. Their "changed" time is updated, and other
  modules that act on save run as well.
- Entities that have a PDF but no images yet get their images generated
  during that save.

Notes
-----

- **File names:** Generated images are stored in the directory configured on
  the image field and are named `<pdf file name>-<page>.<format>`, with pages
  counted from 0. Existing files with the same name are replaced, so two PDFs
  with the same file name overwrite each other's images.
- **Replacing or removing the PDF:** Uploading a different PDF regenerates the
  images. Removing the PDF clears the image field.
- **Manually uploaded images:** Images uploaded by hand into the image field
  are never overwritten, as long as the entity had no generated images before.
- **Conversion errors:** If rendering fails, for example because of a damaged
  PDF, missing Ghostscript or the PHP memory limit, the entity is still saved.
  The error is written to the log (type `pdf_to_image`) and shown as a
  message.

Troubleshooting
---------------

- **The log shows "Failed to read the file":** Ghostscript is missing, or the
  ImageMagick security policy blocks PDF. See steps 1 and 2 under
  *Installation*.
- **Only some pages are generated:** Increase *Number of values* on the image
  field. The module never generates more images than the field can hold.

Issues
------

Please report bugs and feature requests in the issue queue:
https://github.com/backdrop-contrib/pdf_to_image/issues

Current Maintainers
-------------------

- [Alexander Wanke](https://github.com/a8w4)

Credits
-------

- Written for Backdrop CMS by [Peter Anderson](https://github.com/BWPanda).
- Inspired by Drupal's
  [PDF to ImageField](https://www.drupal.org/project/pdf_to_imagefield) module.
- Additional contributions by
  [Steve (albanycomputers)](https://github.com/albanycomputers).

License
-------

This project is GPL v2 software. See the LICENSE.txt file in this directory
for the complete text.
