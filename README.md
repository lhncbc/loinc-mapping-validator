### A LOINC Mapping Validator

This validator is designed to validate/check whether the LOINC mapping for the records
are correct. Currently it only considers the unit of a record but down the road this
might be expanded to consider the values and potentially other factors as well.

##### The LOINC Mapping Validator Library
At the core of this package is the LOINC mapping library. To use the library:

const validator = require('@lhncbc/loinc-mapping-validator');
The validator exposes 2 elements:
- validator.validate() is the function that you can call to validate a given 
  (unit, LOINC number) to see if they go together.
- validator.ValidationStatus defines the set of validation result status tokens. See
  the section "Output File" below for more details.

##### The Command-line Tool
There is a command line tool that comes with this package, which you can use to
batch validate records in a CSV input file, and output a new CSV file that contains
all the input columns plus a few new columns that indicate the validation status
for each record.

To use the command line too, you will need to work with Windows command terminals or
Linux terminals.
- Install Node js on your system
- Download this package

To valid the test file that comes with this package:

    cd into the directory where you saved this package
    node bin/validateLoincUnitCSV.js -i data/sample-test-file.csv -l LOINC -u UNIT

Here are more details on the command line syntax/options:

    node bin/validateLoincUnitCSV.js -i <input-csv-file> -l <loinc-column-name> -u <unit-column-name> [-o <output-file>]
Where:
- \<input-csv-file\>
    - must be a CSV file - there are different variants of CSV files, we can't guarantee that
      this tool works with every one of them. But we've tested it that it works with the CSV 
      file saved from Microsoft excel as "CSV (Comma Delimited)" format.
    - must have the column name row (header row)
    - must have a column for the unit
    - must have a column for the LOINC number
- \<loinc-column-name\>
    - The name of the column that has the LOINC number
- \<unit-column-name\>
    - The name of the column that has the unit
- \<output-file\>
   - This parameter is optional, if not specified, will print you standard output (terminal)
   - A CSV file with all the columns in the input file
   - 5 new columns for the validation results:
      - LMV_UNIT_STATUS: the status of the unit, possible values are:  
        VALID, INVALID, and UNKNOWN
      - LMV_LOINC_STATUS: the status of the LOINC mapping, possible values are:  
        VALID, INVALID, and UNKNOWN
      - LMV_SUBSTITUTED_UNIT: if the given unit is not a UCUM unit and there is a known mapping 
        to a UCUM unit, it will be suggested here.
      - LMV_UNIT_NOTE: a note explaining the LMV_UNIT_STATUS
      - LMV_LOINC_NOTE: a note explaining the LMV_LOINC_STATUS 
    
