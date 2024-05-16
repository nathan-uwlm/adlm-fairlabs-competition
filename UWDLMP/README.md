Hosted version of the tool:
https://nathan-uwlm.github.io/adlm-fairlabs-competition/

Execution Instructions
----------------------

Build the docker image:

```{bash}
cd UWDLMP
docker build . -t fairlabs
cd ..
```

Generate the dashboard with a dataset of your choosing. You may select another dataset by substituting the path for fairlabs_data.csv. 
If the column structure differs, you can adapt it by definiting it in a substitute for the fairlab_input_dict.txt input file.

```{bash}
# Assuming you are in the project root
mkdir output
docker run --rm -it -v $(pwd)/fairlabs_data.csv:/input.csv -v $(pwd)/output:/output -v $(pwd)/UWDLMP/fairlab_input_dict.txt:/dict.txt fairlabs bash
```

How to use the input dict file
------------------------------

-   You can open the file in Excel to help with filling out the columns

-   There are two columns in this file:

    -   Column_out: This column contains the variables the dashboard uses to build the visuals and the statistic tables. **Please do not make any changes to this column.**

    -   Column_in: In this column you will enter the corresponding column name in your data set that matches the definitions listed below for column_out.

        -   For example, "delivery_date" in column_out is for the child's birth date. If the corresponding date is named "child_birth_date" in your data set, you will enter that in the corresponding cell in column_in.

        -   If your data set has a matching column name as it appears in column_out, then copy it into column_in. For example, "uds_collection_date" appears in both column_in and column_out, as that is the name for column in your data set for the collection date of the UDS.

        -   If your data set has missing data for a column_out entry, please leave column_in blank. For example, if you have no CPS reporting dates, you would not fill in the corresponding cell in column_in.

    -   So the end result will look something like this

        | column_out          | column_in           |
        |---------------------|---------------------|
        | delivery_date       | child_birth_date    |
        | uds_collection_date | uds_collection_date |
        | cps_reporting_date  |                     |

-   For maximum utility of the dashboard, we recommend filling in as many of the column_in cells as possible. There are some paired cells that needs only one to be filled in (for example, uds_collection_date and uds_test).

    -   Some cells can be empty, for example cps_reporting_date, without affecting the functionality of the dashboard, but it will result in certain visuals and tables not displaying. You will get an error message indicating which cell was empty.

-   Save the result as a tab delimited .txt file.

# column_out value definitions

-   delivery_date - the column name for the date of birth of the child

-   maternal_birth_date - the column name for the mother's birth date, can be empty as long as maternal_age have a corresponding entry in column_in.

-   maternal_age - the column name if you store the maternal_age at the time of the child's birth, can be empty as long as maternal_birth_date have a corresponding entry in column_in.

-   cps_reporting_date - the column name for the CPS report date. This cell can be empty.

-   uds_collection_date - the column name for the sample collection date of the UDS. This cell can be empty if uds_test have a corresponding entry in column_in.

-   uds_test - the column name if you store whether the mother had a UDS ordered as TRUE/FALSE. This cell can be empty if uds_collection_date have a corresponding entry in column_in.

-   intervention_date - the date of the QI intervention. Please make sure that the year is 4 digits and month and day are 2 digits, for example '2024-01-01' or '01/01/2024'. This cell can be empty.

-   non_thc_detect - the column name if you store non-THC drug detection results as TRUE/FALSE. This cell can be empty as long as non_thc_cols have a corresponding entry in column_in.

-   non_thc_cols - a string containing a list of columns indicating non-THC drug detection for each drug. This cell can be empty as long as non_thc_detect have a corresponding entry in column_in. Your data set should look like this example:

    | pat_id | hydrocodone_detected | lsd_detected |
    |--------|----------------------|--------------|
    | pat_1  | 0                    | 0            |
    | pat_2  | 0                    | 1            |

    0 in the column stands for not detected. 1 stands for detected.

    The corresponding column_in cell should contain the column names of your data set you would like to include as non-THC drug detection results, with the column names concatenated together with '\\t' as the separator. So for the above example, the dict.txt entry will look like this:

    | column_out   | column_in                           |
    |--------------|-------------------------------------|
    | non_thc_cols | hydrocodone_detected\tlsd_detected |

    The dashboard will parse the column_in value and find the corresponding columns in your dataset. If you have only one column you do not need the '\\t' at the end. For example, if you only want hydrocodone to be used, your entry would look like this:

    | column_out   | column_in            |
    |--------------|----------------------|
    | non_thc_cols | hydrocodone_detected |

-   thc_detect - the column name if you store THC drug detection results as TRUE/FALSE. This cell can be empty as long as thc_col have a corresponding entry in column_in.

-   thc_col - the column name indicating THC drug detection. The data in this column should be the same format as non_thc_cols columns. This cell can be empty as long as thc_detect have a corresponding entry in column_in.

-   maternal_race - the column name for the mother's race

-   order_indication - the column name for the provider's order indication. This cell can be empty.

-   ord_indict_non_thc - a string containing the list of order indications you want to be considered as indication for non-THC drug use. Like non_thc_cols, you would concatenate the indications together with '\\t' as the separator. This cell can be empty.

    For example, your entry in dict.txt would look like this if you wanted both "Substance use during pregnancy, excluding marijuana" and "History of opioids prescribed during pregnancy" as order indications for non-THC drug use:

    | column_out         | column_in                                                                                            |
    |--------------------|------------------------------------------------------------------------------------------------------|
    | ord_indict_non_thc | Substance use during pregnancy, excluding marijuana\tHistory of opioids prescribed during pregnancy |
