#!/usr/bin/perl
use strict;
use Getopt::Long;

#################### берет с хвоста несколько файлов (x,y) и рисует независимые графики ############
#################### 

my $window;  ## окно по оси x
my $timeout = 1;
GetOptions ("window=s" => \$window,"sleep=s" => \$timeout );

die("Usage: $0 [--window DELTA] [--sleep SECONDS] file1 file2 file3") unless @ARGV;
 



# open gnuplot
open GNUPLOT, "|gnuplot" || die "Can't initialize gnuplot: $!";
select((select(GNUPLOT), $| = 1)[0]);

print GNUPLOT "set xtics\n";
print GNUPLOT "set ytics\n";
print GNUPLOT "set grid\n";
print GNUPLOT "set multiplot\n";
print GNUPLOT "set style line 1 linecolor rgb 'red'\n";
print GNUPLOT "set style line 2 linecolor rgb 'green'\n";
print GNUPLOT "set style line 3 linecolor rgb 'blue'\n";
print GNUPLOT "set style line 4 linecolor rgb 'cyan'\n";
print GNUPLOT "set style line 5 linecolor rgb 'magenta'\n";
 
my (@in, @pos,@all_points);
my $n = 1;

foreach my $file (@ARGV) {  # open tails

	open (my $fh, '<', $file) || die("Cannot open $file: $!");
	push @in, $fh;
	push @pos,0;
	push @all_points, [];
}


my ($xmin, $xmax, $ymin, $ymax) = (0,1,0,1);

while(1) {   # main loop 
	my ($change_xrange, $change_yrange,$have_changes);
	for(my $i=0;$i<=$#in;$i++) { 
		my $fh = $in[$i];
		my $sz = (stat($fh))[7];
		my $diff  = $sz - $pos[$i];
		if($diff < 0) { # file became smaller: reread it
			seek $fh, 0 , 0;
			$pos[$i] = 0;
			$sz = $diff = (stat($fh))[7];
			$all_points[$i] = [];
			$have_changes = 1;
		}
		if($diff > 0) { 
			my $txt = '';
			read($fh, $txt, $diff);
			$pos[$i] = tell $fh;
			my @lines = split(/\n/, $txt);
			my @points;
			foreach my $l (@lines) { 
				$have_changes = 1;
				my ($x, $y) = split(/\s+/, $l);
				push @points, [$x,$y];
				
				if($x<$xmin) { 		
					$change_xrange = 1; $xmin = $x;
				}
				if($x>$xmax) { 		
					$change_xrange = 1; $xmax = $x;
				}
				if($y<$ymin) { 		
					$change_yrange = 1; $ymin = $y;
				}
				if($y>$ymax) { 		
					$change_yrange = 1; $ymax = $y;
				}
			}
			push @{$all_points[$i]}, @points;
		}
	}
	if ($have_changes) { 
		if($change_xrange) { 
			printf GNUPLOT "set xrange [%lg:%lg]\n",($window && $xmax-$window > $xmin ? $xmax-$window : $xmin),$xmax;
#			warn sprintf "set xrange [%lg:%lg]\n",$xmin,$xmax;
		}
		if($change_yrange) {
			printf GNUPLOT "set yrange [%lg:%lg]\n",$ymin,$ymax;
#			warn sprintf "set yrange [%lg:%lg]\n",$ymin,$ymax;
		}
		print GNUPLOT "unset multiplot\nset multiplot\n";
		for(my $i=0;$i<=$#in;$i++) {
			if (@{$all_points[$i]}) {
				printf GNUPLOT "plot '-' with lines linestyle %d\n", $i+1;
				foreach my $p (@{$all_points[$i]}) { 
					print GNUPLOT "$p->[0] $p->[1]\n";
#				warn  "$p->[0] $p->[1]\n";
				}
				print GNUPLOT "e\n";
			}
		}
	}
	select (undef, undef, undef, $timeout);
}

