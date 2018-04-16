PDF to Image
============

PDF to Image create images from PDF files automatically. This is useful if you'd
like to display a preview of the first page of your uploaded PDF, or showcase a
gallery of all the pages in a PDF file.

This module utilizes the
[Imagick PHP extension](https://secure.php.net/manual/en/book.imagick.php) for
the processing of PDF files to images, and is inspired by (but not a direct port
of) Drupal's
[PDF to ImageField](https://www.drupal.org/project/pdf_to_imagefield) module.

Installation
------------

- Make sure the
  [Imagick PHP extension](https://secure.php.net/manual/en/book.imagick.php) is
  installed on your server.

  On a Debian-based system, this is as simple as running:
  `sudo apt-get install imagemagick php5-imagick`.  
  Note that support for installing Imagick is not provided by this module or its
  maintainer(s).

- Install this module using the official Backdrop CMS instructions at
  https://backdropcms.org/guide/modules.

- Add an Image field to your entity to store the generated image(s). Make sure
  the image format you wish to use is in the 'Allowed file extensions' setting.
  Set the 'Number of values' to the maximum number of pages you wish to store
  (note that this may end up being less, depending how many pages the PDF has).

- Add a File field to your entity and set the widget type to 'File'. Select the
  image field you added above in the 'Image field' setting, then configure the
  other settings to your preference. Make sure 'pdf' is added to the 'Allowed
  file extensions' setting.  
  Note that you can set the 'Number of values' of the file field to whatever you
  like, but only the first file uploaded will be processed by this module.

- Create an entity, upload a PDF file, and the image(s) will be generated on
  save.

Issues
------

Bugs and Feature requests should be reported in the Issue Queue:
https://github.com/backdrop-contrib/pdf_to_image/issues.

Current Maintainers
-------------------

- Peter Anderson (https://github.com/BWPanda).

Credits
-------

- Inspired by Drupal's 'PDF to Imagefield' module
  (https://www.drupal.org/project/pdf_to_imagefield).

License
-------

This project is GPL v2 software. See the LICENSE.txt file in this directory for
complete text.

