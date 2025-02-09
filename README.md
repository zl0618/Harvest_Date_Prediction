# Harvest_Date_Prediction
## A prediction model on the time needed for harvest🥬

At the beginning of this project, the only data I have are: 1. Real-time data of air temperature, humidity and luminosity from farms; 2. Some basic knowledge from the web and farmers is that plant growth is a sigmoid curve. For butter lettuce (the plant species used in this project), the time for harvest needed will be 35 days (for winter) and 50 days (for summer) respectively, and the plant should be around 14cm tall when it is ready for harvest. Those are all the details I've got, but how can I make a prediction model for predicting the time needed for harvest under any specific condition combination (e.g. 10 degrees Celsius, 70% luminosity and 8000 lux luminosity) out of those details? This is what the project is about. 

I started by finding a photo of the sigmoid curve on plant growth first, but the problem is that the graph has no data points on it, and I needed those data points to make a CSV file so that we could fit in the 35/50 days constraint to it (we have to know how tall the plant is on each day after planting it, e.g. on day 3 it is 1.7cm tall, on day 35 it is 14cm tall etc), so I have thought of a method of using OpenCV to detect some data points on the graph to deal with the problem. It worked for some points, but more than enough so that we can just plot those remaining points manually by Matplotlib. 

*The blue dots are the detected points on the line.*
![image](https://github.com/user-attachments/assets/5e42b2f2-fb49-4ef3-864f-bc25e292d1de)
*And we plot the remaining points manually -> just to confirm the coordinates are correct.*
![image](https://github.com/user-attachments/assets/0ac6b226-809a-4a70-86b4-6d002785b9fa)

After that, we have 
