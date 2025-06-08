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
![Trend Of Average Carbon](/Images/Trend%20of%20Average%20Carbon.png)


![Trend Of Total Carbon](/Images/Trend%20of%20Total%20Carbon.png)


## 3.7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
```
SELECT 
	ind_gr.industry_group AS 'Industry group',
	prod_em.year AS Year,
	round(sum(prod_em.carbon_footprint_pcf),2) AS "Total PCF"
FROM product_emissions AS prod_em
LEFT JOIN
	industry_groups AS ind_gr ON prod_em.industry_group_id = ind_gr.id
GROUP BY
	prod_em.year,
	ind_gr.industry_group
ORDER BY
	industry_group ASC,
	year DESC
```

| Industry group                                                         | Year | Total PCF  | 
| ---------------------------------------------------------------------: | ---: | ---------: | 
| "Consumer Durables, Household and Personal Products"                   | 2015 | 931.00     | 
| "Food, Beverage & Tobacco"                                             | 2017 | 3162.00    | 
| "Food, Beverage & Tobacco"                                             | 2016 | 100289.00  | 
| "Food, Beverage & Tobacco"                                             | 2015 | 0.00       | 
| "Food, Beverage & Tobacco"                                             | 2014 | 2685.00    | 
| "Food, Beverage & Tobacco"                                             | 2013 | 4995.00    | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 2015 | 8909.00    | 
| "Mining - Iron, Aluminum, Other Metals"                                | 2015 | 8181.00    | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 2014 | 40215.00   | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 2013 | 32271.00   | 
| "Textiles, Apparel, Footwear and Luxury Goods"                         | 2015 | 387.00     | 
| Automobiles & Components                                               | 2016 | 1404833.00 | 
| Automobiles & Components                                               | 2015 | 817227.00  | 
| Automobiles & Components                                               | 2014 | 230015.00  | 
| Automobiles & Components                                               | 2013 | 130189.00  | 
| Capital Goods                                                          | 2017 | 94949.00   | 
| Capital Goods                                                          | 2016 | 6369.00    | 
| Capital Goods                                                          | 2015 | 3505.00    | 
| Capital Goods                                                          | 2014 | 93699.00   | 
| Capital Goods                                                          | 2013 | 60190.00   | 
| Chemicals                                                              | 2015 | 62369.00   | 
| Commercial & Professional Services                                     | 2017 | 741.00     | 
| Commercial & Professional Services                                     | 2016 | 2890.00    | 
| Commercial & Professional Services                                     | 2014 | 477.00     | 
| Commercial & Professional Services                                     | 2013 | 1157.00    | 
| Consumer Durables & Apparel                                            | 2016 | 1162.00    | 
| Consumer Durables & Apparel                                            | 2014 | 3280.00    | 
| Consumer Durables & Apparel                                            | 2013 | 2867.00    | 
| Containers & Packaging                                                 | 2015 | 2988.00    | 
| Electrical Equipment and Machinery                                     | 2015 | 9801558.00 | 
| Energy                                                                 | 2016 | 10024.00   | 
| Energy                                                                 | 2013 | 750.00     | 
| Food & Beverage Processing                                             | 2015 | 141.00     | 
| Food & Staples Retailing                                               | 2016 | 2.00       | 
| Food & Staples Retailing                                               | 2015 | 706.00     | 
| Food & Staples Retailing                                               | 2014 | 773.00     | 
| Gas Utilities                                                          | 2015 | 122.00     | 
| Household & Personal Products                                          | 2013 | 0.00       | 
| Materials                                                              | 2017 | 213137.00  | 
| Materials                                                              | 2016 | 88267.00   | 
| Materials                                                              | 2014 | 75678.00   | 
| Materials                                                              | 2013 | 200513.00  | 
| Media                                                                  | 2016 | 1808.00    | 
| Media                                                                  | 2015 | 1919.00    | 
| Media                                                                  | 2014 | 9645.00    | 
| Media                                                                  | 2013 | 9645.00    | 
| Retailing                                                              | 2015 | 11.00      | 
| Retailing                                                              | 2014 | 19.00      | 
| Semiconductors & Semiconductor Equipment                               | 2016 | 4.00       | 
| Semiconductors & Semiconductor Equipment                               | 2014 | 50.00      | 
| Semiconductors & Semiconductors Equipment                              | 2015 | 3.00       | 
| Software & Services                                                    | 2017 | 690.00     | 
| Software & Services                                                    | 2016 | 22846.00   | 
| Software & Services                                                    | 2015 | 22856.00   | 
| Software & Services                                                    | 2014 | 146.00     | 
| Software & Services                                                    | 2013 | 6.00       | 
| Technology Hardware & Equipment                                        | 2017 | 27592.00   | 
| Technology Hardware & Equipment                                        | 2016 | 1566.00    | 
| Technology Hardware & Equipment                                        | 2015 | 106157.00  | 
| Technology Hardware & Equipment                                        | 2014 | 167361.00  | 
| Technology Hardware & Equipment                                        | 2013 | 61100.00   | 
| Telecommunication Services                                             | 2015 | 183.00     | 
| Telecommunication Services                                             | 2014 | 183.00     | 
| Telecommunication Services                                             | 2013 | 52.00      | 
| Tires                                                                  | 2015 | 2022.00    | 
| Tobacco                                                                | 2015 | 1.00       | 
| Trading Companies & Distributors and Commercial Services & Supplies    | 2015 | 239.00     | 
| Utilities                                                              | 2016 | 122.00     | 
| Utilities                                                              | 2013 | 122.00     | 

