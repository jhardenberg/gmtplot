#GMTPLOT 0.90

 Creates a quick plot from a netcdf file using GMT and cdo

##Dependencies
 -  [GMT](http://gmt.soest.hawaii.edu/)
 -  [cdo](https://code.zmaw.de/projects/cdo) 
 -  [ps2epsi](http://www.ghostscript.com/)

##Description

The idea for this tool is to create quickly a 'first guess' plot of the contents of 
a netcdf file. 
It is based on the [Generic Mapping Tools](http://gmt.soest.hawaii.edu/), 
a powerful tool, but with a complex set of options. 
GMTPLOT tries to provide a simpler interface.
Since it uses GMT the output plots are high quality and could be used for a publication. 
The options allow a certain degree of customization (they are limited on purpouse). 
If you need more and know how to use GMT you can modify this script to suit your needs or
contribute to the project by adding more options.

##Usage
```
gmtplot inputfile outputprefix [options]

        inputfile    	input netcdf file
	outputprefix	prefix for output files (e.g. "test" will create test.png and test.eps)	

	Options:
	-var string	select variable "string". 
			Default: select first variable in input file
	-timestep value 
			select a timestep in input file. Default: first timestep 
	-title string	Title for plot. Default: nothing		
	-cmin value	minimum value for colormap.
			Default: automatic from file
	-cmax value	maximum value for colormap. 
	                Default: automatic from file
	-cstep value	step to use for colors. Default: (cmax-cmin)/200
	-cstepbar value	
			steps to mark in colobar. Default: (cmax-cmin)/5
	-cmap string	GMT colormap to be used. Default: rainbow
			You can use either a filename or a standard name:
       			Check 	http://geodynamics.usc.edu/~becker/igmt/data/schemes.pdf
				http://soliton.vm.bytemark.co.uk/pub/cpt-city/gmt/index.html
			Try string=whiterain for precipitation
        -listcmap       Lists available colormaps 
        -contour	Use filled contours instead of raster to plot image.
                        The contour interval is cstep
        -contourlines value	Plot discrete contour lines at interval value
        -font	value	The font for annotations and title. Default Helvetica-Bold. 
                        Please see "man gmtdefaults" or 
                        http://gmt.soest.hawaii.edu/gmt/html/man/gmtdefaults.html for a list of fonts.
        -fontsize value	Font size for annotations. Default 12
        -fontsizetitle value	Font size for title. Default 18
	-nocolorbar	switches off colorbar
	-units string	Units for colorbar. 
			Default: read from netcdf file ("units" attribute)
	-R     string
 	-proj  string
	-coord string   Type of projection and region to plot: string=
			robinson	Robinson global projection (Default)
			lonlat
			latlon
			globe
			cylindrical	Cylindrical equidistant global projection - Plate Carrée
			arctic		Polar stereographic projection (arctic 60:90N)
			antarctic	Polar stereographic projection (antarctic -60:-90S) 
			eurocordex-int	Custom Euro-CORDEX oblique mercator projection
			eurocordex	Custom Euro-CORDEX oblique mercator projection
			eurocordex-ext	Custom Euro-CORDEX oblique mercator projection (ext domain)
			x1/x2/y1/y2  	Plot a selected region (cylindrical) 
	-grid  X/Y 	Grid spacing in x and y direction. E.g. -grid 60/30.
			Default: 60/30
	-coast string	Level of detail for coastlines: string=
			c = coarse
			l = low (default)
			i = intermediate
			h = high (not implemented)
			f = full (not implemented)
        -minarea value	Minimum area of features in sq. Km (Default 2500)
	-nonational	Do not plot national boundaries (default for global maps)
	-national	Do plot national boundaries (default for regional maps)
	-remap X/Y  	Remap to a regular grid with X and Y grid points in the 
			longitudinal and latitudinal directions.
			E.g.: "-remap 720/720" remaps to a r720x720 grid first
	-eps		create only eps output graphic (default is both png and eps)
	-png		create only png output graphic (default is both png and eps)
	-density value	Density in PPI for output png (default is 144)	
        -custom	string	Custom CDO command to apply to input field, e.g. -custom divc,3
	-vector	file	Overlays vector plot from file. Default u and v component names are "u" and "v"
	-vectorscale value  Scale for vector (Units: degrees/data units, default 1) 
	-refvector value  Modulus of vector plotted for reference  (Default: 1) 
        -vectorunits string  Units to label the reference vector (Default: -units)
        -keep	 	Do not close ps, another plot will follow (creates a .ps)
        -continue	Continue open plot left by previous -keep
        -noplot 	Do NOT plot inputfile, useful to plot only contours or an empty map
        -nancolor string      Set color for NANs, eg. : -nancolor 128/128/128 (Default)
        -text string/X/Y/FS   Places a string at position X/Y with fontsize FS
        -sea color      Set wet areas (ocean) to color (e.g. -sea blue or -sea 0/0/255)
```

##Examples
    gmtplot test.nc myplot
Default plot of first variable in test.nc using defaults. 
Creates myplot.eps and myplot.png

    gmtplot trmm.nc myplot -var precipitation -timestep 1 -cmin 0 -cmax 2 -cstep 0.01 -cstepbar 0.5 -title "TRMM yearly average precip." -cmap ./GMT_whiterain.cpt -coord -20/50/20/50 -units "mm/hr" -grid 15/15`

Plots TRMM rainfall over the Mediterranean using the "GMT_whiterain.cpt" 
palette file, overriding some defaults.

##Notes 
GMT appears to have problems with some datasets with a curvilinear grid, 
such as those produced by some regional models.
If this happens, you could first reinterpolate to a regular grid 
using the -remap option. E.g. -remap 720/720

##Contact
(c) 2012-2016 Jost von Hardenberg (jvonhardenberg AT isac.cnr.it) - ISAC-CNR, Italy
