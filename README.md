# Project 2: Bangkok Housing

# Business Problem Statement: Affordable Housing Access
Investigate ways to improve access to affordable housing in Bangkok for lower-income residents while maintaining quality and sustainability.

Many people in Bangkok are looking for affordable housing options. Creating a platform (website/application) that helps users find affordable housing units based on their budget, family size, and preferred subdistrict can serve this need.

# Data Dictionary
| Column Name | Data Type | Description |
|---|---|---|
| id | int | ID of selling item |
| province | string | Province name (Bangkok, Samut Prakan, or Nonthaburi) |
| district | string | District name |
| subdistrict | string | Subdistrict name |
| address | string | Address (e.g., street name, area name, soi number) |
| property_type | string | Type of the house (Condo, Townhouse, or Detached House) |
| total_units | float | Number of rooms/houses in the condo/village |
| bedrooms | int | Number of bedrooms |
| baths | int | Number of baths |
| floor_area | float | Total inside floor area (in square meters) |
| floor_level | int | Floor level of the room |
| land_area | float | Total land area (in square meters) |
| latitude | float | Latitude of the house |
| longitude | float | Longitude of the house |
| nearby_stations | int | Number of nearby stations (within 1km) |
| nearby_station_distance | list | List of (station name, distance) tuples |
| nearby_bus_stops | int | Number of nearby bus stops |
| nearby_supermarkets | int | Number of nearby supermarkets |
| nearby_shops | int | Number of nearby shops |
| year_built | int | Year built |
| month_built | string | Month built (January-December) |
| price | float | Selling price (target value) |

# Exploratory Data Analysis
## Identify Missing data
![missing_data](../image/missing_data.png)

There are many missing data in this following:
1. total_units
2. subdistrict
3. bedrooms
4. baths
5. floor_level
6. land_area
7. nearby_station_distance
8. nearby_bus_stops
9. month_built

To impute a reasonable value is in this following methods:
1. Interpolate - to estimate intermediate values from a set of known data points. This method had been used in following missing data:
total_units, bedrooms, baths, floor_level, land_area

2. fillna `Unknown` to nearby_station_distance because may be some location of property are not near railway stations. Plus, I don't know what exactly those location near railway stations

3. Random in range (12-month) to month_built

4. Random Integer for floor_level which I set range 3-30 levels because I think most condominium are not higher than 32 levels

## Outliers
![outliers_province_by_price](../image/outliers_province_by_price.png)
![outliers_outliers_property_type_by_price](../image/outliers_property_type_by_price.png)

Calculate outliers by using IQR to know lower bound and upperbound. However, these data mostly upperbound. then, I remove outliers to make more accuracy on prediction model.

## Misleading Data
Importantly, I found there are wrong data on `subdistrict` column which are property name on this feature. Then I look district data which contains subdistrict data. After that, I replace correct subdistrict data from district data to fill the particular column.

## Distributions
![distribution_feature](https://github.com/pacharajson/project2_Bangkok_Housing/blob/main/image/distribution_feature.png)

## Correlations
![correlation](https://github.com/pacharajson/project2_Bangkok_Housing/blob/main/image/correlation.png)

# Pre-processing
## Possible Selected Features from correlation
1. subdistrict
2. property_type
3. bedrooms
4. baths
5. floor_area
6. floor_level
7. land_area
8. nearby_stations
9. nearby_supermarkets
10. nearby_shops
11. price (Key Feature)

## Modelling
![linreg_pred_model](https://github.com/pacharajson/project2_Bangkok_Housing/blob/main/image/linreg_pred_model.png)

![mse_linreg](https://github.com/pacharajson/project2_Bangkok_Housing/blob/main/image/mse_linreg.png)

I used linear regression from train/test split and extend ridge model to get more accurate

### Ridge Model
To compare with lasso and elasticnet, R2 ridge score is better than R2 lasso score about 10%. 
![r2_score_compare](https://github.com/pacharajson/project2_Bangkok_Housing/blob/main/image/r2_score_compare_model.png)

When I calculate by using root mean standard error(RMSE) score, it show below:
![mse_ridge_score](https://github.com/pacharajson/project2_Bangkok_Housing/blob/main/image/mse_ridge_score.png)

### Lasso Model
When I calculate by using root mean standard error(RMSE) score, it show below:
![mse_lasso_score](https://github.com/pacharajson/project2_Bangkok_Housing/blob/main/image/lasso_rmse_12577216.png)

# Conclusions and Recommendations
In conclusion, the application helps people to find affordable range around 2.5 million to 4 million Baht depend on in this following:
> 1. How large of land area (house and townhome), floor area(condominium)
> 2. The location (subdistrict) that influence the price such as Wattana and other high population is high price, but other areas are much more cheaper.
> 3. Floor level at Condominium can influence price because if higher level is much more expensive.

Recommendations
> 1. Comparing price between user's interested locations.
> 2. Do market research to apply with property type. For example, this data can be applied with customer relationship management data that can focus specific property type such as 20-30 years mostly buy condominium and 30-40 years who have family members prefer to buy house.
> 3. To apply with above, there is a financial range data or the application has feature that calculate income, expenses, and family size to choose property.
