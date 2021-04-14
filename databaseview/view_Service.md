| 列                            | 类型                                                  | 注释 |
| :---------------------------- | ----------------------------------------------------- | ---- |
| id                            | int [**0**]                                           |      |
| name                          | varchar(255) *NULL* []                                |      |
| org_id                        | int *NULL* [**0**]                                    |      |
| organization_name             | varchar(255) *NULL* []                                |      |
| servicefamily_id              | int *NULL* [**0**]                                    |      |
| servicefamily_name            | varchar(255) *NULL* []                                |      |
| description                   | text *NULL*                                           |      |
| status                        | enum('implementation','obsolete','production') *NULL* |      |
| icon                          | varchar(255) *NULL*                                   |      |
| friendlyname                  | varchar(255) *NULL*                                   |      |
| org_id_friendlyname           | varchar(255) *NULL*                                   |      |
| org_id_obsolescence_flag      | int [**0**]                                           |      |
| servicefamily_id_friendlyname | varchar(255) *NULL*                                   |      |
| Serviceicon_data              | longblob *NULL*                                       |      |
| Serviceicon_filename          | varchar(255) *NULL*                                   |      |

```
SELECT DISTINCT
	`Service`.`id` AS `id`,
	`Service`.`name` AS `name`,
	`Service`.`org_id` AS `org_id`,
	`Organization_org_id`.`name` AS `organization_name`,
	`Service`.`servicefamily_id` AS `servicefamily_id`,
	`ServiceFamily_servicefamily_id`.`name` AS `servicefamily_name`,
	`Service`.`description` AS `description`,
	`Service`.`status` AS `status`,
	`Service`.`icon_mimetype` AS `icon`,
	cast( concat( COALESCE ( `Service`.`name`, '' )) AS CHAR charset utf8mb4 ) AS `friendlyname`,
	cast( concat( COALESCE ( `Organization_org_id`.`name`, '' )) AS CHAR charset utf8mb4 ) AS `org_id_friendlyname`,
	COALESCE (( `Organization_org_id`.`status` = 'inactive' ), 0 ) AS `org_id_obsolescence_flag`,
	cast( concat( COALESCE ( `ServiceFamily_servicefamily_id`.`name`, '' )) AS CHAR charset utf8mb4 ) AS `servicefamily_id_friendlyname`,
	`Service`.`icon_data` AS `Serviceicon_data`,
	`Service`.`icon_filename` AS `Serviceicon_filename` 
FROM
	((
			`service` `Service`
			JOIN `organization` `Organization_org_id` ON ((
					`Service`.`org_id` = `Organization_org_id`.`id` 
				)))
		LEFT JOIN `servicefamily` `ServiceFamily_servicefamily_id` ON ((
				`Service`.`servicefamily_id` = `ServiceFamily_servicefamily_id`.`id` 
			))) 
WHERE
	(
	0 <> 1)
```

