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

Model setup
-----------
1. Make a new directory where you want to install the model and move into that
   directory in Terminal
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
