.. include:: ../header.rst

.. _Annot:

================
Annot
================


|pdf_only_class|

Quote from the :ref:`AdobeManual` : *"An annotation associates an object such as a note, sound, or movie with a location on a page of a PDF document, or provides a way to interact with the user by means of the mouse and keyboard."*

There is a parent-child relationship between an annotation and its page. If the page object becomes unusable (closed document, any document structure change, etc.), then so does every of its existing annotation objects -- an exception is raised saying that the object is "orphaned", whenever an annotation property or method is accessed.

================================== ==============================================================
**Attribute**                      **Short Description**
================================== ==============================================================
:meth:`Annot.CleanContents`        PDF only: clean the annot's :data:`contents` object
:meth:`Annot.DeleteResponses`      Delete all responding annotions
:meth:`Annot.GetFile`              Get attached file content
:meth:`Annot.GetOC`                Get :data:`xref` of an :data:`OCG` / :data:`OCMD`
:meth:`Annot.GetPixmap`            Image of the annotation as a pixmap
:meth:`Annot.GetSound`             Get the sound of an audio annotation
:meth:`Annot.GetText`              Extract annotation text
:meth:`Annot.GetTextbox`           Extract annotation text
:meth:`Annot.GetTextPage`          Extract text page 
:meth:`Annot.SetApnBbox`           Set annotation appearance bbox
:meth:`Annot.SetApnMatrix`         Set annotation appearance matrix
:meth:`Annot.SetBorder`            Set annotation's border properties
:meth:`Annot.SetBlendMode`         Set annotation's blend mode
:meth:`Annot.SetColors`            Set annotation's colors
:meth:`Annot.SetFlags`             Set annotation's flags field
:meth:`Annot.SetIrtXRef`           Define the annotation to being "In Response To"
:meth:`Annot.SetName`              Set annotation's name field
:meth:`Annot.SetOC`                Set :data:`xref` to an :data:`OCG` / :data:`OCMD`
:meth:`Annot.SetOpacity`           Change transparency
:meth:`Annot.SetOpen`              Open / close annotation or its Popup
:meth:`Annot.SetPopup`             Create a Popup for the annotation
:meth:`Annot.SetRect`              Change annotation rectangle
:meth:`Annot.SetRotation`          Change rotation
:meth:`Annot.UpdateFile`           Update attached file content
:meth:`Annot.Update`               Apply accumulated annot changes
:attr:`Annot.ApnBbox`              Annotation appearance bbox
:attr:`Annot.ApnMatrix`            Annotation appearance matrix
:attr:`Annot.BlendMode`            Annotation BlendMode
:attr:`Annot.Border`               Border details
:attr:`Annot.Colors`               Border / background and fill colors
:attr:`Annot.FileInfo`             Get attached file information
:attr:`Annot.Flags`                Annotation flags
:attr:`Annot.HasPopup`             Whether annotation has a Popup
:attr:`Annot.IrtXref`              Annotation to which this one responds
:attr:`Annot.Info`                 Various information
:attr:`Annot.IsOpen`               Whether annotation or its Popup is open
:attr:`Annot.LineEnds`             Start / end appearance of line-type annotations
:attr:`Annot.Next`                 Link to the next annotation
:attr:`Annot.Opacity`              The annot's transparency
:attr:`Annot.Parent`               Page object of the annotation
:attr:`Annot.PopupRect`            Rectangle of the annotation's Popup
:attr:`Annot.PopupXref`            The PDF :data:`xref` number of the annotation's Popup
:attr:`Annot.Rect`                 Rectangle containing the annotation
:attr:`Annot.RectDelta`            Annotation delta values to rectangle
:attr:`Annot.Rotation`             Annotation rotation
:attr:`Annot.Type`                 Type of the annotation
:attr:`Annot.Vertices`             Point coordinates of Polygons, PolyLines, etc
:attr:`Annot.Xref`                 The PDF :data:`xref` number
================================== ==============================================================

**Class API**

.. class:: Annot

   .. method:: CleanContents(int sanitize: 1)

      Clean the :data:`contents` streams associated with the annotation. This is the same type of action which :meth:`Page.CleanContents` performs -- just restricted to this annotation.

   .. index::
      pair: matrix; Annot.GetPixmap
      pair: colorSpace; Annot.GetPixmap
      pair: alpha; Annot.GetPixmap
      pair: dpi; Annot.GetPixmap

   .. method:: GetPixmap(Matrix matrix: null, int dpi: 0, ColorSpace colorSpace: null, int alpha: 0)

      Creates a pixmap from the annotation as it appears on the page in untransformed coordinates. The pixmap's :ref:`IRect` equals *Annot.rect.irect* (see below).

      :arg Matrix matrix: a matrix to be used for image creation. Default is :ref:`Identity`.

      :arg int dpi: desired resolution in dots per inch. If not `null`, the matrix parameter is ignored.

      :arg colorSpace: a colorspace to be used for image creation. Default is *Utils.csRGB*.
      :type ColorSpace: :ref:`ColorSpace`

      :arg bool alpha: whether to include transparency information. Default is *false*.

      :rtype: :ref:`Pixmap`

      .. note::
         
         * If the annotation has just been created or modified, you should :meth:`Document.ReloadPage` the page first via `page = doc.ReloadPage(page)`.

         * The pixmap will have *"premultiplied"* pixels if `alpha=1`. To learn about some background, e.g. look for "Premultiplied alpha" `here <https://en.wikipedia.org/wiki/Glossary_of_computer_graphics#P>`_.


   .. method:: SetInfo(AnnotInfo info: null, string content: null, string title: null, string creationDate: null, string modDate: null, string subject: null)

      Changes annotation properties. These include dates, contents, subject and author (title). Changes for *name* and *id* will be ignored. The update happens selectively: To leave a property unchanged, set it to *null*. To delete existing data, use an empty string.

      :arg info: a dictionary compatible with the *info* property (see below). All entries must be strings. If this argument is not a dictionary, the other arguments are used instead -- else they are ignored.
      :arg string content: see description in :attr:`Info`.
      :arg string title: see description in :attr:`Info`.
      :arg string creationDate: date of annot creation. If given, should be in PDF datetime format.
      :arg string modDate: date of last modification. If given, should be in PDF datetime format.
      :arg string subject: see description in :attr:`Info`.

   .. method:: SetLineEnds(PdfLineEnding start, PdfLineEnding end)

      Sets an annotation's line ending styles. Each of these annotation types is defined by a list of points which are connected by lines. The symbol identified by *start* is attached to the first point, and *end* to the last point of this list. For unsupported annotation types, a no-operation with a warning message results.

      .. note::

         * While 'FreeText', 'Line', 'PolyLine', and 'Polygon' annotations can have these properties, MuPDF does not support line ends for 'FreeText', because the call-out variant of it is not supported.
         * Some symbols have an interior area (diamonds, circles, squares, etc.). By default, these areas are filled with the fill color of the annotation. If this is `null`, then white is chosen. The `fillColor` argument of :meth:`Annot.Update` can now be used to override this and give line end symbols their own fill color.

      :arg start: The symbol number for the first point.
      :arg end: The symbol number for the last point.

   .. method:: SetOC(int oc: 0)

      Set the annotation's visibility using PDF optional content mechanisms. This visibility is controlled by the user interface of supporting PDF viewers. It is independent from other attributes like :attr:`Annot.Flags`.

      :arg int oc: the :data:`xref` of an optional contents group (OCG or OCMD). Any previous xref will be overwritten. If zero, a previous entry will be removed. An exception occurs if the xref is not zero and does not point to a valid PDF object.

      .. note:: This does **not require executing** :meth:`Annot.Update` to take effect.

   .. method:: GetOC()

      Return the :data:`xref` of an optional content object, or zero if there is none.

      :returns: zero or the xref of an OCG (or OCMD).


   .. method:: SetIrtXRef(int xref)

      Set annotation to be "In Response To" another one.

      :arg int xref: The :data:`xref` of another annotation.

         .. note:: Must refer to an existing annotation on this page. Setting this property requires no subsequent `Update()`.


   .. method:: SetOpen(int isOpen)

      Set the annotation's Popup annotation to open or closed -- **or** the annotation itself, if its type is 'Text' ("sticky note").

      :arg int isOpen: the desired open state (1 = `true`, 0 = `false`)`.`


   .. method:: SetPopup(Rect rect)

      Create a Popup annotation for the annotation and specify its rectangle. If the Popup already exists, only its rectangle is updated.

      :arg rect: the desired rectangle.



   .. method:: SetOpacity(float opacity)

      Set the annotation's opacity. Opacity can also be set in :meth:`Annot.Update`.

      :arg float opacity: a float in range `[0, 1]`. Any value outside is assumed to be 1. E.g. a value of 0.5 sets the transparency to 50%.

      Three overlapping 'Circle' annotations with each opacity set to 0.5:

      .. image:: ../images/img-opacity.*

   .. attribute:: BlendMode

      The annotation's blend mode. See :ref:`AdobeManual`, page 324 for explanations.

      :rtype: string
      :returns: the blend mode or `""`.


   .. method:: SetBlendMode(string blendMode)

      Set the annotation's blend mode. See :ref:`AdobeManual`, page 324 for explanations. The blend mode can also be set in :meth:`Annot.Update`.

      :arg string blendmode: set the blend mode. Use :meth:`Annot.Update` to reflect this in the visual appearance. For predefined values see :ref:`BlendMode`. Use `PDF_BM_Normal` to **remove** a blend mode.


   .. method:: SetName(string name)

      Change the name field of any annotation type. For 'FileAttachment' and 'Text' annotations, this is the icon name, for 'Stamp' annotations the text in the stamp. The visual result (if any) depends on your PDF viewer. See also :ref:`mupdficons`.

      :arg string name: the new name.

      .. caution:: If you set the name of a 'Stamp' annotation, then this will **not change** the rectangle, nor will the text be layouted in any way. If you choose a standard text from :ref:`StampIcons` (the **exact** name piece after `"STAMP_"`), you should receive the original layout. An **arbitrary text** will not be changed to upper case, but be written in font "Times-Bold" as is, horizontally centered in **one line** and be shortened to fit. To get your text fully displayed, its length using :data:`fontSize` 20 must not exceed 190 points. So please make sure that the following inequality is true: `Utils.GetTextLength(text, fontName: "tibo", fontSize: 20) <= 190`.

   .. method:: SetRect(Rect rect)

      Change the rectangle of an annotation. The annotation can be moved around and both sides of the rectangle can be independently scaled. However, the annotation appearance will never get rotated, flipped or sheared. This method only affects certain annotation types [#f2]_ and will lead to a  console message in other cases. No exception will be raised, but `false` will be returned.

      :arg rect: the new rectangle of the annotation (finite and not empty).

      .. note:: You **need not** invoke :meth:`Annot.Update` for activation of the effect.


   .. method:: SetRotation(int rotate: 0)

      Set the rotation of an annotation. This rotates the annotation rectangle around its center point. Then a **new annotation rectangle** is calculated from the resulting quad.

      :arg int angle: rotation angle in degrees. Arbitrary values are possible, but will be clamped to the interval `[0, 360)`.

      .. note::
        * You **must invoke** :meth:`Annot.Update` to activate the effect.
        * For PDF_ANNOT_FREE_TEXT, only one of the values 0, 90, 180 and 270 is possible and will **rotate the text** inside the current rectangle (which remains unchanged). Other values are silently ignored and replaced by 0.
        * Otherwise, only the :ref:`AnnotationTypes` 'Square', 'Circle', 'Caret', 'Text', 'FileAttachment', 'Ink', 'Line', 'Polyline', 'Polygon', and 'Stamp' can be rotated. For all others the method is a no-op.


   .. method:: SetBorder(Border border: null, float width: -1, string style: null, int[] dashes: null, int clouds: -1)

      PDF only: Change border width, dashing, style and cloud effect. See the :attr:`Annot.Border` attribute for more details.


      :arg Dictionary border: a dictionary as returned by the :attr:`Border` property, with keys `"width"` (`float`), `"style"` (`string`),  `"dashes"` (`int[]`) and `clouds` (`int`). Omitted keys will leave the resp. property unchanged. Set the border argument to `null` (the default) to use the other arguments.

      :arg float width: A non-negative value will change the border line width.
      :arg string style: A value other than `null` will change this border property.
      :arg int[] dashes: All items of the sequence must be integers, otherwise the parameter is ignored. To remove dashing use: `dashes=[]`. If dashes is a non-empty sequence, "style" will automatically be set to "D" (dashed). 
      :arg int clouds: A value >= 0 will change this property. Use `clouds=0` to remove the cloudy appearance completely. Only annotation types 'Square', 'Circle', and 'Polygon' are supported with this property.

   .. method:: SetFlags(int flags)

      Changes the annotation flags. Use the `|` operator to combine several.

      :arg int flags: an integer specifying the required flags.

   .. method:: SetColors(Color colors: null, float[] stroke: null, float[] fill: null)

      Changes the "stroke" and "fill" colors for supported annotation types -- not all annotations accept both.

      :arg Dictionary colors: a dictionary containing color specifications. For accepted dictionary keys and values see below. The most practical way should be to first make a copy of the *colors* property and then modify this dictionary as required.
      :arg float[] stroke: see above.
      :arg float[] fill: see above.


   .. method:: DeleteResponses()

      Delete annotations referring to this one. This includes any 'Popup' annotations and all annotations responding to it.


   .. index::
      pair: blendMode; Annot.Update
      pair: fontSize; Annot.Update
      pair: textColor; Annot.Update
      pair: borderColor; Annot.Update
      pair: fillColor; Annot.Update
      pair: crossOut; Annot.Update
      pair: rotate; Annot.Update

   .. method:: Update(string blendMode: null, float opacity: 0.0f, float fontSize: 0.0f, string fontName: null, float[] textColor: null, float[] borderColor: null, float[] fillColor: null, bool crossOut: true, int rotate: -1)

      Synchronize the appearance of an annotation with its properties after relevant changes. 

      You can safely **omit** this method **only** for the following changes:

         * :meth:`Annot.SetRect`
         * :meth:`Annot.SetFlags`
         * :meth:`Annot.SetOC`
         * :meth:`Annot.UpdateFile`
         * :meth:`Annot.SetInfo` (except any changes to *"content"*)

      All arguments are optional. Blend mode and opacity are applicable to **all annotation types**. The other arguments are mostly special use, as described below.

      Color specifications may be made in the usual format used in PuMuPDF as sequences of floats ranging from 0.0 to 1.0 (including both). The sequence length must be 1, 3 or 4 (supporting GRAY, RGB and CMYK colorspaces respectively). For GRAY, just a float is also acceptable.

      :arg float opacity: **valid for all annotation types:** change or set the annotation's transparency. Valid values are *0 <= opacity < 1*.
      :arg string blendMode: **valid for all annotation types:** change or set the annotation's blend mode. For valid values see :ref:`BlendModes`.
      :arg float fontSize: change :data:`fontSize` of the text. 'FreeText' annotations only.
      :arg float[] textColor: change the text color. 'FreeText' annotations only.
      :arg float[] borderColor: change the border color. 'FreeText' annotations only.
      :arg float[] fillColor: the fill color.

          * 'Line', 'Polyline', 'Polygon' annotations: use it to give applicable line end symbols a fill color other than that of the annotation.

      :arg bool crossOut: add two diagonal lines to the annotation rectangle. 'Redact' annotations only. If not desired, *false* must be specified even if the annotation was created with *false*.
      :arg int rotate: new rotation value. Default (-1) means no change. Supports 'FreeText' and several other annotation types (see :meth:`Annot.SetRotation`), [#f1]_. Only choose 0, 90, 180, or 270 degrees for 'FreeText'. Otherwise any integer is acceptable.

      :rtype: bool

      .. note:: Using this method inside a :meth:`Page.GetAnnots` loop is **not recommended!** This is because most annotation updates require the owning page to be reloaded -- which cannot be done inside this loop. Please use the example coding pattern given in the documentation of this generator.


   .. attribute:: FileInfo

      Basic information of the annot's attached file.

      :rtype: FileInfo
      :returns: a structure with keys `FileName`, `Desc` (description), `Size` (uncompressed file size), `Length` (compressed length) for FileAttachment annot types, else `null`.

   .. method:: GetFile()

      Returns attached file content.

      :rtype: byte[]
      :returns: the content of the attached file.

   .. index::
      pair: buffer; Annot.UpdateFile
      pair: fileName; Annot.UpdateFile
      pair: uFilename; Annot.UpdateFile
      pair: desc; Annot.UpdateFile

   .. method:: UpdateFile(byte[] buffer: null, string fileName: null, string uFilename: null, string desc: null)

      Updates the content of an attached file. All arguments are optional. Default-only arguments lead to a no-op.

      :arg buffer: the new file content. Omit to only change meta-information.
      :arg string fileName: new filename to associate with the file.
      :arg string uFilename: new unicode filename to associate with the file.
      :arg string desc: new description of the file content.

   .. method:: GetSound()

      Return the embedded sound of an audio annotation.

      :rtype: Sound
      :returns: the sound audio file and accompanying properties. These are the possible dictionary keys, of which only "rate" and "stream" are always present.

        =========== =======================================================
        Key         Description
        =========== =======================================================
        Rate        (float, requ.) samples per second
        Channels    (int, opt.) number of sound channels
        Bps         (int, opt.) bits per sample value per channel
        Encoding    (string, opt.) encoding format: Raw, Signed, muLaw, ALaw
        Compression (string, opt.) name of compression filter
        Stream      (byte[], requ.) the sound file content
        =========== =======================================================


   .. attribute:: Opacity

      The annotation's opacity. If set, it is a value in range `[0, 1]`. The PDF default is 1. However, in an effort to tell the difference, we return `-1.0` if not set.

      :rtype: float

   .. attribute:: Parent

      The owning page object of the annotation.

      :rtype: :ref:`Page`

   .. attribute:: Rotation

      The annot rotation.

      :rtype: int
      :returns: a value [-1, 359]. If rotation is not at all, -1 is returned (and implies a rotation angle of 0). Other possible values are normalized to some value value 0 <= angle < 360.

   .. attribute:: Rect

      The rectangle containing the annotation.

      :rtype: :ref:`Rect`

   .. attribute:: Next

      The next annotation on this page or `null`. "Next", as usual in PDF, means the next annotation in the page's array of annotation references. This has nothing to do with geometrical annotation positions on the page.

      :rtype: :ref:`Annot`

   .. attribute:: Type

      A number and one or two strings describing the annotation type, like `[2, 'FreeText', 'FreeTextCallout']`. The second string entry is optional and may be empty. See the appendix :ref:`AnnotationTypes` for a list of possible values and their meanings.

      :rtype: Tuple(PdfAnnotType, string, string)

   .. attribute:: Info

      A dictionary containing various information. All fields are optional strings. Items not provided are represented by empty strings.

      * `name` -- e.g. for 'Stamp' annotations it will contain the stamp text like "Sold" or "Experimental", for other annot types you will see the name of the annot's icon here ("PushPin" for FileAttachment).

      * `content` -- a string containing the text for type *Text* and *FreeText* annotations. Commonly used for filling the text field of annotation pop-up windows.

      * `title` -- a string containing the title of the annotation pop-up window. By convention, this is used for the **annotation author**.

      * `creationDate` -- creation timestamp.

      * `modDate` -- last modified timestamp.

      * `subject` -- subject.

      * `id` -- a unique identification of the annotation. This is taken from PDF key `/NM`. Annotations added by MuPDF.NET will have a unique name, which appears here.

      :rtype: :ref:`AnnotInfo`

   .. attribute:: Flags

      An integer whose bit positions contain flags for how the annotation should be presented.

      :rtype: int

   .. attribute:: LineEnds

      A pair of integers specifying start and end symbol of annotations types 'FreeText', 'Line', 'PolyLine', and 'Polygon'. `null` if not applicable. For possible values and descriptions in this list, see the :ref:`AdobeManual`, table 1.76 on page 400.

      :rtype: Tuple(PdfLineEnding, PdfLineEnding)

   .. attribute:: Vertices

      A list containing a variable number of point ("vertices") coordinates (each given by a pair of floats) for various types of annotations:

      * `Line` -- the starting and ending coordinates (2 float pairs).
      * `FreeText` -- 2 or 3 float pairs designating the starting, the (optional) knee point, and the ending coordinates.
      * `PolyLine` / 'Polygon' -- the coordinates of the edges connected by line pieces (n float pairs for n points).
      * text markup annotations -- 4 float pairs specifying the *QuadPoints* of the marked text span (see :ref:`AdobeManual`, page 403).
      * `Ink` -- list of one to many sublists of vertex coordinates. Each such sublist represents a separate line in the drawing.

      :rtype: Dynamic


   .. attribute:: Colors

      Dictionary of two lists of floats in range `0 <= float <= 1` specifying the "stroke" and the interior ("fill") colors. The stroke color is used for borders and everything that is actively painted or written ("stroked"). The fill color is used for the interior of objects like line ends, circles and squares. The lengths of these lists implicitly determine the colorspaces: 1 = GRAY, 3 = RGB, 4 = CMYK. So `[1.0, 0.0, 0.0]` stands for RGB color red. Both lists can be empty if no color is specified.

      :rtype: float[]

   .. attribute:: Xref

      The PDF :data:`xref`.

      :rtype: int

   .. attribute:: IrtXref

      The PDF :data:`xref` of an annotation to which this one responds. Return zero if this is no response annotation.

      :rtype: int

   .. attribute:: PopupXref

      The PDF :data:`xref` of the associated Popup annotation. Zero if non-existent.

      :rtype: int

   .. attribute:: HasPopup

      Whether the annotation has a Popup annotation.

      :rtype: bool

   .. attribute:: IsOpen

      Whether the annotation's Popup is open -- **or** the annotation itself ('Text' annotations only).

      :rtype: bool

   .. attribute:: PopupRect

      The rectangle of the associated Popup annotation. Infinite rectangle if non-existent.

      :rtype: :ref:`Rect`

   .. attribute:: RectDelta

      A tuple of four floats representing the `/RD` entry of the annotation. The four numbers describe the numerical differences (left, top, -right, -bottom) between two rectangles: the :attr:`Rect` of the annotation and a rectangle contained within that rectangle. If the entry is missing, this property is `(0, 0, 0, 0)`. If the annotation border is a normal, straight line, these numbers are typically border width divided by 2. If the annotation has a "cloudy" border, you will see the breadth of the cloud semi-circles here. In general, the numbers need not be identical. To compute the inner rectangle do `a.rect + a.rect_delta`.

   .. attribute:: Border

      A dictionary containing border characteristics. Empty if no border information exists. The following keys may be present:

      * `width` -- a float indicating the border thickness in points. The value is -1.0 if no width is specified.

      * `dashes` -- a sequence of integers specifying a line dashing pattern. *[]* means no dashes, *[n]* means equal on-off lengths of *n* points, longer lists will be interpreted as specifying alternating on-off length values. See the :ref:`AdobeManual` page 126 for more details.

      * `style` -- 1-byte border style: **"S"** (Solid) = solid line surrounding the annotation, **"D"** (Dashed) = dashed line surrounding the annotation, the dash pattern is specified by the *dashes* entry, **"B"** (Beveled) = a simulated embossed rectangle that appears to be raised above the surface of the page, **"I"** (Inset) = a simulated engraved rectangle that appears to be recessed below the surface of the page, **"U"** (Underline) = a single line along the bottom of the annotation rectangle.

      * `clouds` -- an integer indicating a "cloudy" border, where `n` is an integer `-1 <= n <= 2`. A value `n: 0` indicates a straight line (no clouds), 1 means small and 2 means large semi-circles, mimicking the cloudy appearance. If -1, then no specification is present.

      :rtype: Border


.. _mupdficons:

Annotation Icons in MuPDF
-------------------------

This is a list of icons referenceable by name for annotation types 'Text' and 'FileAttachment'. You can use them via the *icon* parameter when adding an annotation, or use the as argument in :meth:`Annot.SetName`. It is left to your discretion which item to choose when -- no mechanism will keep you from using e.g. the "Speaker" icon for a 'FileAttachment'.

.. image:: ../images/mupdf-icons.*

Output Structures
--------------------------------

AnnotInfo Structure
~~~~~~~~~~~~~~~~~~~

=============== ====================================================================
**Key**         **Value**
=============== ====================================================================
Content         a string containing the text for type Text and FreeText annotations
Name            name of annotation's icon *string*
Title           a string containing the title of the annotation pop-up window.
CreationDate    creation timestamp
ModDate         last modified timestamp
Subject         subject *string*
Id              a unique identification of the annotation *string*
=============== ====================================================================

AnnotXref Structure
~~~~~~~~~~~~~~~~~
=============== ===================================================
**Key**         **Value**
=============== ===================================================
Id              a unique identification of the annotation *string*
Xref            annot's xref *int*
AnnotType       annotation type. see :ref:`AnnotationTypes`
=============== ===================================================

Border Structure
~~~~~~~~~~~~~~~~~

========= ===============================================================
**Key**         **Value**
========= ===============================================================
Width     a *float* indicating the border thickness in points
Style     1-byte border style: `S`, others include `B`, `U`, `I` and `D`.
Dashes    a sequence of *int* specifying a line dashing pattern
Clouds    an integer indicating a "cloudy" border
========= ===============================================================

Color Structure
~~~~~~~~~~~~~~~~~

======== ======================================================================
**Key**  **Value**
======== ======================================================================
Fill     a *list* of float storing the fill color, each item is between 0 and 1
Stroke   a *list* of float storing the stroke color
======== ======================================================================

FileInfo Structure
~~~~~~~~~~~~~~~

========= =================================
**Key**  **Value**
========= =================================
FileName  *string* file name
Desc      *string* description of the file
Length    *int* compressed length
Size      *int* uncompressed file size
========= =================================

.. rubric:: Footnotes

.. [#f1] Rotating an annotation also changes its rectangle. Depending on how the annotation was defined, the original rectangle is **cannot be reconstructed** by setting the rotation value to zero again and will be lost.

.. [#f2] Only the following annotation types support method :meth:`Annot.SetRect`: Text, FreeText, Square, Circle, Redact, Stamp, Caret, FileAttachment, Sound, and Movie.

.. include:: ../footer.rst
