# stock_market_prediction_challenge
## Project Overview
In the financial market, the movement of stock prices are never entirely predictable. Having a reliable 
way to forecast stock prices is crucial for most trading strategies. Many techniques have been 
developed to make predictions, ranging from fundamental techniques that involve analysing 
company financial statements to technical analysis which utilises indicators derived from the prices. 
Different kind of market participants perform trades on the stock which causes the prices to vary 
intraday. These price movements form a non-stationary time series data. Although the series formed 
are often very noisy, one can extract signals from historical price trends in some occasions and 
forecast future prices. 

## Problem Statement 
This project is based on the Stock Market predictions on asset returns from a proprietary dataset 
provided. We make use of artificial neural networks to identify hidden patterns in the data. 

## Metrics
Submissions is done on weighted mean absolute error, where each return being predicted is 
compared with the actual return, scaled by its corresponding weight: 


![Screenshot 2024-03-05 185442](https://github.com/ahmedali1102/stock_market_prediction_challenge/assets/162327449/0f82ff4f-7da8-4c44-aa91-300f8b8dac37)


where 𝑤𝑖 is the weight associated with the return 𝑖, 𝑦̂𝑖 is the predicted return, 𝑦̂ 𝑖 is the actual return, 
and 𝑛 is the number of predictions. These columns are discussed more thoroughly in the next 
section. A total of 62 returns has to be generated by our model for each 5-day window. 

## Exploratory Visualization 
The distribution of daily returns is wider than the intraday returns, as one would have expected. Daily 
returns in the samples range from -60% to 80%, whereas intraday returns only range from about +/-
20%, this is shown in Figure .

The time series data formed by the given asset returns as shown in Figure 3 are quite noisy. The aim of our model is the make predictions on the returns starting from Ret_121, based on the features and historical returns as illustrated in the given Figure.
![stock 1](https://github.com/ahmedali1102/stock_market_prediction_challenge/assets/162327449/332f1bd4-d2a9-40a9-ab59-6d72476a5bc3)

# Algorithms and Techniques 
Regression models can be effective for tasks where the relationship between features and targets is 
relatively straightforward and doesn't involve complex temporal dependencies. This problem involves 
capturing patterns and relationships that depend on the sequence and timing of events hence using 
LSTM model for this project.
The problem is being structured as a regression, a stock returns prediction model is created using 
artificial neural networks, where the 25 anonymous features and various historical returns are being 
fed as inputs, and the output will be 60 ticks of intraday minute returns, together with 2 daily 
returns. An LSTM model is employed due to its effectiveness on time series data. Depending on the 
testing results the model may be split into versions to specialise in predicting intraday and daily 
returns separately, details on this is included in the next section.

## Refinement 
Considerable effort has been made to improve the data processing code to a state where it is both 
efficient and utilises the available functions in the pandas and numpy library. The model has been 
trained in 2 part with 2 hidden layer of 32 neurons, 16 neurons, then 8 neuron with 1 output layer 
where it didn’t helps to refine the model ,same applied with the 4 hidden layers. 
In the final version of our models, both the intraday and daily prediction model have 200 neurons in 
the LSTM layer, with 2 Dense layers (each with their corresponding dropout layers at 20%), using the 
Adam optimser and Relu activation function. In particular, a marking layer has been added in front of 
the LSTM layer to mask any inputs that are zero-ed out at the pre-processing stage to ensure a fair 
representation on the unavailable data points. As the evaluation metric for this competition is 
weighted mean absolute error, mean absolute error is chosen as the loss function during the model 
training process. Prediction results from these final models are discussed in the next section.

## IV. Results
![stock 2](https://github.com/ahmedali1102/stock_market_prediction_challenge/assets/162327449/60879fbc-7b9f-4077-a7c3-8f2bf3682341)

The models are trained across 3 epochs, with a batch size of 500, since the validation loss does not 
improve beyond the first epoch. Mean absolute error is chosen as the loss function, our intraday 
model achieved an error of 0.0006341, whereas our daily model achieved an error of 0.0155. These 
values are very close the metrics reported by the validation set, which has mean absolute errors of 0.0006326 and 0.0154 respectively. 
The number of units in the dense layers are two times that of the LSTM neurons. The intraday return 
model is configured to have a higher number of neurons because of the larger input and output sizes 
compared to the daily model. Increasing the number of neurons or the number of Dense layers do 
not have observable effects on the MAE of the validation set. The choice of optimiser do not have 
much effect on the MAE either, as long as enough epochs is being run to ensure the model is 
sufficiently trained.

The final models have not adequately solved the problem as their prediction capabilities are only 
matching the zero-benchmark. One should note that although the zero-benchmark may appear to 
give a relatively low error, it can never be used in a real-world trading strategy.

## Conclusion
As an attempt to gain further insight from our results, it is not too surprising that our model 
converges to the zero-benchmark solution, given that the zero benchmark can provide a rather 
robust estimate. Further, since the competition is evaluated in weight mean absolute error, over a 
long period of time, zero-benchmark can in fact reach a decent local minimum in the given stock 
market data. A number of stock price movement illustration has been included in Figure 7 to 
demonstrate this.
![stock 3](https://github.com/ahmedali1102/stock_market_prediction_challenge/assets/162327449/c14c6bdd-28f8-4fe6-b844-bb78beeb0d4f)
