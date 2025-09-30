# Carbon-Emission-Analysis
Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. This report aims to analyze carbon emissions to examine the carbon footprint across various industries.

<img src ="https://geographical.co.uk/wp-content/uploads/carbon-dioxide-emissions-title-1200x800.jpg" />
Source img: https://geographical.co.uk/climate-change/geo-explainer-how-carbon-emissions-affect-climate

## DATA
Dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

#### Data Structure

![alt](img/Databasediagram.png)

#### Data Sample
###### product_emissions
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|
|10261-3-2017|14|16|25|2017|Multifunction Printers|110.0|2274|20.05|3.61|76.34|
|10324-1-2016|15|16|19|2016|KURALON  fiber|1500.0|10000|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10418-1-2013|84|9|19|2013|Portland Cement|1000.0|1102|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10661-10-2014|85|28|11|2014|Regular Straight 505® Jeans – Steel (Water<Less™)|0.7665|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10661-10-2015|85|28|6|2015|Regular Straight 505® Jeans – Steel (Water<Less™)|0.7665|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|

###### companies
|id|company_name|
|--|------------|
|1|"Autodesk, Inc."|
|2|"Casio Computer Co., Ltd."|
|3|"Cisco Systems, Inc."|
|4|"CNX Coal Resources, LP"|
|5|"Coca-Cola Enterprises, Inc."|
|6|"Compañía Española de Petróleos, S.A.U. CEPSA"|
|7|"Daikin Industries, Ltd."|
|8|"Elitegroup computer systems co., Ltd."|
|9|"Fuji Xerox Co., Ltd."|
|10|"Gamesa Corporación Tecnológica, S.A."|


###### countries
|id|country_name|
|--|------------|
|1|Australia|
|2|Belgium|
|3|Brazil|
|4|Canada|
|5|Chile|
|6|China|
|7|Colombia|
|8|Finland|
|9|France|
|10|Germany|

###### industry_groups
|id|country_name|
|--|------------|
|1|Australia|
|2|Belgium|
|3|Brazil|
|4|Canada|
|5|Chile|
|6|China|
|7|Colombia|
|8|Finland|
|9|France|
|10|Germany|

## Research

#### 1.Which products contribute the most to carbon emissions?
Top 10 products contribute most carbon emissions

```
SELECT
    pe.product_name,
    AVG(pe.carbon_footprint_pcf) AS average_carbon,
    ROUND((AVG(pe.carbon_footprint_pcf)/pe.weight_kg) , 2) AS average_carbon_by_weight
FROM
    product_emissions pe
GROUP BY 
	pe.product_name 
ORDER BY
    average_carbon DESC
LIMIT 10;
```

|product_name|average_carbon|average_carbon_by_weight|
|------------|--------------|------------------------|
|Wind Turbine G128 5 Megawats|3718044.0000|6.2|
|Wind Turbine G132 5 Megawats|3276187.0000|5.46|
|Wind Turbine G114 2 Megawats|1532608.0000|3.83|
|Wind Turbine G90 2 Megawats|1251625.0000|3.47|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.0000|84.36|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000.0000|1.19|
|TCDE|99075.0000|8.26|
|Mercedes-Benz GLE (GLE 500 4MATIC)|91000.0000|26.0|
|Mercedes-Benz S-Class (S 500)|85000.0000|41.46|
|Mercedes-Benz SL (SL 350)|72000.0000|34.87|




#### What are the industry groups of these products?
Top 10 products contribute most carbon emissions with these industry groups

```
SELECT
    pe.product_name,
    AVG(pe.carbon_footprint_pcf) AS average_carbon,
    ROUND((AVG(pe.carbon_footprint_pcf)/pe.weight_kg) , 2) AS average_carbon_by_weight,
    ig.industry_group 
FROM
    product_emissions pe
    JOIN industry_groups ig ON pe.industry_group_id = ig.id 
GROUP BY 
	pe.product_name 
ORDER BY
    average_carbon DESC
LIMIT 10;
```

|product_name|average_carbon|average_carbon_by_weight|industry_group|
|------------|--------------|------------------------|--------------|
|Wind Turbine G128 5 Megawats|3718044.0000|6.2|Electrical Equipment and Machinery|
|Wind Turbine G132 5 Megawats|3276187.0000|5.46|Electrical Equipment and Machinery|
|Wind Turbine G114 2 Megawats|1532608.0000|3.83|Electrical Equipment and Machinery|
|Wind Turbine G90 2 Megawats|1251625.0000|3.47|Electrical Equipment and Machinery|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.0000|84.36|Automobiles & Components|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000.0000|1.19|Materials|
|TCDE|99075.0000|8.26|Materials|
|Mercedes-Benz GLE (GLE 500 4MATIC)|91000.0000|26.0|Automobiles & Components|
|Mercedes-Benz S-Class (S 500)|85000.0000|41.46|Automobiles & Components|
|Mercedes-Benz SL (SL 350)|72000.0000|34.87|Automobiles & Components|




#### What are the industries with the highest contribution to carbon emissions?
Get 10 industries with the highest contribution to carbon emissions 


```
SELECT 
	ig.industry_group,
	SUM(pe.carbon_footprint_pcf) AS total_carbon_footprint
FROM product_emissions pe 
JOIN industry_groups ig ON pe.industry_group_id = ig.id 
GROUP BY ig.industry_group
ORDER BY total_carbon_footprint DESC 
LIMIT 10;
```


|industry_group|total_carbon_footprint|
|--------------|----------------------|
|Electrical Equipment and Machinery|9801558|
|Automobiles & Components|2582264|
|Materials|577595|
|Technology Hardware & Equipment|363776|
|Capital Goods|258712|
|"Food, Beverage & Tobacco"|111131|
|"Pharmaceuticals, Biotechnology & Life Sciences"|72486|
|Chemicals|62369|
|Software & Services|46544|
|Media|23017|

#### What are the companies with the highest contribution to carbon emissions?
Get 10 companies with the highest contribution to carbon emissions

```
SELECT 
	c.company_name,
	SUM(pe.carbon_footprint_pcf) AS total_carbon_footprint
FROM product_emissions pe 
	JOIN companies c ON pe.company_id = c.id 
GROUP BY c.company_name 
ORDER BY total_carbon_footprint DESC 
LIMIT 10;
```

|company_name|total_carbon_footprint|
|------------|----------------------|
|"Gamesa Corporación Tecnológica, S.A."|9778464|
|Daimler AG|1594300|
|Volkswagen AG|655960|
|"Mitsubishi Gas Chemical Company, Inc."|212016|
|"Hino Motors, Ltd."|191687|
|Arcelor Mittal|167007|
|Weg S/A|160655|
|General Motors Company|137007|
|"Lexmark International, Inc."|132012|
|"Daikin Industries, Ltd."|105600|
