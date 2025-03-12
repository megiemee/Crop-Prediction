# Crop-Prediction
A machine learning designed to recommend crop types to farmers based on the surrounding temperature, amount of rainfall, quality of the surrounding air, and quantity of pesticides in order to maximise yield.

**This was a school project done in a group of 5 people, and this README file is adapted from a presentation on the project.
# Main focus of this model
How might we leverage machine learning to recommend crop types to farmers based on the surrounding temperature, amount of rainfall, quality of the surrounding air, and quantity of pesticides used to maximise yield?	
# Data preparation

![image](https://github.com/user-attachments/assets/2634f300-cba9-4957-8d0c-d8877faba739)
![image](https://github.com/user-attachments/assets/bfa2e2d0-6e34-4f1c-b4b9-e33c4d68030b)

The separate csv files were loaded into dataframes and null values were replaced to prevent errors when handling the data later on. 

![image](https://github.com/user-attachments/assets/19e3a182-5884-470b-9f7a-b93655a2920f)

As the range of years, countries, and crops are different for each dataset, we only took data from the common countries across the datasets, chose 6 staple crops, and limited the year to be between 1990 and 2020.

![image](https://github.com/user-attachments/assets/b7e43532-2297-4ba7-8ef6-af733a72f6eb)

For each crop, we matched the data of the independent variables from the separate datasets, which are the four agricultural production factors, to the dependent variable, which is the yield, based on their corresponding country and year and stored them in a dictionary.

![image](https://github.com/user-attachments/assets/87a54b8f-72fd-48e1-a599-b495a6caf2d1)

This dictionary was then converted into a dataframe and rows with missing values were removed to prevent them from skewing the results.

# Data analysis
By plotting the data, we observed a few trends.

![image](https://github.com/user-attachments/assets/df7df28b-593f-40a1-bea2-607a1ca53f05)

Firstly, in general, crop yield tends to decrease with respect to all of the independent variables except for temperature.

![image](https://github.com/user-attachments/assets/3247bdf7-e875-4721-816f-1a8627ef5f03)

We decided to further investigate the crop yield with respect to temperature for each crop and realized that yield is not linearly related to temperature. Instead, it tends to peak at differing temperatures for different crops. However, due to the importance of temperature in affecting yield, we decided not to leave it out of the model.

![image](https://github.com/user-attachments/assets/14fb58bf-90c2-40cb-9e46-0a6dcaa1a97e) vs  ![image](https://github.com/user-attachments/assets/0f8ba53c-591f-4500-b5d9-1e36689f2dcb)

Lastly, an interesting observation was that despite the vastly different relationships that temperature and precipitation have with respect to yield, 

![image](https://github.com/user-attachments/assets/cf3447eb-57a9-4771-99e0-e2d20e0d8a43)

Temperature and precipitation seem to be related as precipitation levels tend to rise as temperature rises.

# Model 


 ![image](https://github.com/user-attachments/assets/c294a2dd-08a2-4127-9106-335c7c7c1f0e)

To ensure consistency in the test results, we fixed the random state, distribution between test and feature data sets, number of iterations of gradient descent the model goes through, and the rate at which gradient descent is implemented.

 ![image](https://github.com/user-attachments/assets/8bddddc1-6e4a-4610-9539-9739a5651ddd)

With those variables set, we move on to the actual training of the model. While yield is dependent on crop types, it is a variable that we are not able to quantify. Hence, we trained a separate multiple linear regression model for each crop. We start by retrieving the relevant columns of data for the specified crop, then splitting that data into test and training sets. 
 

 ![image](https://github.com/user-attachments/assets/9760e039-10ab-4ad2-b14c-351526decf88)

![image](https://github.com/user-attachments/assets/5e50abc9-169e-4b47-a416-95d27ae57adf)

![image](https://github.com/user-attachments/assets/82fffb71-1e30-4371-b81d-8be8ca60a7d8)
 
After that, we scaled the training data. This step was needed as when we initially tried to train the model using the raw data, it led to an error as the values in the columns were too large for the program to compute. Scaling the values using a common integer like 1000 or using the log function were also not possible as the range of values between columns were too large and the temperature variable contained negative values. Therefore, we settled on scaling the values with the column means. 

![image](https://github.com/user-attachments/assets/a33c7d8e-e4cf-4fda-8972-e9148103ece7)


![image](https://github.com/user-attachments/assets/37fb3d50-64ad-41ae-aa96-60bb5110b513)


With that, we trained the training dataset using gradient descent and obtained the beta values, before using those beta values to conduct predictions on the test data set.

![image](https://github.com/user-attachments/assets/d4aff698-665a-4b96-8242-5604b7f26f12)

 # Model results
 
With those results, we then evaluated each crop’s model accuracy by calculating the root mean square value as well as the ratio of the root mean square value to the mean yield of the crop.

![image](https://github.com/user-attachments/assets/fd4bf92d-111a-4a74-b053-e5a6a2ac2cc3)

Overall, while individual RMSE values seem quite low, it is actually relatively large in comparison to the yield values, which leads us to believe that the model is not the most accurate, likely due to the fact that there are other qualitative independent variables (e.g. skill level of farmers, farmers' familiarity with the crops) which affect the dependent variable that were not taken into account. 
By comparing the individual ratio of RMSE to mean yield for each crop, we can see that it is higher for some which shows the model’s varying accuracy between crops.

# Improvement

 ![image](https://github.com/user-attachments/assets/324c8a44-98d0-482d-998d-25ae8e3c1e54)

With this, we then decided to improve on the model by changing the data processing technique used and scale the data using z-normalization.

![image](https://github.com/user-attachments/assets/130a2be1-a870-440d-80de-3ee386304656) ![image](https://github.com/user-attachments/assets/7baa3623-a5c3-4daf-9cdf-0d12c2944726)

Lastly, we calculated the new RMSE values and compared the differences. From this, we can see that the RMSE value decreased for all crops, which means that the average deviation of the predicted versus actual yield has decreased across the board, indicating that scaling the values with Z Normalization has increased the accuracy of all models.
Individually, the RMSE value and ratio decreased by differing magnitudes for the crops, which shows that z normalization has varying effectiveness on the different models, likely because some datasets had more outliers than others since z normalization reduces the impact of outliers.

# Conclusion
In conclusion, our model can roughly estimate the yield of crops with varying accuracy based on our selected independent variables, which should be good enough to give a recommendation on the crop since the difference between yields of different crops tend to differ more greatly than the RMSE value. However, it is important to note that the actual predicted yields are not very accurate. To boost its accuracy, more independent variables can be explored for the various crops to find variables with a stronger relationship to the yield of the specific crops.



