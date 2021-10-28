# Markov Switching Multi-fractal model

BT variation of MSM model is implemented in Go.

Usage

```
./msm -k 4 -n 200 -w 240 -i input.csv -o results.csv
```

```
 -i string
    	Name of csv file containing return data
  -k int
    	MSM model dimension, +ve integer, caution: k > 10 can take minutes to run
  -n int
    	Number of simulations for computing mean and std dev of vol estimate, minimum 100 (default 200)
  -o string
    	Name of output file (default "results.csv")
  -w int
    	Time window for vol prediction in number of bars, minimum 30 (default 30)
```
