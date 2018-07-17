# SAPHANA_ML
SAP HANA ML Movielens Recommendation System
based on the tutorial at https://www.sap.com/developer/groups/cp-hana-aa-movielens.html

## Steps

### Import the Datasets in data folder using the hdb.data.hdbti

### check the total number of records
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

### check for missing values

```
select count(1)
from "MOVIELENS"."ML_movielens.hdb::data.LINKS" l
where not exists (select 1 from "MOVIELENS"."ML_movielens.hdb::data.MOVIES" m where l."MOVIEID" = m."MOVIEID")
UNION ALL
select count(1)
from "MOVIELENS"."ML_movielens.hdb::data.MOVIES" m
where not exists (select 1 from "MOVIELENS"."ML_movielens.hdb::data.LINKS" l where l."MOVIEID" = m."MOVIEID");

```

There are no missing values in the dataset

#### check the movie genres
There are 18 different genres in the MOVIES dataset

```
DO
BEGIN
  DECLARE genreArray NVARCHAR(255) ARRAY;
  DECLARE tmp NVARCHAR(255);
  DECLARE idx INTEGER;
  DECLARE sep NVARCHAR(1) := '|';
  DECLARE CURSOR cur FOR SELECT DISTINCT "GENRES" FROM "MOVIELENS"."ML_movielens.hdb::data.MOVIES";
  DECLARE genres NVARCHAR (255) := '';
  idx := 1;
  FOR cur_row AS cur() DO
    SELECT cur_row."GENRES" INTO genres FROM DUMMY;
    tmp := :genres;
    WHILE LOCATE(:tmp,:sep) > 0 DO
      genreArray[:idx] := SUBSTR_BEFORE(:tmp,:sep);
      tmp := SUBSTR_AFTER(:tmp,:sep);
      idx := :idx + 1;
    END WHILE;
    genreArray[:idx] := :tmp;
  END FOR;

  genreList = UNNEST(:genreArray) AS ("GENRE");
  SELECT "GENRE" FROM :genreList GROUP BY "GENRE";
END;


```
