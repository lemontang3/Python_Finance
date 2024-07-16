# Python_Finance

## Intraday Strategies

#### Developing, Coding, and Backtesting Strategies using Python
<img width="820" alt="Screenshot 2024-07-16 at 8 04 17 AM" src="https://github.com/user-attachments/assets/24757f0c-de37-4d79-a679-11547cbcd160">   

Importing different packages: Pandas, Numpy, Math, MatPlotLib, ADF Test, DateTime, YFinance

##### a. Strategy 1: Gap Hunting
<img width="823" alt="Screenshot 2024-07-16 at 8 04 52 AM" src="https://github.com/user-attachments/assets/6ce1138e-4d29-4154-a1d7-71aa796fc024">   

Firstly, I downloaded the data for the past one month for S&P 500 (SPY) along with the volatility index (VIX). VIX presents the expected annualized volatility for the next 30 days so I divided the amount by the square root of the active trading days to find the expected daily volatility. This value would then further be used to calculate the stop loss and target price for our trades.
<img width="826" alt="Screenshot 2024-07-16 at 8 05 18 AM" src="https://github.com/user-attachments/assets/ca172f49-520a-4747-bbd7-b384b8237416">  

The gap was calculated by subtracting the previous day’s closing price by the current day’s opening price.

<img width="828" alt="Screenshot 2024-07-16 at 8 05 41 AM" src="https://github.com/user-attachments/assets/43db8fe2-8991-4479-991c-82fcd8528ceb">   

I used the standard deviation of all the gaps to identify abnormal gaps and when we could actually take advantage of a gap up or a gap down; two standard deviations suggest that the gap is larger than 90% of the data given and therefore more chances that it will lead to more profits. I then created a column where Gap Up and Gap Down columns were 1 when the gap was outside one standard deviation and Gap Up 2 and Gap Down 2 where 1 when the gap was outside two standard deviations.
After this, I iterated through my dataframe finding places where there was a Gap Up (Down) and further checked if there was a Gap Up 2 (Gap Down 2). After this, I took a short (long) position in the market and the number of shares depended on the nature of the opening (1 or 2). This value for stored in the Positions Column and the entry price was recorded. Using the .index feature of Pandas, I collected information regarding the date in regard, this in turn was inputted into a new dataframe which contained data regarding the 5 minute intervals. It is because a 1 day interval only reflects the open and close price whereas to make more profitable trades, it is essential to know the open and close for shorter time frames such as 5 minutes. I then proceeded on a strategy to deduce the exit. I thought it was best to use the volatility index for this purpose. For this I added the volatility to 100 and multiplied it to the entry price to find the upper limit and subtracted volatility from 100 and multiplied it to the entry price to find the lower limits. These limits acted as stoploss and target for the different scenarios. If price failed to reach any of these limits, the closing price of the day was set the exit price however, in a more extensive model, more sophisticated approach to deducing the exit pricing could be used. The code and the output for the same are presented on the next page.


<img width="825" alt="Screenshot 2024-07-16 at 8 06 13 AM" src="https://github.com/user-attachments/assets/b1ed1bb7-669b-4031-b203-007de218651f">  

In 6 trades, out of which the majority were of 1 position, I was able to scalp 0.882$ profit per trade. I believe this is quite promising because considering a stable ETF like the SPY, utilizing the gaps we were able to generate profits healthily which could be amplified by trading more volume or increasing the number of positions taken per trade.

<img width="776" alt="Screenshot 2024-07-16 at 8 07 41 AM" src="https://github.com/user-attachments/assets/5967f0c1-d217-441c-b968-2da61eaa4835"> 

Code for Gap Hunting

<img width="779" alt="Screenshot 2024-07-16 at 8 08 31 AM" src="https://github.com/user-attachments/assets/34d1732f-42a9-477d-ba6f-de14d07eaba4">   

Final DataFrame Table for Gap Hunting

##### b. Strategy 2: Momentum Following

<img width="777" alt="Screenshot 2024-07-16 at 8 11 20 AM" src="https://github.com/user-attachments/assets/1c802ac1-a01d-4077-b32e-9bf9acc8f594">   

<img width="781" alt="Screenshot 2024-07-16 at 8 11 36 AM" src="https://github.com/user-attachments/assets/64c9963e-7c45-4fcd-bb0e-bd478c5aa212">   

A Plot for the SPY with the Simple Moving Averages over the period of a month and a year
<img width="777" alt="Screenshot 2024-07-16 at 8 11 57 AM" src="https://github.com/user-attachments/assets/80069cd9-98df-419a-8e50-2834268acb55">   

According to momentum trading, a stock tends to follow its long term trend and setbacks and small corrections present an ideal opportunity to implement day trading strategies. To do this quantitatively, I calculated the percentage change between consecutive closing prices of both the long term simple moving average and the closing price of the SPY.
<img width="778" alt="Screenshot 2024-07-16 at 8 12 21 AM" src="https://github.com/user-attachments/assets/870d15d1-6fec-436c-b374-2e16cf3baad9">   

It is also necessary to involve the Gap Hunting process. It is because if the trend is upwards, and a stock fell the previous day, there are chances that the mean reversion happened during extended hours which is not reflected on the market data provided and the market opens gap up the following day. Taking a trade without this information might lead to losses. This would ensure that there are no gap ups or gap downs in the direction of our preferred trading direction and we do not enter a trade if there are.
<img width="776" alt="Screenshot 2024-07-16 at 8 12 52 AM" src="https://github.com/user-attachments/assets/a5561cb8-b353-430a-a05a-642def0fe9f9">   

This code filters out the data for where the change in SMA is positive, however the SPY had fallen and there was not a gap up opening the following day. It also checks for data where the SMA % Change is negative, and SPY increased the previous day, however there was not a gap down day following it.

<img width="777" alt="Screenshot 2024-07-16 at 8 13 30 AM" src="https://github.com/user-attachments/assets/f49ec931-1f5b-4c7c-96df-002784948f32">   

<img width="778" alt="Screenshot 2024-07-16 at 8 13 44 AM" src="https://github.com/user-attachments/assets/8ca62747-666d-4438-a7d0-0d56696dbd2c">   

<img width="776" alt="Screenshot 2024-07-16 at 8 14 02 AM" src="https://github.com/user-attachments/assets/66f6fdf6-011f-46bb-9f09-54b43b402b6d">  

Over a period from 2022, the model took 256 trades, each of a position of 1 and yielded $0.045 in profit per trade. The low profit suggests that there might be something lacking in my model but it is also a signal that in the long run it is a profit yielding strategy if we ignore the transaction fees and slippage. To better the same strategy, I conducted some literature review and found that it is important that we use a test known as ADF test to find out if a series has a unit root, and then we could modify our data to make it stationary so that the historical average stays constant. Implementing the strategy then could yield better results.

##### c. Strategy 2: Momentum Following (Z-Score)

<img width="778" alt="Screenshot 2024-07-16 at 8 14 46 AM" src="https://github.com/user-attachments/assets/de5deed1-746c-486b-9346-c574c19a1bbf">   

Conducting the ADF test using adfuller, I found out that the Closing price of the SPY was in fact non-stationary. I then found out the closing difference and used that to calculate the ADF Statistic, p-value, critical values for the new series and this indeed came out to be stationary.
<img width="776" alt="Screenshot 2024-07-16 at 8 15 14 AM" src="https://github.com/user-attachments/assets/2df90d37-bf04-4656-beae-cde1c1a9103c">   

In mean reversion trading, the Z-score helps to determine how far the current price is from its historical average (mean). When the Z-score is high (positive), it indicates that the price is significantly above the mean, suggesting that it may revert back down towards the mean. Conversely, when the Z-score is low
(negative), it indicates that the price is significantly below the mean, suggesting that it may revert back up towards the mean. I used the entry threshold as 2 and the exit threshold as 0. Plotting the graph, I derived this. To make this meaningful I had to quantify the data.

<img width="780" alt="Screenshot 2024-07-16 at 8 15 36 AM" src="https://github.com/user-attachments/assets/7d549f90-dfd0-4125-8cb0-6432577d05c0">

<img width="778" alt="Screenshot 2024-07-16 at 8 15 51 AM" src="https://github.com/user-attachments/assets/1f753229-7458-4bc2-9385-6aa0757ed6e1">  

This strategy is more like swing trading where the holding periods are typically longer than a day therefore they might not fall directly under the purview of day trading strategies but the z score threshold might be decreased to fit intraday levels as well. Note there were some positions left uncatered to because there was not an appropriate exit signal to exit the trade.
<img width="777" alt="Screenshot 2024-07-16 at 8 16 23 AM" src="https://github.com/user-attachments/assets/153aee9d-b3f3-4642-8518-9fec4d533e1d">   

There were 96 trades taken over the span of 5 years and the profitability was an impressive $4.104 dollars. This is with 4 positions left which puts in a really good positions and speaks up on the effectiveness of this strategy.

##### d. Strategy 3: Long Short Term Memory Approach
Upon reading various articles on Medium and QuantInsider, I learned about this ML RNN which is very effective with sequential datasets, exactly what we are using.

<img width="779" alt="Screenshot 2024-07-16 at 8 18 14 AM" src="https://github.com/user-attachments/assets/1667c063-b555-44cf-8754-f187f8711048">  

To start with the LSTM Model, the MinMaxScaler was imported to scale the data such that it can be fit into the model. Keras Sequential Model: The Sequential model is a linear stack of layers. LSTM Layer: The LSTM layer is used to create an LSTM network, which is suitable for sequence prediction problems. It can maintain long-term dependencies through its cell state and gates. Dense Layer: The Dense layer is a regular fully connected layer, which is used for outputting predictions or creating complex feature representations. After fetching the data for SPY, I plotted the data as show below.

<img width="779" alt="Screenshot 2024-07-16 at 8 27 34 AM" src="https://github.com/user-attachments/assets/b40526e0-4ba8-49c2-9f42-1c6588eb2418">   

I then trained my model on around 80% of the given data and used the 20% of the remaining to test the predictions of the model. If the model accurately predicts the data, we can then use it to give us insights into the market movement on those days and use that to take advantage of day trading.

<img width="779" alt="Screenshot 2024-07-16 at 8 28 00 AM" src="https://github.com/user-attachments/assets/594330d5-beb4-486b-adbf-e2a4563d36e3">   

To do this, first I set the training data set length to 0.8 of the entire data set and then I transformed the data using the MinMaxScaler to be in values between 0 and 1. The scaled data was then trained from 0 to the training length for all the columns.

<img width="777" alt="Screenshot 2024-07-16 at 8 28 23 AM" src="https://github.com/user-attachments/assets/64060c7b-17f1-4a9c-9cef-fbd3c0f15611">   

The first 60 values were added to the x train data and the 61st value was added to the y train data. This were then converted to numpy arrays so that the model can handle the data and the x was reshaped to be 3 dimensional because LSTM models utilize 3 dimensions.

<img width="777" alt="Screenshot 2024-07-16 at 8 29 52 AM" src="https://github.com/user-attachments/assets/318e94c1-d855-48a5-bcd9-d7180f2f3b24">
<img width="776" alt="Screenshot 2024-07-16 at 8 30 02 AM" src="https://github.com/user-attachments/assets/9079c1c5-de82-4c53-ac0e-b19129523833">
<img width="774" alt="Screenshot 2024-07-16 at 8 30 17 AM" src="https://github.com/user-attachments/assets/a17368d0-4860-45b9-b210-c6a93c596432">
<img width="777" alt="Screenshot 2024-07-16 at 8 30 27 AM" src="https://github.com/user-attachments/assets/f402009a-be1b-4c83-b026-b203f9cec54d">
<img width="778" alt="Screenshot 2024-07-16 at 8 30 38 AM" src="https://github.com/user-attachments/assets/9d354a2b-5737-4746-956e-4d51ac60cd84">

<img width="780" alt="Screenshot 2024-07-16 at 8 29 04 AM" src="https://github.com/user-attachments/assets/eaff864b-c5c1-4635-8a1e-2f9b36ce0e85">  

Upon plotting the data, we can see that the predicted values are indeed very close to the actual values with a Root Mean Square Error of about 0.10. This suggests that this model could be effectively used to predict the price movements and trade. I then proceed to code further to quantify this.
Using a similar approach I had used in the previous methods, I loop through the dataframe finding places where the value of the predictions had increased. If the value increased by more than 1 in dollar value I took a position of 2 otherwise a change of 0.2 was followed by a position of 1. The same was done if the predictions indicated a fall in price. The exit price was set as the closing price. This could be updated in
future models to better improve the accuracy and profitability of the model.

<img width="779" alt="Screenshot 2024-07-16 at 8 31 05 AM" src="https://github.com/user-attachments/assets/6481ae41-bb15-41c7-a0ba-3cd3f675cc5b">  

Following is the tale of the data when I traded based on this model.

<img width="781" alt="Screenshot 2024-07-16 at 8 31 25 AM" src="https://github.com/user-attachments/assets/4fef093a-b6a5-461d-88be-f0b9b902fea8">
<img width="777" alt="Screenshot 2024-07-16 at 8 31 37 AM" src="https://github.com/user-attachments/assets/1cc0576e-1a88-4ceb-8cff-e22b19f9c3cf">
<img width="778" alt="Screenshot 2024-07-16 at 8 31 48 AM" src="https://github.com/user-attachments/assets/703e8043-26c9-4a7d-bb81-9d91eaf94c4b">   

Out of 653 trades taken of multiple positions, this model generated approximately $0.1 per trade.

##### Risk Adjusted Returns
It is important to realise what risks we are bearing and are accepting with each investment. To do so I am going to use the Sharpe Ratio which is a widely used metric in finance helping investors and traders evaluate the risk-adjusted return. To do this I will measure the excess return (return above the risk-free rate) per unit of volatility (standard deviation of the returns). It would quantify the amount of additional return received for the extra volatility endured.
To understand the data presented in the tabular format below, a higher sharpe ratio indicates that the asset has provided better risk-adjusted ratios and more return for the same amount of risk or less risk for the same level of return. The Hit Ratio on the other hand would tell us the number of profitable trades taken as a fraction of all trades taken.

<img width="776" alt="Screenshot 2024-07-16 at 8 32 24 AM" src="https://github.com/user-attachments/assets/52399f60-6a06-493f-a5c2-49e627c8a7ea">








