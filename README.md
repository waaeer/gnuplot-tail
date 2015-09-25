# gnuplot-tail
A graphical tail utility using gnuplot to plot a dynamically changing graph.
tails one or more files with (x,y) pairs and draws independed graphs on signle gnuplot canvas.

## usage

	perl gnuplot-tail	[--window DELTA] [--sleep SECONDS] file1 file2 file3

## Options

* window : if defined, DELTA is maximal range of X axis. [max-DELTA:max]

* sleep : the pause between reading the files inseconds, can be fractional. Default is 1 second.

## preprequisites

* Gnuplot
* perl

