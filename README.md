
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

### 1 Initial Exploration

This notebook just plots the data in different ways, mostly to get used to the data and to look at where the missing data is. Found that there was missing data in 4 fields: Country, County, avg_prices and total_products.

At this point I also noticed that the task was slightly wrong. The task had asked for us to investigate a new line between £20-30 in price, as the client didn't have a product in this price band. However upon investigation of the data I noticed that a product must exist in this price band and that the investigation was supposed to be done for a product in the £30-40 price band. The task provider confirmed this was true.


![Missing band £30-40](https://github.com/JoekingCooper/btakehometask/blob/master/images/productBrand.png)


This investigation also confirmed that 'total_products' was missing for the Mobile data. This is a key piece of information needed for forecasting sales in the last two weeks of August.

### Imputing NaNs

To impute the NaNs for Country and County I thought about doing something complicated like using a dictionary to match the city and region with the correct country and county. This could be done for a real dataset, but considering the task was to forecast sales, independant of region/city/county/country it seemed silly to waste a lot of time on this. So instead I just imputed missing_country and missing_county to these columns if there was no value.

avg_price and total_products are more important to the overall calculation, so I took more care with these, especially total_products. avg_price is used to estimate the sales of the new £30-40 line and, when doing this, the NaNs are simply removed. However, for the forecasting, the overall average price was imputed. This field isn't used for forecasting anyway so really this is just a placeholder value. 

Imputing total_products was a little more involved. I realised that the order number must be less than the total products and we had the order number for all sales (including Mobile data). So I calculated the mean total_products per num_orders for the first month. Then I used a simple apply to make the missing total_products = num_orders * mean. This gave a good estimate of the total products sold, these are the two functions used for the calculation:

"""
df_e=df
def calc_orderno_totprod_relationship(row):
    return row['total_products']/row['num_orders']
tot_product_imputation=np.mean(df_tp_noNaNs.apply(lambda row:calc_orderno_totprod_relationship(row),axis=1))
"""

"""
def impute_tp(row):
    if pd.notnull(row['total_products'])==False:
        return int(row['num_orders']*tot_product_imputation+0.5)#+0.5 is to round up instead of down
     else:
        return row['total_products']
df_e['total_products']=df.apply(lambda row: impute_tp(row),axis=1)
"""

Credits
-------

This package was created with Cookiecutter_ and the `audreyr/cookiecutter-pypackage`_ project template.

.. _Cookiecutter: https://github.com/audreyr/cookiecutter
.. _`audreyr/cookiecutter-pypackage`: https://github.com/audreyr/cookiecutter-pypackage
