# gnuplot-tail
A graphical tail utility using gnuplot to plot a dynamically changing graph.
tails one or more files with (x,y) pairs and draws independed graphs on a single gnuplot canvas.

## Usage

	perl gnuplot-tail	[--window DELTA] [--sleep SECONDS] file1 file2 file3

## Options

* window : if defined, DELTA is maximal range of X axis. [max-DELTA:max]

* sleep : the pause between reading the files in seconds, can be fractional. Default is 1 second.

## Prerequisites

* Gnuplot
* perl

## Example

	perl gnuplot-tail-demo file1 file2 file3 &  ## generates demo data
	perl gnuplot-tail      file1 file2 file3

