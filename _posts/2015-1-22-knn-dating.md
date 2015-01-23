---
layout: post
title: K-nearest neighbor exercise in Julia
permalink: knn-dating
comments: true
tags: [julia, machine learning, knn]
---

My plan is to work through *Machine Learning in Action* (MLA) by Peter Harrington and "translate" the code from Python to Julia. The first exercise concerns k-nearest-neighbor (kNN) algorithm.

<!-- more -->

There are many good sources describing kNN, so I will not take up much time or space here (feel free to skip to the code below). Briefly, we are given a "training" dataset where every data point has a number of characteristics, each modelled as a dimension. We take our "input" datapoint (or a number of points from a "testing" dataset), likewise represented in a multidimensional space, and calculate its Euclidean distance to every point in the training dataset. (Euclidean distance is calculated using Pythagorean formula scaled from 2 to N dimensions.) We then choose k closest neighbors and read off their "labels". Whatever label is most frequently encountered serves to classify our input point.

In the example from MLA, we have data about 1000 dates, each having three dimensions: number of miles traveled, grams of ice cream eaten, and hours of computer games played per month. Each datapoint has a label: how much our client liked her date on a scale from 1 to 3. Our goal is to take an input point (a potential date) and predict how well liked he will be by finding his k nearest neighbors from the training set. (Again, if you find the above description too sparse, there are many better tutorials elsewhere.)

My Julia and Python 3.4 code, translated from Peter Harrington's Python 2.7 code, as well as the dataset, can be found in [this](https://github.com/aflyax/Julia/tree/master/machine_learning_in_action/ch%202) Github repo. The assumption is that the dataset is in the folder from which you're running Julia (or IJulia).

First, we read in the dataset and store it into a DataFrame object:

```julia
using DataFrames, Distances, Gadfly

dating_df = readtable("datingTestSet2.txt", separator='\t', header=false);
names!(dating_df, [:miles, :games, :ice_cream, :opinion])
head(dating_df)
```

Out (first six rows — the "head" — of our data for preview):

```
| Row | miles | games   | ice_cream | opinion |
|-----|-------|---------|-----------|---------|
| 1   | 40920 | 8.32698 | 0.953952  | 3       |
| 2   | 14488 | 7.15347 | 1.6739    | 2       |
| 3   | 26052 | 1.44187 | 0.805124  | 1       |
| 4   | 75136 | 13.1474 | 0.428964  | 1       |
| 5   | 38344 | 1.66979 | 0.134296  | 1       |
| 6   | 72993 | 10.1417 | 1.03296   | 1       |
```

Next, we want to normalize the values to ensure that no dimension influences our distance calculation more than others:

```julia
function normalize(input_df::DataFrame, cols::Array{Int})
    norm_df = deepcopy(input_df)
    for i in cols
        norm_df[i] = (input_df[i] - minimum(input_df[i])) / (maximum(input_df[i]) - minimum(input_df[i]))
    end
    norm_df
end

norm_df = normalize(dating_df, [1:3]);
print(head(norm_df))
```

Out:

```
| Row | miles    | games     | ice_cream | opinion |
|-----|----------|-----------|-----------|---------|
| 1   | 0.448325 | 0.398051  | 0.562334  | 3       |
| 2   | 0.158733 | 0.341955  | 0.987244  | 2       |
| 3   | 0.285429 | 0.0689252 | 0.474496  | 1       |
| 4   | 0.823201 | 0.62848   | 0.252489  | 1       |
| 5   | 0.420102 | 0.0798203 | 0.0785783 | 1       |
| 6   | 0.799722 | 0.484802  | 0.608961  | 1       |
```

Now we are ready for the kNN function. The function itself uses `Distances` package, which I imported in the first code block above.

<code data-gist-id="abcbea50f614c8479bc7" data-gist-hide-footer="true"></code>

Distance calculation happens on line 4; lines 7–9 enter the k nearest distances into a dictionary. If `prob=true` was passed, the function returns (lines 13–15) a dictionary with all labels and corresponding probabilities (the frequency of each label among the neighbors). Otherwise, line 11 returns the most frequently encountered label. (`indmax` returns the index of the highest member of the vector.)

That's really the core of the exercise. I also wrote two "wrapper" functions, one that calls `find_kNN()` over a range of datapoints(each compared to a training dataset), and the other that randomly splits the global dataset into training and test datasets over multiple trials and plots the accuracy of the individual trials (in green) and the average accuracy as of the trial (in red). (For the record, I still haven't figured out a good way to have custom labels on a Gadfly plot — I don't really want to use DataFrames for that).

The last two lines call the `plot_probs()`, having it run ten trials (each time test data set being 25% of the whole dataset).

```julia

using DataFrames, Distances, Gadfly

function mass_kNN(test_array::Matrix, train_array::Matrix, test_labels::Vector, train_labels::Vector, k::Int64)
    
    count_correct = 0
    for i in 1:size(test_array,1)
        kNN_label = find_kNN(test_array[i,:][:], train_array, train_labels, k = k)
        kNN_label == test_labels[i] ? count_correct += 1 : "pass";
    end    
    return count_correct / size(test_array, 1)
end

function plot_probs(input_array::Matrix, test_portion::Float64, cols::Vector, num_trials::Int64, k::Int64)
    success_rate = Float64[]
    mean_success = Float64[]
    
    data = convert(Matrix{Float64}, input_array[:,cols])
    labels = convert(Vector{Int64}, input_array[:,cols[end]+1])
    
    for i in 1:num_trials
        n = size(input_array,1)
        is_train = shuffle([1:n] .> floor(n * test_portion))
        train_array, test_array = data[is_train,:], data[~is_train,:]
        train_labels, test_labels = labels[is_train], labels[~is_train]
        
        print("*")
        push!(success_rate, mass_kNN(test_array, train_array, test_labels, train_labels, k))
        push!(mean_success, mean(success_rate))
    end

    println("")

    x = [1:num_trials]
    
    p = plot(x=x, y=success_rate, Geom.line, Geom.point, Guide.xticks(ticks=x),
        Guide.xlabel("trial"), Guide.ylabel("accuracy"), Guide.title("kNN runs"), 
        Theme(default_color=color("seagreen")))
    q = layer(x=x, y=mean_success, Geom.line, Geom.point,
        Theme(default_color=color("indianred")))
    
    append!(p.layers, q);
    display(p)
    
end

norm_array = array(norm_df);
@time plot_probs(norm_array, 0.25, [1:3], 10, 3);
```

Out:

`elapsed time: 0.485163728 seconds (216355912 bytes allocated, 45.36% gc time)`
<object type="image/svg+xml" data="\images\kNN.svg"></object>

Similar [code](http://nbviewer.ipython.org/github/aflyax/Julia/blob/master/machine_learning_in_action/ch%202/dating_kNN_py.ipynb) in Python runs for about 0.95 seconds. (Note: I got some help from StackOverflow gurus to optimize my function a bit. Apparently, list comprehensions slow down Julia quite a bit. Re-writing them as `for` loops sped up my code about twice.)