---
title: "User Data API"
date: 2022-09-21T15:06:50-07:00
weight: 331
draft: false
pre: "<b> - </b>"
---

{{<line_break>}}

#### The API functions for user data including its arguments are as listed

- [convert_userdata_csv_to_json](#function-1-convert_userdata_csv_to_json)
  - [userdata_dir](#argument-1-userdata_dir)
  - [output_dir](#argument-2-output_dir)
- [load_userdata_to_standards_database](#function-2-load_userdata_to_standards_database)
- [generate_userdata_to_csv](#function-3-generate_userdata_to_csv)

Read the section below for a detailed breakdown of each function. 

{{< line_break >}}

#### FUNCTION 1: convert_userdata_csv_to_json

The function converts user data from CSV format to JSON format, and it is typically called before loading the user data before baseline generation.

```
standard = Standard.build("90.1-PRM-2019")
userdata_dir = "#{File.dirname(Dir.pwd)}/user_data"
output_dir = "#{File.dirname(Dir.pwd)}/output"
# Converts user data from .csv file to .json file
json_path = standard.convert_userdata_csv_to_json(userdata_dir, output_dir)
```
Here is a breakdown of steps involved in the “convert_userdata_csv_to_json” function.

- **Step 1: Load default templates**

   The process first loads all the default user data templates as `json`.
 
- **Step 2: Insert data from csv to json**

   In this step, it grabs all the user CSV files saved in the `userdata_dir` and inserts data from the `csv` files into the `json` data. 
  
- **Step 3: Save json files**

   Lastly, the function saves the `json` to the `output_dir` in the `user_data_json` folder.

The function has two arguments, `userdata_dir` and `output_dir`.
##### **ARGUMENT 1: userdata_dir**

This argument shall be a valid directory string that points to the folder where all user data `csv` is saved. An example can be:

```
"C:\\documents\\my_user_data"
```

##### **ARGUMENT 2: output_dir**

This argument shall be a valid directory string that points to the folder where all user data `json` is saved. An example can be:

```
"C:\\documents\\user_data_json"
```

{{< line_break >}}

#### FUNCTION 2: load_userdata_to_standards_database

The function loads the user data folder into the `90.1-PRM-2019` instance.

```
standard = Standard.build("90.1-PRM-2019")
userdata_dir = "#{File.dirname(Dir.pwd)}/user_data"
output_dir = "#{File.dirname(Dir.pwd)}/output"
# Converts user data from .csv file to .json file
json_path = standard.convert_userdata_csv_to_json(userdata_dir, output_dir)
standard.load_userdata_to_standards_database(json_path)
```
A typical workflow will call `convert_userdata_csv_to_json` first to convert all user data from `csv` to `json` and then call `load_userdata_to_standards_database` to load the json into the workflow.

##### **ARGUMENT 1: json_path**
`json_path` shall be a valid directory string that points to the folder where all user data `json` is saved.

{{<line_break>}}

#### FUNCTION 3: generate_userdata_to_csv

This function helps generate user data from a user model and save the csvs to the user_data_path.

```
demo_path = "#{File.dirname(Dir.pwd)}/source"
proposed_geo_model = "/#{demo_folder}/#{proposed_model}"

Script begins
translator = OpenStudio::OSVersion::VersionTranslator.new
model = translator.loadModel("#{demo_path}#{proposed_geo_model}").get
# Generate baseline
userdata_dir = "#{File.dirname(Dir.pwd)}/user_data"
standard = Standard.build ( "90.1-PRM-2019")
standard.generate_userdata_to_csv(model, userdata_dir)

```
A typical workflow will call `generate_userdata_to_csv` to generate user data csv files based on the OpenStudio model and saves them to the specified data path. It can generate either a single user data csv (specified by `user_data_file`) or all available user data types if `user_data_file` is nil. The specific user data types are defined as different classes, and the code iterates through them to generate and save the CSV files as needed.

##### **ARGUMENT 1: user_model**

`user_model` is the OpenStudio model that will be used to create the user data.

##### **ARGUMENT 2: user_data_path**

This argument shall be a valid directory string that points to the folder where all user data `csv` is saved.


##### **ARGUMENT 3: user_data_file**

This argument shall specify the name of the user data file to be generated. If not provided, it defaults to nil.






