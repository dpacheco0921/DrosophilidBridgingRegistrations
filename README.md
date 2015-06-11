Introduction
============
This repository complements our [BridgingRegistrations](https://github.com/jefferislab/BridgingRegistrations) repository for Drosophila melanogaster bridging registrations and extends bridges across to other Drosophila species (yakuba, simulans and virilis).

Details
=======
Each bridging registration in this repository maps one common template brain onto another. The naming convention is `TARGET_SOURCE.list` where TARGET is a short name for the final template brain and SOURCE is a short name for the template that defines the starting space. Thus to map image data in the melanogaster space to the virilis template space one would want `DmelIS1_DvirIS1.list`. 

Software
========
All bridging registrations were constructed with the aid of the open source Computational Morphometry Toolkit ([CMTK](http://www.nitrc.org/projects/cmtk/)) . In each cases the final output contains a non-rigid (warping) intensity-based registration between two template brains (CMTK command `warp`). In certain cases the initial affine that was used as a starting step was computed using a surface or landmarks-based registration rather than CMTK's intensity based registration (`registration`).  You will need to install a recent (>=2.4, >=3.2 strongly recommended) version of CMTK to use these registrations.

Usage
=====

First download (or git clone) this repository

We recommend that you convert all images to the [NRRD format](http://teem.sourceforge.net/nrrd/). [Fiji/ImageJ](http://fiji.sc) can read and write this format.

Image data
----------

    reformatx --floating sample_image.nrrd template_brain.nrrd target_source.list

Note that reformatx uses the `template_brain.nrrd` to determine the size and spatial location of the block of image data to produce. If you do not have the template brain, you can still produce output by manually defining the region that you want using reformatx's `--target-grid` option. 

    --target-grid <string>
              Define target grid for reformating as Nx,Ny,Nz:dX,dY,dZ[:Ox,Oy,Oz] (dims:pixel:offset)

For details

    reformatx --help

3D coordinates
--------------

White space separated 3d coordinates can be converted (in either direction) using 

    streamxform -- --inverse target_source.list < source_coords.txt > target_coords.txt
    streamxform target_source.list < target_coords.txt > sample_coords.txt

Note the use of the `--inverse` switch when mapping coordinates from source space to target space. This may be the opposite of what you expect but originates from the fact that in order to map a block of image data from source space to target space, the transformation that must actually be defined is the one that maps a regular grid of points in the target space back to sample locations in the source space. Note also the use of an extra `--` to separate the positional arguments from the other options when using `--inverse` to specify that a given registration should be inverted.

For details

    streamxform --help

Limitations
===========
These bridging registrations are rarely as accurate registrations for data acquired in the same lab on the same microscope. Nevertheless we find them to be very useful for comparative work across coordinate spaces.
