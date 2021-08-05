LMD GCM installation notes
==========================
This document is intended to keep track of notes for installing and using the
LMD GCM. The modelers give setup instructions, but I'd like to expand upon
them here (possibly because I'm not as tech literate as I ought to be).

About
-----
* Unfortunately the LMD team uses subversion instead of git.
* The subversion repo can be found at: https://trac.lmd.jussieu.fr/Planeto.
* The user's manual can be found at:
  https://trac.lmd.jussieu.fr/Planeto/browser/trunk/LMDZ.MARS/user_manual.pdf.

Environment setup
-----------------
The model will need several things for it to properly run.

* **subversion**. I ran :code:`sudo apt install subversion` to install it on
  Ubuntu.
* **gfortran** *or* **ifort**. The model is written in FORTRAN and the
  modelers have setup scripts for these compilers. Others may work too... but
  if you choose to use one you're kinda on your own. I've read ifort is faster
  but I've found gfortran is much easier to install as it's simply
  :code:`apt install gfortran` on Ubuntu. 

Model download
--------------
1. Make a new directory where you want to install the model and move into that
   directory in Terminal.
2. Install the model by executing :code:`svn checkout
   http://svn.lmd.jussieu.fr/Planeto/trunk --depth empty` in Terminal. It
   should say :code:`Checked out revision 2553` or something like that. This
   will do a *sparse checkout* and essentially do nothing but link the project
   directory to the svn repository.
3. Go into trunk and execute :code:`svn update LMDZ.MARS LMDZ.COMMON UTIL`.
   This will add these 3 directories to trunk. My guess is that the LMD model
   has files specific to Mars and they initially have you initialize an empty
   repo so you don't get directories that have all sorts of files you don't
   want.

   Note that the manual doesn't mention anything about installing UTIL but that
   command they provided does. Margaux did not install UTIL for me.

4. Now we need to download a set of supplementary files (if not installing
   this on an LMD computer). From within the trunk directory execute:
   :code:`wget -r -nH -R "*html*, *robots*" --cut-dirs=3
   http://www.lmd.jussieu.fr/~lmdz/planets/mars/datadir`. This will download
   all files from the website in the command and put a datadir directory in
   in the current directory with all of those files. Then remove the robots
   file (I cannot figure out how to not download it). -r will recursively
   download, -nH (no header) will suppress making a www.lmd.jussieu.fr
   directory, -R will ignore files with html or robots in them, and finally
   --cut-dirs will skip making dirs for ~lmd, planets, and mars.

   Note that in the user manual, the web address has a higher tilde symbol.
   I had to change it the normal height symbol for the address to register
   correctly.

   Another note: when Margaux installed the model for me, she must have just
   run a wget -r <website> because it also includes a bunch of unnecessary files
   and the terrible directory structure.

5. Finally, regardless of where we're installing the model, we need some
   initial conditions. From within the trunk directory execute:
   :code:`wget -r -nH -R "*html*, *robots*" --cut-dirs=3
   http://www.lmd.jussieu.fr/~lmdz/planets/mars/starts` to download some model
   starts.

   Note that again the higher tilde in the user's manual had to be fixed to
   this normal height symbol for it to work.

   Also note that while they say this is required, I can't find it anywhere in
   the model that Margaux installed.
