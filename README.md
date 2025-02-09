# Harvest_Date_Prediction
## A prediction model on the time needed for lettuce harvest🥬
At the beginning of this project, the only data we have are: 1. Real-time data of air temperature, humidity, and luminosity from farms; 2. Some basic knowledge from the web and farmers is that plant growth is a sigmoid curve. For butter lettuce (the plant species used in this project), the time for harvest needed will be 35 days (for winter) and 50 days (for summer) respectively, and the plant should be around 14cm tall when it is ready for harvest. Those are all the details we have, but how can we make a prediction model for predicting the time needed for harvest under any specific condition combination (e.g. 10 degrees Celsius, 70% humidity, and 8000 lux luminosity)? This is what the project is about.

**Data Analysis** (Can be found in `Lettuce G/Codes/Data Analysis (Wing Lam Garden).ipynb`)
We started by finding a photo of the sigmoid curve on plant growth first, but the problem is that the graph has no data points on it, and we needed those data points to make a CSV file so that we could fit in the 35/50 days constraint to it (we have to know how tall the plant is on each day after planting it, e.g. on day 3 it is 1.7cm tall, on day 35 it is 14cm tall, etc.). Therefore, we thought of a method of using OpenCV to detect some data points on the graph to deal with the problem. It worked for some points, but more than enough so that we can just plot those remaining points manually by Matplotlib.

*The blue dots are the detected points on the line.*
![image](https://github.com/user-attachments/assets/5e42b2f2-fb49-4ef3-864f-bc25e292d1de)
*And we plot the remaining points manually, just to confirm the coordinates are correct.*
![image](https://github.com/user-attachments/assets/0ac6b226-809a-4a70-86b4-6d002785b9fa)

So what can we do with those coordinates? They are pixel coordinates based on the picture that we've used, but it doesn't tell us anything about the plant height vs. date relation (they've told us something about the "ratio" though). Therefore, we rescaled these data points and made 2 new graphs for the summer curve and winter curve respectively. This time with the new graphs, we can also make a CSV file for the coordinates on this new graph, something like x-coord (Day) = 1 and y-coord (Plant height) = 0cm. We have also found the polynomial fit for this curve as intuitively we thought that it looked like a 3rd-degree polynomial function graph as well, and this polynomial function can be found in `Lettuce G/Datasets/Plant growth formula.pdf`.

From another file, we also have an amended dataset file for the air temperature, humidity, and luminosity real-time data in the year 2023 (we have already dropped the unwanted data and restructured the data). Hence, we just merge the amended data with "the plant height vs days after planting CSV file" which we have just created, making it into a finalised dataset file `Lettuce G/Datasets/finalised_df.csv` for our deep learning model. We then prepare the sliding window as a usual practice. 

*Note that we used linear regression to make a multivariate regression model (with multiple independent variables). Due to the lack of record of the plant's increase in height per day, we have to "guess" and assign a "predicted harvest time needed" for each combination of air temp, humidity and luminosity (e.g. if it is 10 degrees Celsius, luminosity 8000 lux and humidity 70%, then the time needed for harvest would be 35 days, and if it is 41 degree Celsius, 14000 lux luminosity and 81% humidity, then the time needed for harvest would be 50 days --> all of these are just arbitrary guesses that we have made based on the past experiences of the farmers: usually 35 days needed for winter and 50 days for summer)*
![image](https://github.com/user-attachments/assets/2ae70e99-c258-4668-8753-370cbbacf302)

**Building the Neural Network** (Can be found in `Lettuce G/Codes/Harvest_date_prediction_model.ipynb`)
After all those preparations, we used CONV1D (a 1-dimensional CNN) as our neural network architecture. If we treat those parameters as a bunch of vectors in 1D, then we can use it to recognise the data as a 1D graph pattern, thus giving us the predictions we wanted (CONV1D should work better/ easier to make than LSTM for multivariate cases). We've used Tensorflow here as our framework, and after training, we have found that Mean Absolute Error (MAE): 2.4103807054057667; R-squared (R²) Score: 0.237327069981358, which indicates that our prediction model is not good enough. We just have to make some changes to the model such that it can work better. 
![image](https://github.com/user-attachments/assets/b6f78b89-e9b0-43b4-b2e9-6f9aa0b983c6)

*Remark: Mean absolute error (MAE) is the average of the absolute differences between the predicted values and the actual values. e.g. if the actual time needed for harvest is 50 days but the prediction model showed 52 days, then MAE would be 2. The R-squared score is a statistical measure that represents the proportion of the variance for the dependent variable that's explained by the independent variables in the model. It ranges from 0 to 1 and we would like it to be as close to 1 as possible (more accurate).*

I have done something like adding additional CONV1D layers to help the model learn more complex patterns in the data, adding dropout layers to prevent overfitting etc. The new prediction model looks much better (Mean Absolute Error (MAE): 1.5044464559188941; R-squared (R²) Score: 0.7169538785253806) and I have saved this new model to `Lettuce G/Codes/LettuceG_model.keras`. 
![image](https://github.com/user-attachments/assets/1938ad96-4718-4e68-a8ac-9a830e96b38f)
![image](https://github.com/user-attachments/assets/1fac6c69-509d-4f8a-8d8e-08aaf7f0c89f)

At last, we just have to change some values for those parameters in that `yesterdays_data` array, and we can already make our prediction on the time needed for harvest (the input should follow the order: `Air temp, humidity, luminosity`). 

For this prediction model, it is just an early-age prototype so there are many flaws in it. Please feel free to contact me if you have any thoughts on it 👽 thanks!
































