# SAPHANA_ML
SAP HANA ML Movielens Recommendation System


Import the Datasets in data folder using the hdb.data.hdbti

## check the total number of records

```
select 'links'   as "table name", count(1) as "row count" from "MOVIELENS"."ML_movielens.hdb::data.LINKS"
union all
select 'movies'  as "table name", count(1) as "row count" from "MOVIELENS"."ML_movielens.hdb::data.MOVIES"
union all
select 'ratings' as "table name", count(1) as "row count" from "MOVIELENS"."ML_movielens.hdb::data.RATINGS"
union all
select 'tags'    as "table name", count(1) as "row count" from "MOVIELENS"."ML_movielens.hdb::data.TAGS";

```


| Table Name | Row Count |
| -----------|:---------:|
| links   | 9125   |
| movies  | 9125   |
| ratings | 100004 |
| tags    | 1296   |
