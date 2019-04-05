### Reproduce the output

    ./kuva ./blossom-pattern.jpg -P2 -PNG -o output -v -ra 0.35 -re 5 -C 30    		# blossom image
    ./kuva ./brick.jpg -P2 -PNG -o output -v -ra 0.2 -re 10 -C 15 			# brick texture


### Sample

#### Input
![Image of Input](https://github.com/ljsabc/kuva/raw/master/blossom-pattern.jpg)
    
#### Output
![Image of output](https://github.com/ljsabc/kuva/raw/master/output.png)

#### Input
![Image of Input](https://github.com/ljsabc/kuva/raw/master/brick.jpg)
    
#### Output
![Image of output](https://github.com/ljsabc/kuva/raw/master/output2.png)

### Original README

	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

		     K U V A

	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

	JP <jeanphilippe.aumasson@gmail.com>
	http://www.131002.net 
	January 2006
	Version 0.1

	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


	0. WHAT IS KUVA ?
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


	KUVA is an implementation of a graph cut technique to
	generate a texture from a small sample image (the patch), introduced
	in the following article:

	@article{kwatra:2003:SIGGRAPH,
	    author = {Vivek Kwatra and Arno Schödl and Irfan Essa and Greg Turk and Aaron Bobick},
	    title = {Graphcut Textures: Image and Video Synthesis Using Graph Cuts},
	    journal = {ACM Transactions on Graphics, SIGGRAPH 2003},
	    year = {2003},
	    month = {July},
	    volume = {22},
	    number = {3},
	    pages = {277-286}
	} 
	The article and many examples can be found at
	http://www.cc.gatech.edu/cpl/projects/graphcuttextures/

	KUVA takes as input a small image (about 200x200 pixels generally),
	and build a texture according to the given options: defaultly, the
	texture is only displayed, not saved on disk (for this, use the -o option).

	Another image is displayed, in addition to the texture: this image,
	with blue lines across it, shows the seams that were computed by the
	graph cut algorithm. For more details about the algorithms used, read
	the original article or the file doc/report.pdf.

	Why this name ? "kuva" means "image" in Finnish.



	1. INSTALLATION
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


	KUVA works on Linux systems, you only require a C compiler, and
	standard headers. Imagemagick is required if you want to use other
	image formats than BMP.

	Once downloaded the compressed archive, type in a terminal:

	$ tar zxvf kuva-VERSION.tar.gz
	$ cd kuva-VERSION
	$ make

	Then run KUVA with
	$ ./kuva


	2. USE KUVA
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


	In this section we tell a few words about KUVA's options.

	Remember that you can interrupt texture computation by typing Ctrl-C,
	then the current texture is displayed before the process ends.

	Another hidden function is the "stop & go", when holding the mouse's
	left button on the image while computing the texture.

	The general syntax is
	$ ./kuva [input image] [options]

	You can display the options list by entering only
	$ ./kuva

	The options can be divided into several classes:


	2.1. COST FUNCTIONS

	The article describes two cost functions, we implemented both of them,
	defaultly using the second. Then enter -C1 to use the basic function.


	2.2 PLACEMENT ALGORITHM

	Three very different algorithms are available to place the patch on
	the texture, before finding seams:

	    * Random placement ( -P1 )
		     The faster algorithm, gives good result for homogenous
		     textures.

	    * Entire patch matchin ( -P2 )
		     The slower algorithm, the best for complex and structured
		     textures.  

	    * Subpatch matching ( -P3 , default algorithm )
		     A balance between the two previous algorithms, faster
		     than -P2 but do not fit too complex images.


	2.3 OUTPUT TYPE

	If you choosed to save the output image on disk, by entering the -o
	option, you can also specify an image format, with the options -BMP
	(default), -JPG, or -PNG.

	Note that you need Imagemagick to use other image formats than BMP.


	2.4 TRANSFORMATIONS

	Before placing the patch on the output image, you can apply some
	transformations to increase the diversity of the texture, when the
	image allows it:


	    * Mirror ( -m )
		     To apply a random mirror effect.

	    * Rotate ( -r )
		     To apply a random rotation.


	2.5 OTHER OPTIONS

	We present here some examples of parameters for the example patches,
	so as to figure out which algorithm fits for which texture type.


	    * -C n
		Set the reduction coefficient applyed to edges cost
		(default: 20).

	    * -cx n
		Set the texture width to n times the image's (default: 3).

	    * -cy n
		Set the texture height to n times image's (default: 3).

	    * -h 
		Display the help informations.

	    * -o f
		Save output image on disk under the given name (do not give
		extension).

	    * -pc
		Set initial patch position to the top-left corner.

	    * -pr
		Randomly set the initial patch position (default).

	    * -ra n
		Set the minimal overlap ratio.

	    * -re n
		Process to n refinement steps after the whole texture is
		filled. 

	    * -sr
		Automatically switch to a faster placement algorithm when no
		advance in the texture synthesis.

	    * -v
		Set verbose mode.



	3. EXAMPLES
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


	$ ./kuva img/lobelia.gif

	Here we use all default parameters, the subpatch matching fits well
	for this image, which is too heterogenous for random placement, but is
	diversified enough to have a good result with this algorithm.


	$ ./kuva img/ecrous.gif -cx 2 -cy 2 -P2 -re 1

	This image is quite difficult, there is a clear structure, and we can
	easily see discontinuities, that's why we prefer the entire patch
	matching. here we want a not too large image, and also ask for a final
	refinement step.


	$ ./kuva img/carpet.jpg -P1 -r -C1

	The carpet.jpg image is random enough to use the random placement, in
	addition we add diversity by applying random rotation, and use the
	faster basic functions, since we won't perceive any improvement here
	by using -C2.


	$ ./kuva img/freshblueberries.gif -o blueberries -PNG -pc

	We specify here an output image and a format (PNG), hence the final
	texture will be saved in the current directory as bluesberries.png. We
	also ask KUVA to set the initial patch at the top-left corner.

