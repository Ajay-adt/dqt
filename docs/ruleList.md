# Rule Type Mapping


| Rule ID | Expectation Name                             | Action Function Name                        |
|--------------|----------------------------------------------|---------------------------------------------|
| 1            | `expect_column_to_exist`                     | (No specific action function assigned)     |
| 2            | `expect_table_row_count_to_be_between`       | (No specific action function assigned)     |
| 3            | `expect_table_row_count_to_equal`            | (No specific action function assigned)     |
| 4            | `expect_column_values_to_be_unique`         | `remove_duplicates`                        |
| 5            | `expect_column_values_to_not_be_null`       | `handle_null_values`                       |
| 6            | `expect_column_values_to_be_null`           | `handle_null_values`                       |
| 7            | `expect_column_values_to_be_of_type`        | `handle_data_type`                         |
| 8            | `expect_column_values_to_be_in_type_list`   | `handle_data_type`                         |
| 9            | `expect_column_values_to_be_in_set`         | `handle_in_set`                            |
| 10           | `expect_column_values_to_not_be_in_set`     | `handle_not_in_set`                        |
| 11           | `expect_column_values_to_be_between`        | `handle_value_range`                      |
| 12           | `expect_column_values_to_be_increasing`     | `handle_value_trend`                      |
| 13           | `expect_column_values_to_be_decreasing`     | `handle_value_trend`                      |
| 14           | `expect_column_value_lengths_to_be_between` | `handle_value_length`                     |
| 15           | `expect_column_values_to_match_regex`       | `handle_regex_match`                      |
| 16           | `expect_column_values_to_not_match_regex`   | `handle_regex_mismatch`                   |
| 17           | `expect_column_values_to_match_regex_list`  | `handle_regex_list_match`                 |
| 18           | `expect_column_values_to_match_strftime_format` | `handle_strftime_format`               |
| 19           | `expect_column_values_to_be_dateutil_parseable` | `handle_dateutil_parseable`           |
| 20           | `expect_column_values_to_be_json_parseable` | `handle_json_parseable`                 |
| 21           | `expect_column_values_to_match_json_schema` | `handle_json_schema_match`              |
| 22           | `expect_column_mean_to_be_between`           | `handle_statistical_range`              |
| 23           | `expect_column_median_to_be_between`         | `handle_statistical_range`              |
| 24           | `expect_column_stdev_to_be_between`          | `handle_statistical_range`              |
| 25           | `expect_column_unique_value_count_to_be_between` | `handle_unique_value_count_range` |
| 26           | `expect_column_proportion_of_unique_values_to_be_between` | `handle_proportion_of_unique_values_range` |
| 27           | `expect_column_most_common_value_to_be`     | `handle_most_common_value`               |
| 28           | `expect_column_most_common_value_to_be_in_set` | `handle_most_common_value_in_set`     |
| 29           | `expect_column_kl_divergence_to_be_less_than` | `handle_kl_divergence`                 |
| 30           | `expect_column_bootstrapped_ks_test_p_value_to_be_greater_than` | `handle_ks_test_p_value`          |
| 31           | `expect_column_chisquare_test_p_value_to_be_greater_than` | `handle_chisquare_test_p_value` |
