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

* **subversion**.
* **gfortran** *or* **ifort**. The model is written in FORTRAN and the
  modelers have setup scripts for these compilers. Others may work too... but
  if you choose to use one you're kinda on your own. I've read ifort is faster
  but I've found gfortran is much easier to install.
* **netCDF**. This one requires the same compiler suite that will be used to
  compile the model (gfortran corresponds to gcc, ifort to icc, etc.).
* **fcm**. This is some utility created by LMD.

Ubuntu
******
First let's update apt with :code:`sudo apt update`. Then we can install
things as described below.

* Install subversion with :code:`apt install subversion`
* Install gfortran with :code:`apt install gfortran`
* Install netCDF with :code:`apt install netcdf-bin`.
* netCDF visualization software. I think this is optional for anyone working
  in Python, as scipy has a library that can do this, so you can avoid this
  hassle.
* Install fcm by running :code:`
  svn checkout http://forge.ipsl.jussieu.fr/fcm/svn/PATCHED/FCM_V1.2` from
  within the trunk directory. Note that the link provided in the documentation
  does not work for me (problem with the underscore). It should say *checked
  out revision 12*.

  We now need to make our shell know about fcm. Open .bashrc (assuming you're
  using bash) and add the FCM's bin directory to the path. I make an lmd
  directory in my home directory where I checkedout trunk, so the command for
  me is :code:`export PATH="$HOME/lmd/trunk/FCM_V1.2/bin:$PATH"`.

I'm a little worried that the netcdf won't have the same compiler flags as
gfortran, and thus won't play well together. But here's to hoping!

macOS
*****
Sumedha will update this when she finds the time.

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

0̃l