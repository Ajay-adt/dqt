```python
     from dqt import DQEngine
     from pyspark.dbutils import DBUtils
     DQEngine.dbutils = DBUtils(spark)


```python
def start_data_validation():
    from pyspark.dbutils import DBUtils
    dbutils = DBUtils(spark)
    secrets_list                = DQEngine.fetch_secrets(dbutils)
    #################################################
    #  Initilize GX DataContext Object and Configure Backend Setup !!!
    #################################################
    context                     = DQEngine.InitilizeGX(dbutils)
    gx_config_params            = DQEngine.GXConfigParams(*secrets_list+[context])

    if gx_config_params.AZURE_STORAGE_CONNECTION and gx_config_params.CONTAINER:
        DQEngine.setup_metadata_stores(gx_config_params)
        DQEngine.setup_data_docs_site(gx_config_params)
        DQEngine.update_gx_config(gx_config_params)

    #################################################
    #   Get GX DataContext Object
    #################################################
    context                     = DQEngine.InitilizeGX(dbutils)

    #################################################
    #   Take input from ADF & load data

    current_time                = datetime.now().isoformat()
    input_params                = DQEngine.InputParams(spark,dbutils,GXDS_STORAGE_ACCOUNT_NAME,GXDS_CONTAINER_NAME,GXDS_CONTAINER_DIR,fileName,fileFormat,ruleList,lastModifiedTimestamp,current_time,pipeline_run_id)
    expectation_suite_name      = f"{input_params.fileName}_suite"
    file_location               = DQEngine.init_variable(input_params)
    file_list                   = get_file_list(input_params,file_location)
    data                        = get_file(input_params,file_list)

    #################################################
    #   Create Batch Request
    #################################################

    batch_request_params        = DQEngine.BatchRequestParams(context,input_params,data)
    batch_request               = DQEngine.create_batch_request(batch_request_params)

    #################################################
    #   Expectatins Suite Creation
    #################################################
    expectation_suite_params    = DQEngine.CustomExpectationsSuiteParams(dbutils,context,expectation_suite_name,input_params.ruleList)
    DQEngine.custom_expectations_suite(expectation_suite_params)

    #################################################
    #   Validate Suite
    #################################################
    validation_params           = DQEngine.ValidationParams(dbutils,context,batch_request,expectation_suite_name)
    checkpoint_result,validator = DQEngine.validate_checkpoint(validation_params)

    #################################################
    #   Validate Suite and logging output into DB
    #################################################
    notification_params         = MSTNotificationAction(dbutils,checkpoint_result,validator,gx_config_params)
    DQEngine.mst_notificatio_action(notification_params)
    action_params                = DQEngine.ActionParams(input_params,checkpoint_result)
    sourceDF,dq_log              = DQEngine.take_action(data,action_params)
    
    return sourceDF,dq_log

```python
if dqRule:
    sourceDF,dq_log = start_data_validation()
    sourceDF.createOrReplaceTempView("v_source")
    spark.sql("""use catalog {}""".format(catalog))
    spark.sql("""create database if not exists {}""".format('silver'))
    selected_catalog = spark.sql("""select current_catalog()""").collect()[0]["current_catalog()"""]
    if catalog == selected_catalog:
        silver_layer(key_column,sourceDF,catalog,tgt_container, tgt_file,writeLocation)
    else:
        dbutils.notebook.exit(f"Selected Catalog: {selected_catalog} \n Give Catalog: {catalog} \n Are Different Please Check Catalog Should Be Created")
else:
   # Action
   pass
```
