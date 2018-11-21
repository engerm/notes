# BlueHydra / blue_hydra / Blue Hydra misc notes - 20181121

General notes...

## SQLite tables (as configured in blue_hydra.yml)

1. Load db file into sqlite3. Execute:

    ```
sqlite3 blue_hydra.db
    ```
    
1. Show all tables. Execute:

    ```
.tables
    ```
    
 * result

    ```
blue_hydra_devices         blue_hydra_sync_versions
blue_hydra_pulse_trackers
    ```
 
1. Show blue_hydra_devices schema. Execute:

    ```
.schema blue_hydra_devices
    ```
    
 * result

    ```
CREATE TABLE IF NOT EXISTS "blue_hydra_devices" ("id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, "uuid" VARCHAR(50), "name" VARCHAR(50), "status" VARCHAR(50), "address" VARCHAR(50), "uap_lap" VARCHAR(50), "vendor" TEXT, "appearance" VARCHAR(50), "company" VARCHAR(50), "company_type" VARCHAR(50), "lmp_version" VARCHAR(50), "manufacturer" VARCHAR(50), "firmware" VARCHAR(50), "classic_mode" BOOLEAN DEFAULT 'f', "classic_service_uuids" TEXT, "classic_channels" TEXT, "classic_major_class" VARCHAR(50), "classic_minor_class" VARCHAR(50), "classic_class" TEXT, "classic_rssi" TEXT, "classic_tx_power" TEXT, "classic_features" TEXT, "classic_features_bitmap" TEXT, "le_mode" BOOLEAN DEFAULT 'f', "le_service_uuids" TEXT, "le_address_type" VARCHAR(50), "le_random_address_type" VARCHAR(50), "le_company_data" VARCHAR(50), "le_company_uuid" VARCHAR(50), "le_proximity_uuid" VARCHAR(50), "le_major_num" VARCHAR(50), "le_minor_num" VARCHAR(50), "le_flags" TEXT, "le_rssi" TEXT, "le_tx_power" TEXT, "le_features" TEXT, "le_features_bitmap" TEXT, "ibeacon_range" VARCHAR(50), "created_at" TIMESTAMP, "updated_at" TIMESTAMP, "last_seen" INTEGER, "needs_sync" BOOLEAN, "filthy_attrs" TEXT);
    ```
 
 1. Show blue_hydra_sync_versions schema. Execute:

    ```
.schema blue_hydra_sync_versions
    ```
    
 * result

    ```
CREATE TABLE IF NOT EXISTS "blue_hydra_sync_versions" ("id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, "version" VARCHAR(50));
    ```
 
 1. Show blue_hydra_pulse_trackers schema. Execute:

    ```
.schema blue_hydra_pulse_trackers
    ```
    
 * result

    ```
CREATE TABLE IF NOT EXISTS "blue_hydra_pulse_trackers" ("id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, "synced_at" INTEGER, "hard_reset_at" INTEGER, "reset_at" INTEGER);
    ```
 
## select rows

Execute:

```
sqlite3 -header -csv blue_hydra.db "select status,name,address,vendor,company,company_type,manufacturer,le_company_data from blue_hydra_devices;"
```

* result

```
offline,,21:12:CD:XX:XX:XX,"Apple, Inc.","Apple, Inc. (76)",Unknown,,0100
offline,,13:5A:BA:XX:XX:XX,"N/A - Random Address","Apple, Inc. (76)",,,21120a002112
offline,,50:B4:0B:XX:XX:XX,"N/A - Random Address","Apple, Inc. (76)",Unknown,"Ericsson Technology Licensing (0)",21125e2112
offline,,5E:60:57:XX:XX:XX,"N/A - Random Address","Apple, Inc. (76)",Unknown,,2112142112
offline,,56:6D:2B:XX:XX:XX,"N/A - Random Address","Apple, Inc. (76)",Unknown,"Ericsson Technology Licensing (0)",2112652112
offline,,5A:4E:AF:XX:XX:XX,"N/A - Random Address","Apple, Inc. (76)",Unknown,,21123a2112
offline,,67:4D:79:XX:XX:XX,"N/A - Random Address","Apple, Inc. (76)",Unknown,"Ericsson Technology Licensing (0)",2112aa2112
offline,,57:42:9A:XX:XX:XX,"N/A - Random Address","Apple, Inc. (76)",Unknown,,2112a52112
offline,,78:47:C1:XX:XX:XX,"N/A - Random Address","Apple, Inc. (76)",Unknown,,2112ab2112
offline,,4D:03:37:XX:XX:XX,"N/A - Random Address","Apple, Inc. (76)",Unknown,"Broadcom Corporation (15)",0b1cbe9fad
online,,68:A7:05:XX:XX:XX,"N/A - Random Address","Apple, Inc. (76)",Unknown,,2112562112
```



## resources

* General sqlite3 query: [Multiple warnings related to datamapper String max length and preventing the recording of devices. #112
](https://github.com/pwnieexpress/blue_hydra/issues/112)