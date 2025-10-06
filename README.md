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

#### Which products contribute the most to carbon emissions?
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

Looking at the average_carbon column, it is clear that Wind Turbine dominates the absolute carbon emissions list, taking the top 4 spots.

* The G128 5 Megawatts Wind Turbine has the highest emissions.
* While wind turbines are a clean energy technology when they are in operation, these high emissions almost certainly come from the Upstream stage. The production of high carbon intensity materials such as towers and components and blades. The sheer size and weight of these components too big.
* Otherwise, the product with the highest Carbon intensity: Land Cruiser Prado. FJ Cruiser. Dyna trucks... has the highest carbon intensity with 84.36 kg CO2 per kg of product weight, nearly 14 times that of the largest wind turbine (6.2).

Conclusion:

* Wind turbines are a large CO 2 problem due to their size.
* Trucks/SUVs are a low CO 2 problem due to their unclean manufacturing processes per unit weight.



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

* As mentioned, the huge size and weight of the products are the main reasons for the high total emissions of the Electrical Equipment and Machinery industry.
* Automobiles & Components products do not have as large an overall footprint as wind turbines but require more carbon per unit weight.



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

* The Electrical Equipment and Machinery sector had a total emissions of 9,801,558. This was nearly four times the number of the second-ranked sector (Automobiles & Components), and accounted for the majority of total emissions among the top 10 sectors.
* Automobiles & Components industry with total emissions of 2,582,264. As mentioned, their products have very high CO 2 /kg.
* The Materials sector has total emissions of 577,595. Its total emissions are significantly lower than the two leading sectors. Much of the Materials sector's emissions are already included in the emissions of the sectors that use it (e.g., emissions from steel production are included in Wind Turbine emissions).



#### What are the companies with the highest contribution to carbon emissions?
Get 10 companies with the highest contribution to carbon emissions

```
SELECT 
	c.company_name,
	SUM(pe.carbon_footprint_pcf) AS total_carbon_footprint,
	(
        SELECT
            pe_inner.product_name
        FROM
            product_emissions pe_inner
        WHERE
            pe_inner.company_id = c.id 
        ORDER BY
            pe_inner.carbon_footprint_pcf DESC
        LIMIT 1
    ) AS largest_emitting_product
FROM product_emissions pe 
	JOIN companies c ON pe.company_id = c.id 
GROUP BY c.company_name 
ORDER BY total_carbon_footprint DESC 
LIMIT 10;
```

|company_name|total_carbon_footprint|largest_emitting_product|
|------------|----------------------|------------------------|
|"Gamesa Corporación Tecnológica, S.A."|9778464|Wind Turbine G128 5 Megawats|
|Daimler AG|1594300|Mercedes-Benz GLE (GLE 500 4MATIC)|
|Volkswagen AG|655960|Audi A6|
|"Mitsubishi Gas Chemical Company, Inc."|212016|TCDE|
|"Hino Motors, Ltd."|191687|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|
|Arcelor Mittal|167007|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|
|Weg S/A|160655|Electric Motor|
|General Motors Company|137007|Average of all GM vehicles produced and used in the 10 year life-cycle.|
|"Lexmark International, Inc."|132012|MX812DFE - Mono Laser Printer|
|"Daikin Industries, Ltd."|105600|Commercial Air Conditioner|

The data confirms that the total emissions of most of the top companies are dominated by one or a few specific products/product lines.
* Gamesa Corporación Tecnológica, S.A. (9,778,464): This huge emissions are almost entirely due to the 5 Megawatts G128 Wind Turbine.
* Hino Motors, Ltd. (191,687): Their largest emitting product is a group of vehicles (Land Cruiser Prado, FJ Cruiser, Dyna trucks).



#### What are the countries with the highest contribution to carbon emissions?
Get 10 countries with the highest contribution to carbon emissions

```
SELECT 
	c.country_name,
	SUM(pe.carbon_footprint_pcf) AS total_carbon_footprint
FROM product_emissions pe 
	JOIN countries c ON pe.country_id = c.id 
GROUP BY c.country_name 
ORDER BY total_carbon_footprint DESC 
LIMIT 10;
```

|country_name|total_carbon_footprint|
|------------|----------------------|
|Spain|9786130|
|Germany|2251225|
|Japan|653237|
|USA|518381|
|South Korea|186965|
|Brazil|169337|
|Luxembourg|167007|
|Netherlands|70417|
|Taiwan|62875|
|India|24574|

* Spain's total emissions were 9,786,130. This figure is almost identical to the total emissions of the company that topped the previous list: "Gamesa Corporación Tecnológica, S.A." (9,778,464).
  ==> This shows that Gamesa is Spain's main emitter.
* Germany: Ranked second with 2,251,225. This figure is close to the total emissions of German automakers in the previous data (Daimler AG + Volkswagen AG ≈ 2,250,260).
* Japan: Ranked third with 653,237. This figure is in line with the total emissions of Japanese companies in the list (Hino Motors, Mitsubishi Gas Chemical, Daikin Industries, Ltd.).
  ==> Germany and Japan's emissions come mainly from the Automotive & Components and Chemical/Machinery sectors;




#### What is the trend of carbon footprints (PCFs) over the years?
To analyze the trend of carbon footprint (PCF) over the years, we need to calculate the total or average carbon footprint (carbon_footprint_pcf) for each year in the product_emissions table.

```
SELECT
    t1.year,
    t1.average_pcf,
    t1.total_pcf_sum,
    t1.number_of_products_recorded,
    (
        SELECT
            pe_inner.product_name
        FROM
            product_emissions pe_inner
        WHERE
            pe_inner.year = t1.year 
        ORDER BY
            pe_inner.carbon_footprint_pcf DESC
        LIMIT 1
    ) AS largest_emitting_product
FROM
    (
        SELECT
            year,
            ROUND(AVG(carbon_footprint_pcf), 2) AS average_pcf,
            SUM(carbon_footprint_pcf) AS total_pcf_sum,
            COUNT(id) AS number_of_products_recorded
        FROM
            product_emissions
        GROUP BY
            year
    ) AS t1
ORDER BY
    t1.year;
```

|year|average_pcf|total_pcf_sum|number_of_products_recorded|largest_emitting_product|
|----|-----------|-------------|---------------------------|------------------------|
|2013|2399.32|503857|210|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|
|2014|2457.58|624226|254|Electric Motor|
|2015|43188.90|10840415|251|Wind Turbine G128 5 Megawats|
|2016|6891.52|1640182|238|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|
|2017|4050.85|340271|84|TCDE|

2013 record low emissions, led by a massive steel structure (low PCF/kg).
2014 Emissions were stable compared to 2013, due to a lighter industrial machinery product.
2015 recorded a significantly higher average_pcf (43,188.90) and total_pcf_sum (10,840,415) than any other year.
* The largest emitter 2015 was the 5 Megawatts G128 Wind Turbine.
* This wind turbine was the largest absolute emitter of carbon in the entire dataset, with a PCF of 3,718,044.00. This pushed the total emissions for the year highest.
2016 record a significant growth. This spike is likely related to the recorded shipments of high carbon intensity (high PCF/kg) cars.
2017 record is down from 2016, but still higher than 2013/2014. The number of products recorded has dropped sharply (only 84 records), making it more difficult to assess trends.



#### Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
Find the earliest and latest year for each industry. Take the average PCF for each industry over all years. Calculate the reduction (start - end).

```
SELECT
    ig.industry_group,
    T.start_year,
    T.end_year,
    T.start_pcf_avg,
    T.end_pcf_avg,
    ROUND((T.start_pcf_avg - T.end_pcf_avg), 2) AS pcf_decrease
FROM
    industry_groups ig
INNER JOIN
    (
        SELECT
            t1.industry_group_id,
            t1.start_year,
            t1.end_year,
            (
                SELECT AVG(pe_start.carbon_footprint_pcf)
                FROM product_emissions pe_start
                WHERE pe_start.industry_group_id = t1.industry_group_id AND pe_start.year = t1.start_year
            ) AS start_pcf_avg,
            (
                SELECT AVG(pe_end.carbon_footprint_pcf)
                FROM product_emissions pe_end
                WHERE pe_end.industry_group_id = t1.industry_group_id AND pe_end.year = t1.end_year
            ) AS end_pcf_avg
        FROM
            (
                SELECT
                    industry_group_id,
                    MIN(year) AS start_year,
                    MAX(year) AS end_year
                FROM
                    product_emissions
                GROUP BY
                    industry_group_id
            ) AS t1
    ) AS T
ON
    ig.id = T.industry_group_id
ORDER BY
    pcf_decrease DESC;
```

|industry_group|start_year|end_year|start_pcf_avg|end_pcf_avg|pcf_decrease|
|--------------|----------|--------|-------------|-----------|------------|
|Media|2013|2016|2411.2500|602.6667|1808.58|
|Technology Hardware & Equipment|2013|2017|1053.4483|788.3429|265.11|
|Consumer Durables & Apparel|2013|2016|286.7000|40.0690|246.63|
|Food & Staples Retailing|2014|2016|77.3000|0.5000|76.80|
|Semiconductors & Semiconductor Equipment|2014|2016|16.6667|2.0000|14.67|
|Telecommunication Services|2013|2015|52.0000|45.7500|6.25|
|Retailing|2014|2015|6.3333|5.5000|0.83|
|Household & Personal Products|2013|2013|0.0000|0.0000|0.00|
|Containers & Packaging|2015|2015|373.5000|373.5000|0.00|
|"Textiles, Apparel, Footwear and Luxury Goods"|2015|2015|14.3333|14.3333|0.00|
|Electrical Equipment and Machinery|2015|2015|891050.7273|891050.7273|0.00|
|"Consumer Durables, Household and Personal Products"|2015|2015|116.3750|116.3750|0.00|
|Tires|2015|2015|1011.0000|1011.0000|0.00|
|Food & Beverage Processing|2015|2015|7.0500|7.0500|0.00|
|Tobacco|2015|2015|1.0000|1.0000|0.00|
|Chemicals|2015|2015|1949.0313|1949.0313|0.00|
|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|2015|2015|685.3077|685.3077|0.00|
|Trading Companies & Distributors and Commercial Services & Supplies|2015|2015|39.8333|39.8333|0.00|
|Semiconductors & Semiconductors Equipment|2015|2015|1.0000|1.0000|0.00|
|"Mining - Iron, Aluminum, Other Metals"|2015|2015|2727.0000|2727.0000|0.00|
|Gas Utilities|2015|2015|61.0000|61.0000|0.00|
|Utilities|2013|2016|61.0000|61.0000|0.00|
|"Food, Beverage & Tobacco"|2013|2017|94.2453|143.7273|-49.48|
|Commercial & Professional Services|2013|2017|144.6250|370.5000|-225.88|
|Software & Services|2013|2017|1.5000|690.0000|-688.50|
|Energy|2013|2016|750.0000|2506.0000|-1756.00|
|Materials|2013|2017|4177.3542|11217.7368|-7040.38|
|Capital Goods|2013|2017|5015.8333|18989.8000|-13973.97|
|Automobiles & Components|2013|2016|26037.8000|40138.0857|-14100.29|
|"Pharmaceuticals, Biotechnology & Life Sciences"|2013|2014|16135.5000|40215.0000|-24079.50|

* Industries with products less based on heavy industrial materials (such as Media, Hardware) have significantly improved their emission levels.
* A large number of industries have pcf_decrease=0.00. These industries only have data for a single year. It is not possible to assess the performance trends of these industries. Notably, the Electrical Equipment and Machinery industry (the largest absolute emitter) is included in this group.
* Sectors with the largest negative pcf_decrease values ​​have seen a sharp increase. The average emissions of the Automobiles & Components sector have increased dramatically (26,037.80 → 40,138.09). The Materials sector has seen a significant increase in average emissions, which is very worrying as it is a source of supply for other sectors.
