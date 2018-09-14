
# BTakeHomeTask


A take home task, completed for a job application.


* Free software: MIT license


## Summary
-----------
The goal of this project is to impute missing data, forecast two weeks of sales and to also forecast the sales of a brand new line, with no data on it.

This project was completed primarily in Jupyter Notebooks, with Cookiecutter used to populate the repository. If there were more time this work could be turned into a python package.

## Initial Plan
------------
To complete this task the plan was to complete the following sub-tasks in roughly the following order:

* Impute the missing data for the Country, County, avg_prices
* Impute missing total products - this is the most important field
* Create an estimate for missing price band
* Investiagte which platforms have the best sales
* Forecast sales for the last two weeks
* Apply this forecast to the new line

## Notebook Guide
--------------
This section will provide a guide to the notebooks in btakehometask/notebooks/

### 1. Initial Exploration

This notebook just plots the data in different ways, mostly to get used to the data and to look at where the missing data is. Found that there was missing data in 4 fields: Country, County, avg_prices and total_products.

At this point I also noticed that the task was slightly wrong. The task had asked for us to investigate a new line between £20-30 in price, as the client didn't have a product in this price band. However upon investigation of the data I noticed that a product must exist in this price band and that the investigation was supposed to be done for a product in the £30-40 price band. The task provider confirmed this was true.


![Missing band £30-40](https://github.com/JoekingCooper/btakehometask/blob/master/images/productBrand.png)


This investigation also confirmed that 'total_products' was missing for the Mobile data. This is a key piece of information needed for forecasting sales in the last two weeks of August.

### 2. Imputing NaNs

To impute the NaNs for Country and County I thought about doing something complicated like using a dictionary to match the city and region with the correct country and county. This could be done for a real dataset, but considering the task was to forecast sales, independant of region/city/county/country it seemed silly to waste a lot of time on this. So instead I just imputed missing_country and missing_county to these columns if there was no value.

avg_price and total_products are more important to the overall calculation, so I took more care with these, especially total_products. avg_price is used to estimate the sales of the new £30-40 line and, when doing this, the NaNs are simply removed. However, for the forecasting, the overall average price was imputed. This field isn't used for forecasting anyway so really this is just a placeholder value. 

Imputing total_products was a little more involved. I realised that the order number must be less than the total products and we had the order number for all sales (including Mobile data). So I calculated the mean total_products per num_orders for the first month. Then I used a simple apply to make the missing total_products = num_orders * mean. This gave a good estimate of the total products sold.

### 3. Estimate New Line Sales

Here I've used the the avg_price to estimate the sales of a new line. This was basically done by fitting a Gaussian distribution to the abg_price histogram. This is quite crudely done initially and is returned to later to imporve the fit and to change the distribution. This is only needed after the forecasting is already done, so it's better to look at a few times. I looked at this before trying to forecast because I was concerned it might be difficult and timely and it may be needed to inform the forecasting process. Overall it was fairly simple to build.

### 4. Forecasting Process

Next I built the forecasting process, this mostly involved a lot of investigation and then coming up with three possible solutions:

1. Using the previous months sales and mapping them directly to the next months sales
2. Fitting a bespoke signal to the data and forecasting using this signal
3. Using a ready made package, such as facebook's prophet to help with the forecasting.

I that the third option would be fairly pointless, as it shows no real skill to use. Also prophet is not easily compatible with Windows machines, which is the operating system I am using. So I focused on the first two options.

In this notebook, I build the process to map the previous months sales to August. I actually thought this would be the easier of the two methods. But transforming and manipulating the data in this way is actually quite costly in terms of time. This process also involved removing the weekly trends from the data to just look at the monthly trend. 

### 5. Forecasting New Line

I matched the first two weeks of august to the first two weeks of July using a simple least-square fitting technique, giving a linear relation between sales.

This is a fairly crude method, but it is actually a fairly accurate method because of the limited amount of data avalible. I then used the model from notebook 3. to forecast sales for the new line. I noticed that the sales where being massively underestimated, so at this
point I returned to the new line model to work more on the accuracy of this model.

![Underestimated Sales](https://github.com/JoekingCooper/btakehometask/blob/master/images/underestimatedsales.png)

### 6. Estimate New Line Sales Return

I thought that a Gaussian distribution might not be the best idea when I was first modelling, however it is an easy model to fit and understand, with fewer parameters. This time I decided to try getting a Poisson distribution to work, I also noticed that I had used the wrong file in the first estimate and also given the model a skewed dataset, so it was fitting to £35 = 0 counts, when it should have been ignoring £35. With these two errors corrected and the Poisson working I ran the model again.

I also thought that the binned values were not representing the data very well so I reduced the number of bins and refit the distribution. Again, this gives a simple function that can be used to estimate the sales of the new line, assuming people purchase products in a roughly poisson distribution.

Notebook 5. now has a more accurate sales prediction.

![Underestimated Sales](https://github.com/JoekingCooper/btakehometask/blob/master/images/5estimatedsales.png)

### 7. Forecasting with a Bespoke Model

Now forecast with the second method, where I model the current dataset and try to predict using that model. I decided to fit multiple sine functions to the data, to represent the weekly and monthly trends. This is done in quite a simplistic but novel way but dampening noise in different ways using a moving average function. This makes long term trends and short term trends easier to see.

This function is then fitted to the data and can be used to predict further into the future. The fitted functions are shown here:

![FirstSinFit](https://github.com/JoekingCooper/btakehometask/blob/master/images/FirstSinFit.png)

The forecast sales are then shown here:

![FirstForecast](https://github.com/JoekingCooper/btakehometask/blob/master/images/FirstForecast.png)

Obviously the errors on this prediction seem a little small so I want to revisit those and double check the calculations.

### 8. Forecasting with a Bespoke Model, with corrected errors


Credits
-------

This package was created with Cookiecutter_ and the `audreyr/cookiecutter-pypackage`_ project template.

.. _Cookiecutter: https://github.com/audreyr/cookiecutter
.. _`audreyr/cookiecutter-pypackage`: https://github.com/audreyr/cookiecutter-pypackage
