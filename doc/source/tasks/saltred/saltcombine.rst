.. _saltcombine:

***********
saltcombine
***********


Name
====

saltcombine -- Combine SALT images

Usage
=====

saltcombine images outimage (combine) (reject) (mask) (weight) (blank) (scale)
(statsec) (lthresh) (hthresh) (clobber) (logfile) (verbose)

Parameters
==========


*images*
    String. List of input images including, if necessary, absolute or
    relative paths to the data. Data can be provided as a comma-separated
    list, or a string with a wildcard (e.g. 'images=S20061210*.fits'), or
    a foreign file containing an ascii list of image filenames. For ascii
    list option, the filename containing the list must be provided
    preceded by a '@' character, e.g. 'images=@listoffiles.lis'. The input
    files will be overwritten with the gain-corrected data.

*outimage*
    String. This is the name of the output image to be created.  This image
    is the combined image of all of the input images.

*combine*
    String. Method for combining the data.  The different options are
    average and median.

*reject*
    String. Method for rejecting the data.  The different options are
    None, ccdclip, and sigclip.  ccdclip uses the variance in the data
    to reject pixels based on the expected noise.  sigclip rejects pixels that
    are outside of the thresholds set by hthresh and lthresh.

*mask*
    Bool. If set to 'yes', the BPM frames in the data will be used to
    reject pixels

*weight*
    Bool. If set to 'yes', the inverse variances frames in the data will be used to
    produce a weighted average of the data.

*blank*
    Real. Value to use if there are no useful pixels at a position

*scale*
    String. Type of multiplicative scaling.  If it is set to 'average', it will use
    the 'average' of the section specified in statsec to scale the images. Likewise
    for the 'median'.

*statsec*
    String. Section to use for calculating the value to scale by.  Leave blank to use
    the whole image.

*lthresh*
    Real. Lower threshold to reject pixels for 'ccdclip' or 'sigclip'

*hthresh*
    Real. Upper threshold to reject pixels for 'ccdclip' or 'sigclip'

*(verbose)*
    Boolean. If verbose=n, log messages will be suppressed.

Description
===========

SALTCOMBINE is a tool for combining images into a master image.  The program
has a number of options for how the images are combine including the combination
and rejection methods.

The task assume that all input images are of the same size and type.
Options for combining the sources are average or median.  For average
combination, a weighted average can be applied if inverse variance frames are
included with the data.   To include inverse variance frames, the extension of
the frames should be included using the IVAREXT keyword in the corresponding
data extension.   Also, if the BPMEXT keyword is set, it will also use the bad
pixel maps for rejecting pixels.


Options for rejection only include ccdclip and sigclip.  If 'ccdclip' is
selected, the gain and readnoise will be extracted from the image header and
used to calculate the expected variance in the frames.  The task will check to
see if the data have already been gain corrected and use the appropriate value
if they have been. If 'sigclip' is selected, the standard deviation of the
pixels being averaged will be calculated for use in the rejection. Any source
outside of the thresholds set by lthresh and hthresh will be rejected.

Prior to combining the images, the images can be multiplicatively scaled. The
user can set the scale statistic to either calculate the mean or the median
of a region statsec.  The images will then be scaled by that value.   If statsec
is left blank, the scaling will be based on the statistics of the entire image.


Examples
========

1. To combine a set of SALT images into a master flat::

    --> saltcombine images='@images.lis' outimage='VFLAT.fits'
    logfile='salt.log' verbose='yes'

Time and disk requirements
==========================

Individual unbinned full frame RSS image files can be 112MB in size. It is
recommended to use workstations with a minimum of 512MB RAM. On a
linux machine with 2.8 Ghz processor and 2 Gb of RAM, one 2051x2051 image can
be processed in 0.31 sec.

Bugs and limitations
====================

Not all features available in iraf.ccdred.combine are available
for SALTCOMBINE.

Send feedback and bug reports to salthelp@saao.ac.za

See also
========

 :ref:`saltclean` :ref:`saltmosaic`