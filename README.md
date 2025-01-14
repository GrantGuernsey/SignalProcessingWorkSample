# Approach

## Distance Time Warping (DTW) Approach

Dynamic Time Warping (DTW) is a technique used to measure the similarity between two temporal sequences that may vary in speed. This approach is particularly useful for time series data where the time axis may not be perfectly aligned.

### Steps:

1. **Load Data**: Load the experimental and reference data from CSV files.
2. **Preprocess Data**: Generate sliding windows for each reference signal to handle partial overlaps.
3. **Calculate DTW Distance**: For each experimental signal, calculate the DTW distance to each reference window.
4. **Find Best Matches**: Identify the top 2 matches with the lowest DTW distances.
5. **Estimate Probabilities**: Estimate the probability of matching based on the DTW distances.
6. **Measure Runtime**: Measure the per-experimental-signal runtime required for processing.

### Performance and conclusion
The per-experimental runtime result was an average of 0.000788 seconds. Although this result is super quick especially considering this is python, using this approach is not scalable. As the number of reference signals increases, so will the runtime. This is because we are calculating the DTW for each of the window sizes that we have available, window sizes being the length of the experimental data. This approach would also be less effective if the experimental data were smaller, as it would require many more comparisons. Overall, this is a good solution if there is a low number of reference signals, the reference signals are fairly short, and the size of the experimental data is not small compared to the size of the reference signals.

## Machine Learning Approach (Random Forest)
A machine learning approach using a Random Forest classifier can also be employed to find the best matches. This approach involves extracting features from the signals and training a classifier to predict the best matches.

### Steps:

1. **Extract Features**: Use FFT and statistical features for each signal.
2. **Train Model**: Train a Random Forest classifier on the reference windows.
3. **Predict Matches**: Use the trained model to predict the top 3 matches for each experimental signal.
4. **Refine Matches with DTW**: Calculate DTW distances for the top 3 matches and select the best match. This is to eliminate any faulty matches (such as finding a better match in the wrong direction).
5. **Measure Runtime**: Measure the per-experimental-signal runtime required for processing.

## Performance and conclusion
The per-experimental runtime result was an average of 0.003951 seconds, and the average distance (how good of a match was found) was about 48% greater than the DTW approach (the greater the value the worse the match). The average runtime is still fast, but it is approximately 5 times slower than the DTW approach. The benefit of this approach is its scalability. The runtime will increase by a significantly smaller amount if we have a large number of reference signals, long reference signals, and any size of experimental data. In trade-off for runtime efficiency, we gain the ability to scale this application without experiencing the same detrimental effects on runtime.
