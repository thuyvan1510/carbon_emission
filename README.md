# carbon_emission

## Table 'product_emissions'
```sql
SELECT *
FROM product_emissions
LIMIT 5;
```
## Result
| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf | operations_percent_total_pcf | downstream_percent_total_pcf | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -------------------------: | ---------------------------: | ---------------------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                      | 17.36                        | 2.01                         | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                      | 5.51                         | 63.84                        | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                      | 4.51                         | 70.41                        | 

## Table 'industry_groups'
```sql
SELECT *
FROM industry_groups
LIMIT 5;
```
## Result:
| id | industry_group                                                         | 
| -: | ---------------------------------------------------------------------: | 
| 1  | "Consumer Durables, Household and Personal Products"                   | 
| 2  | "Food, Beverage & Tobacco"                                             | 
| 3  | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 
| 4  | "Mining - Iron, Aluminum, Other Metals"                                | 
| 5  | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 

## Find out duplicated records
```sql
SELECT *, COUNT(*) AS count_duplictate
FROM product_emissions
GROUP BY 
    id,
    company_id,
    country_id,
    industry_group_id,
    year,
    product_name,
    weight_kg,
    carbon_footprint_pcf,
    upstream_percent_total_pcf,
    operations_percent_total_pcf,
    downstream_percent_total_pcf
HAVING count(*)>1
LIMIT 10;
```
## Result
| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf                       | operations_percent_total_pcf                     | downstream_percent_total_pcf                     | count_duplictate | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -----------------------------------------------: | -----------------------------------------------: | -----------------------------------------------: | ---------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                                            | 30.00                                            | 12.50                                            | 2                | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                                            | 30.00                                            | 12.50                                            | 2                | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                                            | 17.36                                            | 2.01                                             | 2                | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                                            | 5.51                                             | 63.84                                            | 2                | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                                            | 4.51                                             | 70.41                                            | 2                | 
| 10261-3-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 2274                 | 20.05                                            | 3.61                                             | 76.34                                            | 2                | 
| 10324-1-2016 | 15         | 16         | 19                | 2016 | KURALON  fiber                                                  | 1500      | 10000                | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2                | 
| 10418-1-2013 | 84         | 9          | 19                | 2013 | Portland Cement                                                 | 1000      | 1102                 | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2                | 
| 10661-1-2014 | 85         | 28         | 11                | 2014 | 501® Original Jeans – Dark Stonewash                            | 0.997     | 16                   | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2                | 
| 10661-1-2015 | 85         | 28         | 6                 | 2015 | 501® Original Jeans – Dark Stonewash                            | 0.997     | 16                   | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2                | 

## Which products contribute the most to carbon emissions?
```sql
SELECT product_name,round(avg(carbon_footprint_pcf),2) as contribution
FROM product_emissions
GROUP BY product_name
ORDER BY contribution DESC;
```
## Result
| product_name                                                                                                                       | contribution | 
| ---------------------------------------------------------------------------------------------------------------------------------: | -----------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044.00   | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187.00   | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608.00   | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625.00   | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.00    | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.00    | 
| TCDE                                                                                                                               | 99075.00     | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.00     | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.00     | 
| Mercedes-Benz SL (SL 350)                                                                                                          | 72000.00     | 

## What are the industry groups of these products?
```sql
SELECT 
  pe.product_name,
  ig.industry_group,
  ROUND(AVG(pe.carbon_footprint_pcf), 2) AS contribution
FROM product_emissions pe
JOIN industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY pe.product_name, ig.industry_group
ORDER BY contribution DESC
LIMIT 10;
```
## Result
| product_name                                                                                                                       | industry_group                     | contribution | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | -----------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3718044.00   | 
| Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3276187.00   | 
| Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | 1532608.00   | 
| Wind Turbine G90 2 Megawats                                                                                                        | Electrical Equipment and Machinery | 1251625.00   | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | Automobiles & Components           | 191687.00    | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | 167000.00    | 
| TCDE                                                                                                                               | Materials                          | 99075.00     | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | Automobiles & Components           | 91000.00     | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | Automobiles & Components           | 85000.00     | 
| Mercedes-Benz SL (SL 350)                                                                                                          | Automobiles & Components           | 72000.00     | 

## What are the industries with the highest contribution to carbon emissions?
```sql
SELECT 
  ig.industry_group,
  ROUND(SUM(pe.carbon_footprint_pcf), 2) AS contribution
FROM product_emissions pe
JOIN industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY ig.industry_group
ORDER BY contribution DESC
LIMIT 10;
```
##Result
| industry_group                                   | contribution | 
| -----------------------------------------------: | -----------: | 
| Electrical Equipment and Machinery               | 9801558.00   | 
| Automobiles & Components                         | 2582264.00   | 
| Materials                                        | 577595.00    | 
| Technology Hardware & Equipment                  | 363776.00    | 
| Capital Goods                                    | 258712.00    | 
| "Food, Beverage & Tobacco"                       | 111131.00    | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486.00     | 
| Chemicals                                        | 62369.00     | 
| Software & Services                              | 46544.00     | 
| Media                                            | 23017.00     | 

## What are the companies with the highest contribution to carbon emissions?
```sql
SELECT 
    cp.company_name,
    ROUND(SUM(pe.carbon_footprint_pcf), 2) AS contribution
FROM product_emissions pe
JOIN companies cp ON cp.id = pe.company_id
GROUP BY pe.company_id
ORDER BY contribution DESC
LIMIT 10;
```
## Result
| company_name                            | contribution | 
| --------------------------------------: | -----------: | 
| "Gamesa Corporación Tecnológica, S.A."  | 9778464.00   | 
| Daimler AG                              | 1594300.00   | 
| Volkswagen AG                           | 655960.00    | 
| "Mitsubishi Gas Chemical Company, Inc." | 212016.00    | 
| "Hino Motors, Ltd."                     | 191687.00    | 
| Arcelor Mittal                          | 167007.00    | 
| Weg S/A                                 | 160655.00    | 
| General Motors Company                  | 137007.00    | 
| "Lexmark International, Inc."           | 132012.00    | 
| "Daikin Industries, Ltd."               | 105600.00    | 

## What are the countries with the highest contribution to carbon emissions?
```sq
SELECT 
  ct.country_name,
  ROUND(SUM(pe.carbon_footprint_pcf), 2) AS contribution
FROM product_emissions pe
JOIN countries ct ON ct.id = pe.country_id
GROUP BY pe.company_id
ORDER BY contribution DESC
LIMIT 10;
```
## Result
| country_name | contribution | 
| -----------: | -----------: | 
| Spain        | 9778464.00   | 
| Germany      | 1594300.00   | 
| Germany      | 655960.00    | 
| Japan        | 212016.00    | 
| Japan        | 191687.00    | 
| Luxembourg   | 167007.00    | 
| Brazil       | 160655.00    | 
| USA          | 137007.00    | 
| USA          | 132012.00    | 
| Japan        | 105600.00    | 

## What is the trend of carbon footprints (PCFs) over the years?
```sql
SELECT 
	year,
  	ROUND(SUM(carbon_footprint_pcf), 2) AS contribution
FROM product_emissions
GROUP BY year
ORDER BY year ASC;
```
## Result
| year | contribution | 
| ---: | -----------: | 
| 2013 | 503857.00    | 
| 2014 | 624226.00    | 
| 2015 | 10840415.00  | 
| 2016 | 1640182.00   | 
| 2017 | 340271.00    | 

## Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
 ```sql
SELECT
  industry_group,
  ROUND(SUM(CASE WHEN year = 2013 THEN ROUND(carbon_footprint_pcf, 2) ELSE 0 END),2) AS "2013",
  ROUND(SUM(CASE WHEN year = 2014 THEN ROUND(carbon_footprint_pcf, 2) ELSE 0 END),2) AS "2014",
  ROUND(SUM(CASE WHEN year = 2015 THEN ROUND(carbon_footprint_pcf, 2) ELSE 0 END),2) AS "2015",
  ROUND(SUM(CASE WHEN year = 2016 THEN ROUND(carbon_footprint_pcf, 2) ELSE 0 END),2) AS "2016",
  ROUND(SUM(CASE WHEN year = 2013 THEN ROUND(carbon_footprint_pcf, 2) ELSE 0 END),2) AS "2017"
FROM product_emissions pe
LEFT JOIN industry_groups ig ON ig.id = pe.industry_group_id
GROUP BY industry_group
ORDER BY 
	'2013',
	'2014',
	'2015',
	'2016',
	'2017';
```
## Result
| industry_group                                                         | 2013      | 2014      | 2015       | 2016       | 2017      | 
| ---------------------------------------------------------------------: | --------: | --------: | ---------: | ---------: | --------: | 
| "Food, Beverage & Tobacco"                                             | 4995.00   | 2685.00   | 0.00       | 100289.00  | 4995.00   | 
| Food & Beverage Processing                                             | 0.00      | 0.00      | 141.00     | 0.00       | 0.00      | 
| Capital Goods                                                          | 60190.00  | 93699.00  | 3505.00    | 6369.00    | 60190.00  | 
| Technology Hardware & Equipment                                        | 61100.00  | 167361.00 | 106157.00  | 1566.00    | 61100.00  | 
| Materials                                                              | 200513.00 | 75678.00  | 0.00       | 88267.00   | 200513.00 | 
| Consumer Durables & Apparel                                            | 2867.00   | 3280.00   | 0.00       | 1162.00    | 2867.00   | 
| "Textiles, Apparel, Footwear and Luxury Goods"                         | 0.00      | 0.00      | 387.00     | 0.00       | 0.00      | 
| Software & Services                                                    | 6.00      | 146.00    | 22856.00   | 22846.00   | 6.00      | 
| Chemicals                                                              | 0.00      | 0.00      | 62369.00   | 0.00       | 0.00      | 
| Semiconductors & Semiconductor Equipment                               | 0.00      | 50.00     | 0.00       | 4.00       | 0.00      | 
| Commercial & Professional Services                                     | 1157.00   | 477.00    | 0.00       | 2890.00    | 1157.00   | 
| Retailing                                                              | 0.00      | 19.00     | 11.00      | 0.00       | 0.00      | 
| Utilities                                                              | 122.00    | 0.00      | 0.00       | 122.00     | 122.00    | 
| Gas Utilities                                                          | 0.00      | 0.00      | 122.00     | 0.00       | 0.00      | 
| Telecommunication Services                                             | 52.00     | 183.00    | 183.00     | 0.00       | 52.00     | 
| Electrical Equipment and Machinery                                     | 0.00      | 0.00      | 9801558.00 | 0.00       | 0.00      | 
| Containers & Packaging                                                 | 0.00      | 0.00      | 2988.00    | 0.00       | 0.00      | 
| "Mining - Iron, Aluminum, Other Metals"                                | 0.00      | 0.00      | 8181.00    | 0.00       | 0.00      | 
| Media                                                                  | 9645.00   | 9645.00   | 1919.00    | 1808.00    | 9645.00   | 
| Automobiles & Components                                               | 130189.00 | 230015.00 | 817227.00  | 1404833.00 | 130189.00 | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 32271.00  | 40215.00  | 0.00       | 0.00       | 32271.00  | 
| Tires                                                                  | 0.00      | 0.00      | 2022.00    | 0.00       | 0.00      | 
| Trading Companies & Distributors and Commercial Services & Supplies    | 0.00      | 0.00      | 239.00     | 0.00       | 0.00      | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 0.00      | 0.00      | 8909.00    | 0.00       | 0.00      | 
| "Consumer Durables, Household and Personal Products"                   | 0.00      | 0.00      | 931.00     | 0.00       | 0.00      | 
| Energy                                                                 | 750.00    | 0.00      | 0.00       | 10024.00   | 750.00    | 
| Food & Staples Retailing                                               | 0.00      | 773.00    | 706.00     | 2.00       | 0.00      | 
| Household & Personal Products                                          | 0.00      | 0.00      | 0.00       | 0.00       | 0.00      | 
| Tobacco                                                                | 0.00      | 0.00      | 1.00       | 0.00       | 0.00      | 
| Semiconductors & Semiconductors Equipment                              | 0.00      | 0.00      | 3.00       | 0.00       | 0.00      | 

