# I. Introduction
 Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

# II. Dataset:
## 1. Data source:

Publicly available product carbon-footprint data from Nature.com

## 2. Data structure:
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.
### Table 1: product_emissions 
**1. id:** Identifier for each product emission record.

**2. company_id:** Identifier for the company associated with the product.

**3. country_id:** Identifier for the country where the product is being produced.

**4. industry_group_id:** Identifier for the industry group to which the product belongs.

**5. year:** The year in which the emissions data was recorded.

**6. product_name:** The name of the product associated with the emissions data.

**7. weight_kg:** The weight of the product in kilograms.
carbon_footprint_pcf: The carbon footprint of the product, measured in CO2 equivalent.

**8. upstream_percent_total_pcf:** The percentage of the total carbon footprint attributed to upstream activities.

**9. operations_percent_total_pcf:** The percentage of the total carbon footprint attributed to operations.

**10. downstream_percent_total_pcf:** The percentage of the total carbon footprint attributed to downstream activities.

### Table 2: industry_groups
**1. id:** Unique identifier for each industry group.

**2. industry_group:** The name of the industry group, categorizing businesses within similar sectors based on their products or services offered.
 
### Table 3: companies
**1. id:** Unique identifier for each company.

**2. company_name:** The name of the company, identifying the specific organization within the dataset.   

### Table 4: countries 
**1. id:** Unique identifier for each country.

**2. country_name:** The name of the country. 

# III. Analyse data:
## Question 1:Which products contribute the most to carbon emissions?

```
SELECT 
	product_name,
	ROUND(AVG(carbon_footprint_pcf),2) AS Average_Carbon_Footprint_pcf
FROM product_emissions
GROUP BY product_name
ORDER BY AVG(carbon_footprint_pcf) DESC
LIMIT 10;

```
| product_name                                                                                                                       | Average_Carbon_Footprint_pcf | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044.00                   | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187.00                   | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608.00                   | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625.00                   | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.00                    | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.00                    | 
| TCDE                                                                                                                               | 99075.00                     | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.00                     | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.00                     | 
| Mercedes-Benz SL (SL 350)                                                                                                          | 72000.00                     | 



## Question 2: What are the industry groups of these products?
```
SELECT 
	ig.industry_group, 
	pe.product_name, 
	ROUND(AVG(pe.carbon_footprint_pcf),2) AS Average_Carbon_Footprint_pcf
FROM product_emissions AS pe
	JOIN industry_groups AS ig ON pe.industry_group_id = ig.id
GROUP BY industry_group, product_name
ORDER BY AVG(carbon_footprint_pcf) DESC
LIMIT 20;

```
| industry_group                     | product_name                                                                                                                       | Average_Carbon_Footprint_pcf | 
| ---------------------------------: | ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------: | 
| Electrical Equipment and Machinery | Wind Turbine G128 5 Megawats                                                                                                       | 3718044.00                   | 
| Electrical Equipment and Machinery | Wind Turbine G132 5 Megawats                                                                                                       | 3276187.00                   | 
| Electrical Equipment and Machinery | Wind Turbine G114 2 Megawats                                                                                                       | 1532608.00                   | 
| Electrical Equipment and Machinery | Wind Turbine G90 2 Megawats                                                                                                        | 1251625.00                   | 
| Automobiles & Components           | Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.00                    | 
| Materials                          | Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.00                    | 
| Materials                          | TCDE                                                                                                                               | 99075.00                     | 
| Automobiles & Components           | Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.00                     | 
| Automobiles & Components           | Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.00                     | 
| Automobiles & Components           | Mercedes-Benz SL (SL 350)                                                                                                          | 72000.00                     | 
| Capital Goods                      | Electric Motor                                                                                                                     | 70323.50                     | 
| Automobiles & Components           | Mercedes-Benz SL-Class                                                                                                             | 69000.00                     | 
| Automobiles & Components           | Mercedes-Benz S-Class Hybrid (S 400 h)                                                                                             | 66000.00                     | 
| Automobiles & Components           | Mercedes-Benz CLS (350 BlueEFFICIENCY)                                                                                             | 60000.00                     | 
| Automobiles & Components           | Mercedes-Benz CLS-Class                                                                                                            | 57100.00                     | 
| Automobiles & Components           | Mercedes-Benz S-Class                                                                                                              | 54000.00                     | 
| Capital Goods                      | Commercial Air Conditioner                                                                                                         | 51066.00                     | 
| Automobiles & Components           | Mercedes-Benz S-Class Hybrid (S 300 BlueTEC HYBRID)                                                                                | 51000.00                     | 
| Automobiles & Components           | Mercedes-Benz C-Class                                                                                                              | 50500.00                     | 
| Automobiles & Components           | Mercedes-Benz E-Class (E 200)                                                                                                      | 50000.00   

## Question 3: What are the industries with the highest contribution to carbon emissions?
```
SELECT 
	ig.industry_group, 
	ROUND(SUM(pe.carbon_footprint_pcf),2) AS Total_Carbon_Footprint_pcf
FROM product_emissions AS pe
	 LEFT JOIN industry_groups AS ig ON pe.industry_group_id = ig.id
GROUP BY industry_group
ORDER BY AVG(carbon_footprint_pcf) DESC
LIMIT 10;
```

| industry_group                                   | Total_Carbon_Footprint_pcf | 
| -----------------------------------------------: | -------------------------: | 
| Electrical Equipment and Machinery               | 9801558.00                 | 
| Automobiles & Components                         | 2582264.00                 | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486.00                   | 
| Capital Goods                                    | 258712.00                  | 
| Materials                                        | 577595.00                  | 
| "Mining - Iron, Aluminum, Other Metals"          | 8181.00                    | 
| Energy                                           | 10774.00                   | 
| Chemicals                                        | 62369.00                   | 
| Media                                            | 23017.00                   | 
| Software & Services                              | 46544.00        

## Question 4: What are the companies with the highest contribution to carbon emissions?
```
SELECT 
	company_name, 
	ROUND(Sum(pe.carbon_footprint_pcf),2) AS Total_Carbon_Footprint_pcf
FROM product_emissions AS pe
	JOIN companies AS c ON pe.company_id = c.id
GROUP BY company_name
ORDER BY SUM(carbon_footprint_pcf) DESC
LIMIT 10;
```
| company_name                            | Total_Carbon_Footprint_pcf | 
| --------------------------------------: | -------------------------: | 
| "Gamesa Corporación Tecnológica, S.A."  | 9778464.00                 | 
| Daimler AG                              | 1594300.00                 | 
| Volkswagen AG                           | 655960.00                  | 
| "Mitsubishi Gas Chemical Company, Inc." | 212016.00                  | 
| "Hino Motors, Ltd."                     | 191687.00                  | 
| Arcelor Mittal                          | 167007.00                  | 
| Weg S/A                                 | 160655.00                  | 
| General Motors Company                  | 137007.00                  | 
| "Lexmark International, Inc."           | 132012.00                  | 
| "Daikin Industries, Ltd."               | 105600.00                  | 

## Question 5:What are the countries with the highest contribution to carbon emissions?
```
SELECT 
	country_name AS Country, 
	ROUND(SUM(pe.carbon_footprint_pcf),2) AS Total_Carbon_Footprint_pcf
FROM product_emissions AS pe
	JOIN countries AS ct ON pe.company_id = ct.id
GROUP BY country_name
ORDER BY SUM(carbon_footprint_pcf) DESC
LIMIT 10;
```
| Country      | Total_Carbon_Footprint_pcf | 
| -----------: | -------------------------: | 
| Germany      | 9778464.00                 | 
| Lithuania    | 212016.00                  | 
| Greece       | 191687.00                  | 
| Japan        | 132012.00                  | 
| Colombia     | 105600.00                  | 
| South Africa | 35505.00                   | 
| France       | 21364.00                   | 
| Italy        | 20000.00                   | 
| Ireland      | 11160.00                   | 
| India        | 9328.00                    | 

## Question 6: What is the trend of carbon footprints (PCFs) over the years?

```
SELECT 
	pe.year, 
	ROUND(AVG(pe.carbon_footprint_pcf),2) AS Average_Carbon_Footprint_pcf,
	ROUND(SUM(pe.carbon_footprint_pcf),2) AS Total_Carbon_Footprint_pcf
FROM product_emissions AS pe
GROUP BY year
ORDER BY year ASC
```


