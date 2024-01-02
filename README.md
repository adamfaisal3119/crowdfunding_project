# Crowdfunding_ETL

The first few lines of code are as follows: 

# Import dependencies
import pandas as pd
import numpy as np
pd.set_option('max_colwidth', 400)

The first line of code 'import pandas as pd' this line imports the Pandas library and assigns it the alias 'pd'. Similarly 'import numpy as np' line imports the NumPy library and assigns it the alias 'np'. NumPy is another important library for numerical operations and is often used in conjunction with Pandas for handling numerical data efficiently.

'pd.set_option('max_colwidth', 400)': This line sets a Pandas option to display a maximum column width of 400 characters when printing DataFrames. By default, Pandas truncates the display of long strings or columns in DataFrames to make the output more readable. However, this line ensures that columns with text content can be displayed more comprehensively up to a maximum of 400 characters.

In[8] has the following code: 

crowdfunding_info_df = pd.read_excel('Resources/crowdfunding.xlsx')
crowdfunding_info_df.head()

The code 'pd.read_excel('Resources/crowdfunding.xlsx')' uses the read_excel function from the Pandas library to read data from an Excel file named 'crowdfunding.xlsx'. The file is  located in the 'Resources' directory. The resulting data is stored in the DataFrame crowdfunding_info_df.

The next line with 'crowdfunding_info_df.head()' is used after reading the data. This line uses the head() method to display the first few rows of the DataFrame. The head() method is handy for quickly inspecting the structure and content of the DataFrame. By default, it displays the first 5 rows, but you can specify the number of rows to show by passing a number inside the parentheses (e.g., head(10) to show the first 10 rows).

In[9] has the following code: 

crowdfunding_info_df.info()

The code crowdfunding_info_df.info() is utilizing the info() method provided by Pandas to present a summary of the information contained in the DataFrame named crowdfunding_info_df.


In[10] contains this code: 

crowdfunding_info_df["category & sub-category"].unique() 

The code 'crowdfunding_info_df["category & sub-category"].unique()' is using Pandas to extract the unique values from the column named "category & sub-category" in the DataFrame crowdfunding_info_df.

What the code is doing can be explained by this breakdown: 

'crowdfunding_info_df["category & sub-category"]': This part of the code selects the specific column "category & sub-category" from the DataFrame crowdfunding_info_df. The result is a Pandas Series containing all the values in that column.

'.unique()': This method is then applied to the selected column, and it returns an array of unique values present in that column. It removes duplicate values and provides a distinct list of categories and sub-categories.

For example, if "category & sub-category" contains values like "Technology", "Fashion", "Health", etc., this line of code will return an array with these unique values.

for In[11]
crowdfunding_info_df[["category", "subcategory"]] = crowdfunding_info_df["category & sub-category"].str.split('/', n=1, expand=True)
crowdfunding_info_df.head(10) . For this code it is  splitting the "category & sub-category" column into two separate columns, "category" and "subcategory," based on the '/' delimiter.

The rest of the file will be split into 4 subcatogeries 

Section 1: Extracting Unique Categories and Subcategories


categories = crowdfunding_info_df["category"].unique()
subcategories = crowdfunding_info_df["subcategory"].unique()

print(categories)
print(subcategories)
print(f"There are {len(categories)} unique crowdfunding categories.")
print(f"There are {len(subcategories)} unique crowdfunding subcategories.")

This code does the following: 


1) Extracts unique values from the "category" and "subcategory" columns in the crowdfunding_info_df.
2) Prints the unique values and the count of unique values for both categories and subcategories.


Section 2: Creating Category and Subcategory DataFrames


category_ids = np.arange(1, 10)
subcategory_ids = np.arange(1, 25)

# ... (prints category_ids and subcategory_ids)

cat_ids = ["cat0" + str(cat_id) for cat_id in category_ids]
scat_ids = ["scat0" + str(scat_id) for scat_id in subcategory_ids]

# ... (prints cat_ids and scat_ids)

category_df = pd.DataFrame({
    "category_id": cat_ids,
    "category": categories
})

subcategory_df = pd.DataFrame({
    "subcategory_id": scat_ids,
    "subcategory": subcategories
})


In essence this code: 

1) Creates arrays of category and subcategory IDs.
2) Creates DataFrames (category_df and subcategory_df) with category/subcategory IDs and names.
3) Prints the created category and subcategory IDs.


Section 3: Cleaning and Transforming the Campaign DataFrame

# ... (previous code)
campaign_df['goal'] = campaign_df['goal'].astype(float)
campaign_df['pledged'] = campaign_df['pledged'].astype(float)

# ... (checks datatypes)

campaign_df['launch_date'] = pd.to_datetime(campaign_df['launch_date'], unit='s')
campaign_df['end_date'] = pd.to_datetime(campaign_df['end_date'], unit='s')

# ... (checks datatypes again)

# ... (merges DataFrames)

campaign_cleaned = campaign_merged_df.drop(columns=["staff_pick", "spotlight", "category & sub-category", "category", "subcategory"])
campaign_cleaned.to_csv("Resources/campaign.csv", index=False)
campaign_cleaned


1) Converts 'goal' and 'pledged' columns to float data type.
2) Converts 'launch_date' and 'end_date' columns to datetime data type.
3) Merges the campaign DataFrame with category_df and subcategory_df.
4) Drops unnecessary columns and exports the cleaned DataFrame to a CSV file.


Section 4: Processing and Cleaning Contact Information

# ... (previous code)
contacts_df['contact_id'] = contacts_df['contact_info'].str.extract(p)
contacts_df['contact_id'] = contacts_df['contact_id'].astype(int)
contacts_df['name'] = contacts_df['contact_info'].str.extract(p)
contacts_df['email'] = contacts_df['contact_info'].str.extract(p)

contacts_df_copy = contacts_df[['contact_id', 'name', 'email']]
contacts_df_copy[['first_name', 'last_name']] = contacts_df_copy['name'].str.split(n=1, expand=True)

contacts_df_copy = contacts_df_copy.drop(columns='name')
contacts_df_final = contacts_df_copy[['contact_id', 'first_name', 'last_name', 'email']]

# ... (prints data types)

contacts_df_final.to_csv("Resources/contacts.csv", encoding='utf8', index=False)


1) Extracts 'contact_id' from 'contact_info' using regular expressions.
2) Converts 'contact_id' to int data type.
3) Extracts 'name' and 'email' using regular expressions.
4) Creates 'first_name' and 'last_name' columns.
5) Drops unnecessary columns and exports the cleaned DataFrame to a CSV file.

In conclusion the provided code demonstrates a structured and thorough approach to data preprocessing. Key tasks include handling unique values in categories and subcategories, creating categorical DataFrames, cleaning and transforming the campaign dataset by adjusting data types and merging with category information, and cleaning contact information by extracting relevant details. The resulting datasets, "campaign.csv" and "contacts.csv," are well-organized and ready for further analysis or machine learning tasks. The use of Pandas, NumPy, and regular expressions showcases a robust data manipulation process. Remember to adapt the code according to your specific dataset and analysis goals.
