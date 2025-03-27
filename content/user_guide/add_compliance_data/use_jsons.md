+++
title = "How to use Json Schema"
date = 2022-09-21T15:04:27-07:00
weight = 321
chapter = true
+++
## Provide compliance data using Json Schema

{{<line_break>}}

### What is JSON Schema?
JSON Schema is a powerful tool for validating the structure and content of JSON data. It defines the expected format of JSON documents, providing a clear, structured way to ensure that data adheres to a defined set of rules.

{{<line_break>}}
#### Why Use JSON Schema?
- **Validation**: JSON Schema helps validate that the JSON data meets a predefined format, ensuring data integrity and reducing errors.
- **Documentation**: By defining the structure of the data, JSON Schema acts as a form of documentation, making it easier for developers to understand what the data should look like.
- **Interoperability**: It ensures that different systems can understand and exchange JSON data reliably by conforming to the same schema.

{{<line_break>}}
#### How to Use JSON Schema?
JSON Schema can sounds great but its use is very different from the `.csv` files. These schemas are meant to serve a documentation of data files but not data files themselves. Taking the `userdata_building.json` as example.

```javascript
{
    "title": "ASHRAE 90.1 PRM Building Type User Data",
    "definitions": {
      "UserDataBuildingData": {
        "type": "object",
        "properties": {
          "userdata_building": {
            "type": "array",
            "description": "List of userdata_building rows",
            "items": {
              "$ref": "#/definitions/UserDataBuilding"
            }
          }
        }
      },
      "UserDataBuilding": {
        "type": "object",
        "properties": {
          "name": {
            "type": "string",
            "description": "Identifier or name for the building object"
          },
          "building_type_for_hvac": {
            "type": ["null","string"],
            "enum": [
              null,
              "heated-only storage",
              "hospital",
              "other nonresidential",
              "public assembly",
              "residential",
              "retail",
              "unconditioned"
            ],
            "default": null,
            "description": "Building type for HVAC",
            "$comment": "This field is used to determine the baseline model HVAC system. It can be overwritten at the zone level (see the documentation for userdata_thermal_zone.json). It should be set to one of the following values that best describes the proposed model building type."
          },
          "building_type_for_swh": {
            "type": ["null","string"],
            "enum": [
              null,
              "All others",
              "Automotive facility",
              "Convenience store",
              "Convention center",
              "Courthouse",
              "Dining: Bar lounge/leisure",
              "Dining: Cafeteria/fast food",
              "Dining: Family",
              "Dormitory",
              "Exercise center",
              "Fire station",
              "Grocery store",
              "Gymnasium",
              "Health-care clinic",
              "Hospital and outpatient surgery center",
              "Hotel",
              "Library",
              "Manufacturing facility",
              "Motel",
              "Motion picture theater",
              "Multifamily",
              "Museum",
              "Office",
              "Parking garage",
              "Penitentiary",
              "Performing arts theater",
              "Police station",
              "Post office",
              "Religious facility",
              "Retail",
              "School/university",
              "Sports arena",
              "Town hall",
              "Transportation",
              "Warehouse",
              "Workshop"
            ],
            "default": null,
            "description": "Building type for SWH",
            "$comment": "This field is used to determine the baseline model service water heating system. It should be set to one of the following values that best describes the proposed model building type."
          },
          "building_type_for_wwr": {
            "type": ["null", "string"],
            "enum": [
              null,
              "All others",
              "Grocery store",
              "Healthcare (outpatient)",
              "Hospital",
              "Hotel/motel <= 75 rooms",
              "Hotel/motel > 75 rooms",
              "Office 5,000 to 50,000 sq ft",
              "Office <= 5,000 sq ft",
              "Office > 50,000 sq ft",
              "Restaurant (full service)",
              "Restaurant (quick service)",
              "Retail (stand alone)",
              "Retail (strip mall)",
              "School (primary)",
              "School (secondary and university)",
              "Warehouse (nonrefrigerated)"
            ],
            "default": null,
            "description": "Building type for Window to wall ratio",
            "$comment": "This field is used to determine the baseline model’s window-to-wall ratio. It should be set to one of the following values that best described the proposed models building type."
          },
          "is_exempt_from_rotations": {
            "type": "boolean",
            "default": false,
            "description": "Is exempted from rotation requirements",
            "$comment": "Set this field to true if it can be demonstrated to the satisfaction of the rating authority that the building orientation is dictated by site considerations. If it doesnt apply, leave this field blank or set to false."
          }
        },
        "description": "User data building object",
        "required": ["name"],
        "additionalProperties": false
      }
    }
  }
```

The above schema defines the format of the userdata json file for buildings. Here's a `userdata_building.json` file that adheres to the provided JSON Schema: 
```javascript
{
  "userdata_building": [
    {
      "name": "Building A",
      "building_type_for_hvac": "hospital",
      "building_type_for_swh": "Hospital and outpatient surgery center",
      "building_type_for_wwr": "Hospital",
      "is_exempt_from_rotations": true
    },
    {
      "name": "Building B",
      "building_type_for_hvac": "public assembly",
      "building_type_for_swh": "Convention center",
      "building_type_for_wwr": "Office <= 5,000 sq ft",
      "is_exempt_from_rotations": false
    }
  ]
}
```

In this example:
- Each item in the userdata_building array corresponds to a building with required and optional properties.
- Properties like `building_type_for_hvac`, `building_type_for_swh`, and `building_type_for_wwr` use allowable values as specified in the schema.
- The `is_exempt_from_rotations` property is set to either true or false as specified in the schema.

{{<line_break>}}
#### How to use these jsons in PRM?

Similiarly, user who wish to use Json files instead of the `.csv` files shall name their json files exactly the same name as the JSON schema files. In the `user_data_path` directory, having these files created as the example below.

```
- user_data_json
    --> userdata_building.json
    --> userdata_space.json
    --> userdata_exterior_lighting.json
```
- **Use with OS Measure**

There is no change to the use of user data files when process it with PRM measure except the process will prioritize json files over csv files if found the `user_data_path` contains mix of these two types of files.

- **Use with OS SDK API**

In this approach, we can skip calling `convert_userdata_csv_to_json` function if provided data in jsons.
```ruby
# Create PRM object
standard = Standard.build('90.1-PRM-2019')
baseline_dir = "#{File.dirname(Dir.pwd)}/output"
json_path = "#{File.dirname(Dir.pwd)}/user_data_json", "#{baseline_dir}"
# Load the user data json files into the PRM routine.
standard.load_userdata_to_standards_database(json_path)
# Generate baseline
standard.model_create_prm_stable_baseline_building(model, climate_zone, hvac_bldg_type, wwr_bldg_type, swh_bldg_type, baseline_dir, unmet_load_hours_check=true, debug=false)
```



