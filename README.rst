=============
BTakeHomeTask
=============

A take home task, completed for a job application.


* Free software: MIT license


Summary
--------
The goal of this project is to impute missing data, forecast two weeks of sales and to also forecast the sales of a brand new line, with no data on it.

This project was completed primarily in Jupyter Notebooks, with Cookiecutter used to populate the repository. If there were more time this work could be turned into a python package.

Initial Plan
------------
To complete this task the plan was to complete the following sub-tasks in roughly the following order:
*Impute the missing data for the Country, County, avg_prices
*Impute missing total products - this is the most important field
*Create an estimate for missing price band
*Investiagte which platforms have the best sales
*Forecast sales for the last two weeks
*Apply this forecast to the new line

Notebook Guide
--------------
This section will provide a guide to the notebooks in btakehometask/notebooks/
** 1 Initial Exploration **
This notebook just plots the data in different ways, mostly to get used to the data and to look at where the missing data is. Found that there was missing data in 4 fields: Country, County, avg_prices and total_products


Credits
-------

This package was created with Cookiecutter_ and the `audreyr/cookiecutter-pypackage`_ project template.

.. _Cookiecutter: https://github.com/audreyr/cookiecutter
.. _`audreyr/cookiecutter-pypackage`: https://github.com/audreyr/cookiecutter-pypackage
