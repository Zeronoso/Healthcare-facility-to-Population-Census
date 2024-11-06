# Healthcare-facility-to-Population-Census


My main goal for this project was to create a dashboard comparing healthcare access across Canada
in comparison to the population density. I was able to find the open database of healthcare facilities for 2020. In tandem I was also able to find a population census for  2016 and 2021

with this data, my goal was to compare the population growth rate between 2016 and 2021 for each region and compare it to the number of healthcare facilities per capita. My hypothesis's with this data was that Regions with high population growth and high population density from 2016 to 2021  have lower healthcare facility density in 2020. This would explore the idea that population density, combined with growth, correlates with lower facility density and in such potential barriers of access to the growing population.


[Open Database of Healthcare Facilities](url)
[Population Census 2016-2021](url)

# Data Quality 
Both datasets used in this project -- 2020 Open Healthcare Facilities Database (ODHF) and the 
2016/2021 population census data: Canada, provinces and territories, and economic regions
are sourced from Statistics Canada, the national agency responsible for producing reliable, high-quality data on various aspects on Canadian economy and societal changes, and enforces high standards in data collection. As a result, these databases are widely recognized and trusted by researchers for providing a solid foundation for my analysis on healthcare accessibility in relation to population densities.

# Data cleaning 

## Open Database of Healthcare Facilities File

### Step 1: File encoding check

Upon loading the Open Database of Healthcare Facilities (ODHF) file, an encoding error occurred. 
Using 'chardet', I identified the correct encoding type and proceeded with the data cleaning process.

### Step 2: Handing Missing Data

Initial inspection of the database using Pandas listed several missing entries as follows:

![Screenshot 2024-11-06 054930](https://github.com/user-attachments/assets/522038c4-f8c3-4a64-9242-b2905a34f526)


Primarily in the city and coordinate fields (latitude and longitude). 

Given the projects focus on healthcare access in relation to geographic location, these fields were main priorities for correction.

- City data: Only 23 entries had missing city names. I isolated these files into a separate CSV file, manually filled each city's name based on the relative rows geographical coordinates, and then concatenated the corrected entries back into the main dataset. The combined result was saved as 
'combined_healthcare_data.csv' 

### Step 3: Geocoding Missing Coordinates

To deal with the missing latitude and longitude values. I implemented geocoding using the 'geopy' library in Python. Utilizing the city and province data to generate coordinates, this technique successfully filled the majority of the missing values. Leaving two entries still without coordinates, I manually retrieved these said coordinates via Google Maps and updated the dataset.


### Step 4: Removing Irrelevant Columns

For this analysis, columns such as CSDname, CSDuid, Pruid and source_format_str_address were deemed unnecessary. However, CSDname, CSDuid, and Pruid are unique identifiers from Statistics Canada that could be relevant to future work. Therefore, I preserved a backup file (odhf_cleaned_backup.csv) containing the cleaned database with these columns for potential future use.




## Population Census File

### Step 1: Retrieving the relative data

the original dataset was obtained from Statistics Canada, where I was able to selectively download columns that were relevant to the scope of my analysis, streamlining the cleaning process 

### Step 2: Handling/formatting the data

The first step was checking for miss data, where no missing data was found. Next, I formatted the column names, removing any commas that could interfere with data processing.

Next, upon inspecting the data types, I found that many of the columns were categorized as object types, which needed to be converted into numeric types for proper aggregation in future analyses. To handle this, I removed any commas from the numerical values and converted the data to appropriate numeric types.

I also renamed the columns for improved clarity and ease of use in the analysis process.

### Step 3: normalizing data

To ensure consistency with the "odhf_cleaned.csv" dataset, I normalized the abbreviations for provinces and territories to match those in the healthcare facility dataset.

Lastly, I identified and removed rows that represented aggregated data for cities or towns spanning multiple provinces, as these were not needed for specific provincial data analysis and they had separate entries handling their populations in each province they are apart of.


### Step 4: accuracy check

In quick summary, the geographic data column is structured into five distinct categories

1. Country: represents the total population of the country.

2. PR: Represents the total population count for each province

3. CMA: Refers to a Census Metropolitan Area, defined as an area with a population of at least 100,000, with at least 50,000 living in its core.

4. CA: Refers to a Census Agglomeration, which is an area with a core population of at least 10,000

5. TERR: A unique abbreviation for the territories (Yukon, Northwest Territories, and Nunavut), used instead of the "PR" abbreviation for provinces.

To test the accuracy of the data, I conducted an aggregation by summing the population of all rows with 'bc' (British Columbia) in the 'province' column and subtracted the sum with the row containing both 'bc' in its province column and 'PR' in its 'geographic area type abbreviation'. 

The aggregation resulted in a population of 4,477,748 for total population 2021 in comparison of the British Columbia row's total population 2021 of 5,000,849.

I verified the results by performing the same aggregation on the original, uncleaned dataset, and the results were consistent. Based on these findings, I concluded that the discrepancy is likely due to missing population unaccounted for from rural areas in the 'CMA' or 'CA' categories. I will keep this in mind when analyzing the data in the future if applicable.
