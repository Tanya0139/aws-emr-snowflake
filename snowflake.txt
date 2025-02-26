DROP DATABASE IF EXISTS redfin_database_1;
CREATE DATABASE redfin_database_1;
CREATE SCHEMA redfin_schema;
// Create Table

CREATE OR REPLACE TABLE redfin_database_1.redfin_schema.redfin_table (
    city STRING,
    homes_sold INT,
    inventory INT,
    median_dom FLOAT,
    median_ppsf FLOAT,
    median_sale_price FLOAT,
    months_of_supply FLOAT,
    period_duration INT,
    period_end_month STRING,
    period_end_yr INT,
    property_type STRING,
    sold_above_list FLOAT,
    state STRING
);

SELECT * FROM redfin_database_1.redfin_schema.redfin_table LIMIT 1000;

SELECT COUNT(*) FROM redfin_database_1.redfin_schema.redfin_table
-- DESC TABLE redfin_database.redfin_schema.redfin_table;


// Create file format object
CREATE SCHEMA file_format_schema;
CREATE OR REPLACE file format redfin_database_1.file_format_schema.format_parquet
    type = 'PARQUET'

// Create staging schema
CREATE SCHEMA external_stage_schema;
// Create staging
--DROP STAGE redfin_database.external_stage_schema.redfin-data-project-yml-tanya;
CREATE OR REPLACE STAGE redfin_database_1.external_stage_schema.redfin_data_project_yml_tanya 
    url="s3://redfin-data-project-yml-tanya/redfin-transform-zone-yml/redfin_data.parquet/part-00000-e79e263d-39cc-48f7-b0d0-26f487de2ced-c000.snappy.parquet"
    credentials=(aws_key_id='xxx'
    aws_secret_key='xxx')
    FILE_FORMAT = redfin_database_1.file_format_schema.format_parquet;

list @redfin_database_1.external_stage_schema.redfin_data_project_yml_tanya;


select * from @redfin_database_1.external_stage_schema.redfin_data_project_yml_tanya;

select
$1:city as City,
$1:homes_sold as homes_sold,
$1:inventory as Inventory,
round($1:median_dom, 2) as median_dom,
round($1:median_ppsf, 2) as median_ppsf,
round($1:median_sale_price,2) as median_sale_price,
round($1:months_of_supply,2) as months_of_supply,
$1:period_duration as period_uration,
$1:period_end_month as period_end_month,
$1:period_end_yr as period_year_end,
$1:property_type as property_type,
round($1:sold_above_list, 2) as sold_abive_list,
$1:state as State
from @redfin_database_1.external_stage_schema.redfin_data_project_yml_tanya;

COPY INTO redfin_database_1.redfin_schema.redfin_table
FROM(select
$1:city as City,
$1:homes_sold as homes_sold,
$1:inventory as Inventory,
round($1:median_dom, 2) as median_dom,
round($1:median_ppsf, 2) as median_ppsf,
round($1:median_sale_price,2) as median_sale_price,
round($1:months_of_supply,2) as months_of_supply,
$1:period_duration as period_uration,
$1:period_end_month as period_end_month,
$1:period_end_yr as period_year_end,
$1:property_type as property_type,
round($1:sold_above_list, 2) as sold_abive_list,
$1:state as State
FROM @redfin_database_1.external_stage_schema.redfin_data_project_yml_tanya);

//create pipe

// Create schema for snowpipe
-- DROP SCHEMA redfin_database.snowpipe_schema;
CREATE OR REPLACE SCHEMA redfin_database_1.snowpipe_schema;


CREATE OR REPLACE PIPE redfin_database_1.snowpipe_schema.redfin_snowpipe
AUTO_INGEST = TRUE
AS 
COPY INTO redfin_database_1.redfin_schema.redfin_table
FROM(select
$1:city as City,
$1:homes_sold as homes_sold,
$1:inventory as Inventory,
round($1:median_dom, 2) as median_dom,
round($1:median_ppsf, 2) as median_ppsf,
round($1:median_sale_price,2) as median_sale_price,
round($1:months_of_supply,2) as months_of_supply,
$1:period_duration as period_uration,
$1:period_end_month as period_end_month,
$1:period_end_yr as period_year_end,
$1:property_type as property_type,
round($1:sold_above_list, 2) as sold_abive_list,
$1:state as State
FROM @redfin_database_1.external_stage_schema.redfin_data_project_yml_tanya);

DESC PIPE redfin_database_1.snowpipe_schema.redfin_snowpipe;
SHOW PIPES IN SCHEMA redfin_database_1.snowpipe_schema;


SELECT * 
FROM TABLE(
    INFORMATION_SCHEMA.COPY_HISTORY(
        TABLE_NAME => 'REDFIN_DATABASE_1.REDFIN_SCHEMA.REDFIN_TABLE',
        START_TIME => DATEADD(HOUR, -24, CURRENT_TIMESTAMP)
    )
);

