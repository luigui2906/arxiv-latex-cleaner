# `arxiv_latex_cleaner`

This tool allows you to easily clean the LaTeX code of your paper to submit to
arXiv. From a folder containing all your code, e.g. `/path/to/latex/`, it
creates a new folder `/path/to/latex_arXiv/`, that is ready to ZIP and upload to
arXiv.

## Example call:

```console
arxiv_latex_cleaner /path/to/latex --im_size 500 --images_whitelist='{"images/im.png":2000}'
```

## Installation:

```console
pip install arxiv-latex-cleaner
```

| :exclamation:  arxiv_latex_cleaner is only compatible with Python >=3  :exclamation: |
|--------------------------------------------------------------------------------------|

Alternatively, you can download the source code:

```console
git clone https://github.com/google-research/arxiv-latex-cleaner
cd arxiv-latex-cleaner/
python -m arxiv_latex_cleaner --help
```

And install as a command-line program directly from the source code:

```console
python setup.py install
```

## Main features:

#### Privacy-oriented

*   Removes all auxiliary files (`.aux`, `.log`, `.out`, etc.).
*   Removes all comments from your code (yes, those are visible on arXiv and you
    do not want them to be). These also include `\begin{comment}\end{comment}`
    and `\iffalse\fi` environments.
*   Optionally removes user-defined commands entered with `commands_to_delete`
    (such as `\todo{}` that you redefine as the empty string at the end).

#### Size-oriented

There is a 50MB limit on arXiv submissions, so to make it fit:

*   Removes all unused `.tex` files (those that are not in the root and not
    included in any other `.tex` file).
*   Removes all unused images that take up space (those that are not actually
    included in any used `.tex` file).
*   Optionally resizes all images to `im_size` pixels, to reduce the size of the
    submission. You can whitelist some images to skip the global size using
    `images_whitelist`.
*   Optionally compresses `.pdf` files using ghostscript (Linux and Mac only).
    You can whitelist some PDFs to skip the global size using
    `images_whitelist`.

#### TikZ picture source code concealment

To prevent the upload of tikzpicture source code or raw simulation
data, this feature:

*   Replaces the tikzpicture environment `\begin{tikzpicture} ... \end{tikzpicture}`
    with the respective `\includegraphics{EXTERNAL_TIKZ_FOLDER/picture_name.pdf}`.
*   Requires externally compiled TikZ pictures as `.pdf` files in folder `EXTERNAL_TIKZ_FOLDER`.
    See section 53 in the [PGF/TikZ manual](https://ctan.org/pkg/pgf?lang=en) on TikZ picture externalization.
*   Only replaces environments with preceding `\tikzsetnextfilename{picture_name}` command
    (as in `\tikzsetnextfilename{picture_name}\begin{tikzpicture} ... \end{tikzpicture}`) where
    the externalized `picture_name.pdf` filename matches `picture_name`.

## Usage:

```
usage: arxiv_latex_cleaner@v0.1.8 [-h] [--resize_images] [--im_size IM_SIZE]
                                  [--compress_pdf]
                                  [--pdf_im_resolution PDF_IM_RESOLUTION]
                                  [--images_whitelist IMAGES_WHITELIST]
                                  [--commands_to_delete COMMANDS_TO_DELETE [COMMANDS_TO_DELETE ...]]
                                  input_folder

Clean the LaTeX code of your paper to submit to arXiv. Check the README for
more information on the use.

positional arguments:
  input_folder          Input folder containing the LaTeX code.

optional arguments:
  -h, --help            show this help message and exit
  --resize_images       Resize images.
  --im_size IM_SIZE     Size of the output images (in pixels, longest side).
                        Fine tune this to get as close to 10MB as possible.
  --compress_pdf        Compress PDF images using ghostscript (Linux and Mac
                        only).
  --pdf_im_resolution PDF_IM_RESOLUTION
                        Resolution (in dpi) to which the tool resamples the
                        PDF images.
  --images_whitelist IMAGES_WHITELIST
                        Images (and PDFs) that won't be resized to the default
                        resolution,but the one provided here. Value is pixel
                        for images, and dpi forPDFs, as in --im_size and
                        --pdf_im_resolution, respectively. Format is a
                        dictionary as: '{"path/to/im.jpg": 1000}'
  --commands_to_delete COMMANDS_TO_DELETE [COMMANDS_TO_DELETE ...]
                        LaTeX commands that will be deleted. Useful for e.g.
                        user-defined \todo commands.
  --use_external_tikz EXTERNAL_TIKZ_FOLDER
                        Folder (relative to input folder) containing
                        externalized TikZ figures in PDF format.
```

## Note

This is not an officially supported Google product.
