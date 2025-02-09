# Harvest_Date_Prediction
## A prediction model on the time needed for harvest🥬
At the beginning of this project, the only data we have are: 1. Real-time data of air temperature, humidity, and luminosity from farms; 2. Some basic knowledge from the web and farmers is that plant growth is a sigmoid curve. For butter lettuce (the plant species used in this project), the time for harvest needed will be 35 days (for winter) and 50 days (for summer) respectively, and the plant should be around 14cm tall when it is ready for harvest. Those are all the details we have, but how can we make a prediction model for predicting the time needed for harvest under any specific condition combination (e.g. 10 degrees Celsius, 70% humidity, and 8000 lux luminosity)? This is what the project is about.

**Data Analysis** (Can be found in xxx)
We started by finding a photo of the sigmoid curve on plant growth first, but the problem is that the graph has no data points on it, and we needed those data points to make a CSV file so that we could fit in the 35/50 days constraint to it (we have to know how tall the plant is on each day after planting it, e.g. on day 3 it is 1.7cm tall, on day 35 it is 14cm tall, etc.). Therefore, we thought of a method of using OpenCV to detect some data points on the graph to deal with the problem. It worked for some points, but more than enough so that we can just plot those remaining points manually by Matplotlib.

*The blue dots are the detected points on the line.*
![image](https://github.com/user-attachments/assets/5e42b2f2-fb49-4ef3-864f-bc25e292d1de)
*And we plot the remaining points manually, just to confirm the coordinates are correct.*
![image](https://github.com/user-attachments/assets/0ac6b226-809a-4a70-86b4-6d002785b9fa)

So what can we do with those coordinates? They are pixel coordinates based on the picture that we've used, but it doesn't tell us anything about the plant height vs. date relation (they've told us something about the "ratio" though). Therefore, we rescaled these data points and made 2 new graphs for the summer curve and winter curve respectively. This time with the new graphs, we can also make a CSV file for the coordinates on this new graph, something like x-coord (Day) = 1 and y-coord (Plant height) = 0cm. We have also found the polynomial fit for this curve as intuitively we thought that it looked like a 3rd-degree polynomial function graph as well, and this polynomial function can be found in xxxxx.

From another file, we also have an amended dataset file for the air temperature, humidity, and luminosity real-time data in the year 2023. 
