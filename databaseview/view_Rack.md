| 列                            | 类型                                                         | 注释 |
| :---------------------------- | ------------------------------------------------------------ | ---- |
| id                            | int [**0**]                                                  |      |
| name                          | varchar(255) *NULL* []                                       |      |
| description                   | text *NULL*                                                  |      |
| org_id                        | int *NULL* [**0**]                                           |      |
| organization_name             | varchar(255) *NULL* []                                       |      |
| business_criticity            | enum('high','low','medium') *NULL* [**low**]                 |      |
| move2production               | date *NULL*                                                  |      |
| serialnumber                  | varchar(255) *NULL* []                                       |      |
| location_id                   | int *NULL* [**0**]                                           |      |
| location_name                 | varchar(255) *NULL* []                                       |      |
| status                        | enum('implementation','obsolete','production','stock') *NULL* [**production**] |      |
| brand_id                      | int *NULL* [**0**]                                           |      |
| brand_name                    | varchar(255) *NULL* []                                       |      |
| model_id                      | int *NULL* [**0**]                                           |      |
| model_name                    | varchar(255) *NULL* []                                       |      |
| asset_number                  | varchar(255) *NULL* []                                       |      |
| purchase_date                 | date *NULL*                                                  |      |
| end_of_warranty               | date *NULL*                                                  |      |
| nb_u                          | int *NULL*                                                   |      |
| finalclass                    | varchar(255) *NULL* [**FunctionalCI**]                       |      |
| friendlyname                  | varchar(255) *NULL*                                          |      |
| obsolescence_flag             | int [**0**]                                                  |      |
| obsolescence_date             | date *NULL*                                                  |      |
| org_id_friendlyname           | varchar(255) *NULL*                                          |      |
| org_id_obsolescence_flag      | int [**0**]                                                  |      |
| location_id_friendlyname      | varchar(255) *NULL*                                          |      |
| location_id_obsolescence_flag | int [**0**]                                                  |      |
| brand_id_friendlyname         | varchar(255) *NULL*                                          |      |
| model_id_friendlyname         | varchar(255) *NULL*                                          |      |

```
SELECT DISTINCT
	`Rack`.`id` AS `id`,
	`Rack_FunctionalCI`.`name` AS `name`,
	`Rack_FunctionalCI`.`description` AS `description`,
	`Rack_FunctionalCI`.`org_id` AS `org_id`,
	`Organization_org_id`.`name` AS `organization_name`,
	`Rack_FunctionalCI`.`business_criticity` AS `business_criticity`,
	`Rack_FunctionalCI`.`move2production` AS `move2production`,
	`Rack_PhysicalDevice`.`serialnumber` AS `serialnumber`,
	`Rack_PhysicalDevice`.`location_id` AS `location_id`,
	`Location_location_id`.`name` AS `location_name`,
	`Rack_PhysicalDevice`.`status` AS `status`,
	`Rack_PhysicalDevice`.`brand_id` AS `brand_id`,
	`Brand_brand_id_Typology`.`name` AS `brand_name`,
	`Rack_PhysicalDevice`.`model_id` AS `model_id`,
	`Model_model_id_Typology`.`name` AS `model_name`,
	`Rack_PhysicalDevice`.`asset_number` AS `asset_number`,
	`Rack_PhysicalDevice`.`purchase_date` AS `purchase_date`,
	`Rack_PhysicalDevice`.`end_of_warranty` AS `end_of_warranty`,
	`Rack`.`nb_u` AS `nb_u`,
	`Rack_FunctionalCI`.`finalclass` AS `finalclass`,
	cast( concat( COALESCE ( `Rack_FunctionalCI`.`name`, '' )) AS CHAR charset utf8mb4 ) AS `friendlyname`,
	COALESCE (( `Rack_PhysicalDevice`.`status` = 'obsolete' ), 0 ) AS `obsolescence_flag`,
	`Rack_FunctionalCI`.`obsolescence_date` AS `obsolescence_date`,
	cast( concat( COALESCE ( `Organization_org_id`.`name`, '' )) AS CHAR charset utf8mb4 ) AS `org_id_friendlyname`,
	COALESCE (( `Organization_org_id`.`status` = 'inactive' ), 0 ) AS `org_id_obsolescence_flag`,
	cast( concat( COALESCE ( `Location_location_id`.`name`, '' )) AS CHAR charset utf8mb4 ) AS `location_id_friendlyname`,
	COALESCE (( `Location_location_id`.`status` = 'inactive' ), 0 ) AS `location_id_obsolescence_flag`,
	cast( concat( COALESCE ( `Brand_brand_id_Typology`.`name`, '' )) AS CHAR charset utf8mb4 ) AS `brand_id_friendlyname`,
	cast( concat( COALESCE ( `Model_model_id_Typology`.`name`, '' )) AS CHAR charset utf8mb4 ) AS `model_id_friendlyname` 
FROM
	((
			`rack` `Rack`
			JOIN (((
						`physicaldevice` `Rack_PhysicalDevice`
						LEFT JOIN `location` `Location_location_id` ON ((
								`Rack_PhysicalDevice`.`location_id` = `Location_location_id`.`id` 
							)))
					LEFT JOIN `typology` `Brand_brand_id_Typology` ON ((
							`Rack_PhysicalDevice`.`brand_id` = `Brand_brand_id_Typology`.`id` 
						)))
				LEFT JOIN `typology` `Model_model_id_Typology` ON ((
						`Rack_PhysicalDevice`.`model_id` = `Model_model_id_Typology`.`id` 
						))) ON ((
					`Rack`.`id` = `Rack_PhysicalDevice`.`id` 
				)))
		JOIN (
			`functionalci` `Rack_FunctionalCI`
			JOIN `organization` `Organization_org_id` ON ((
					`Rack_FunctionalCI`.`org_id` = `Organization_org_id`.`id` 
					))) ON ((
				`Rack`.`id` = `Rack_FunctionalCI`.`id` 
			))) 
WHERE
	((
			0 <> COALESCE (( `Brand_brand_id_Typology`.`finalclass` = 'Brand' ), 1 )) 
	AND (
	0 <> COALESCE (( `Model_model_id_Typology`.`finalclass` = 'Model' ), 1 )))
```

