
## Great Expectations

#### Create and save expectations in an Expectation Suite
```python
# Import necessary libraries
import great_expectations as ge

# Load the Great Expectations context
context = ge.data_context.DataContext("../../gx")

# Load the JSON data into a Pandas DataFrame
data_file_path = "../../data/nested.json"
df = ge.read_json(data_file_path)

# Create a batch of data
batch = ge.dataset.PandasDataset(df)

# Define expectations for the 'id' column
batch.expect_column_values_to_be_between('id', min_value=1, max_value=100)
batch.expect_column_values_to_be_unique('id')

# Define expectations for the 'name' column
batch.expect_column_values_to_match_regex('name', r'^[A-Za-z\s]+$')
batch.expect_column_values_to_not_be_null('name')

# Create new columns for nested values
batch["details_age"] = batch["details"].apply(lambda x: x.get("age"))
batch["details_address_city"] = batch["details"].apply(lambda x: x.get("address").get("city"))
batch["details_address_state"] = batch["details"].apply(lambda x: x.get("address").get("state"))

# Define expectations for nested fields
batch.expect_column_values_to_be_between('details_age', min_value=0, max_value=120)
batch.expect_column_values_to_match_regex('details_address_city', r'^[A-Za-z\s]+$')
batch.expect_column_values_to_match_regex('details_address_state', r'^[A-Za-z\s]+$')

batch.save_expectation_suite('nestedjson_expectations_suite')
```

#### Vaildate batch against an expectation suite
```python
import great_expectations as ge
import openpyxl

# Load the Great Expectations context
context = ge.data_context.DataContext("../.")

# Load the JSON data into a Pandas DataFrame
data_file_path = "../../data/nested.json"
df = ge.read_json(data_file_path)

# Create a batch of data
batch = ge.dataset.PandasDataset(df)

# Create new columns for nested values
batch["details_age"] = batch["details"].apply(lambda x: x.get("age"))
batch["details_address_city"] = batch["details"].apply(lambda x: x.get("address").get("city"))
batch["details_address_state"] = batch["details"].apply(lambda x: x.get("address").get("state"))

result = batch.validate("nestedjson_expectations_suite.json")
print(result)
```

#### Validate batch and create data docs
```python
# Import necessary libraries
import great_expectations as ge
import datetime

# Load the Great Expectations context
context = ge.data_context.DataContext("../.")

# Load the JSON data into a Pandas DataFrame
data_file_path = "../../data/nested.json"
df = ge.read_json(data_file_path)

# Create a batch of data
batch = ge.dataset.PandasDataset(df)

# Create new columns for nested values
batch["details_age"] = batch["details"].apply(lambda x: x.get("age"))
batch["details_address_city"] = batch["details"].apply(lambda x: x.get("address").get("city"))
batch["details_address_state"] = batch["details"].apply(lambda x: x.get("address").get("state"))

# Define expectations for the 'id' column
batch.expect_column_values_to_be_between('id', min_value=1, max_value=100)
batch.expect_column_values_to_be_unique('id')

# Define expectations for the 'name' column
batch.expect_column_values_to_match_regex('name', r'^[A-Za-z\s]+$')
batch.expect_column_values_to_not_be_null('name')

# Define expectations for nested fields
batch.expect_column_values_to_be_between('details_age', min_value=0, max_value=120)
batch.expect_column_values_to_match_regex('details_address_city', r'^[A-Za-z\s]+$')
batch.expect_column_values_to_match_regex('details_address_state', r'^[A-Za-z\s]+$')

# # Load the expectation suite
# expectation_suite_name = 'nestedjson_expectations_suite'
# suite = context.get_expectation_suite(expectation_suite_name)

# Validate the batch against the expectation suite
results = context.run_validation_operator(
    "action_list_operator", 
    assets_to_validate=[batch],
    run_name = "abcd1",
    run_time = datetime.datetime.now(datetime.timezone.utc),
)

# Print the validation results
print(results.to_dict)

context.build_data_docs()
context.open_data_docs(resource_identifier=results.list_validation_result_identifiers()[0])
```
