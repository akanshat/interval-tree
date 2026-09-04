# Interval Tree Implementation

## Usage
- Golang is required to run this. You can install Go by following the instructions [here](https://golang.org/doc/install)
- Run the cli using `go run ./cmd/service`
- It expects inputs in the form of `EquipmentName StartTime EndTime`, for example `Equipment2 10:30 19:15`.
- Default data is given. If you want to use your own json data, you can provide the path to the file using `-filepath` flag. For example: `go run ./cmd/service -filepath ./custom-data.json`

## Running Tests
- Tests can be run using `go test ./...`

## Problem Description
Background:
In an industrial system there are various types of devices such as sensors, equipments, controllers etc.
    - The hierarchy (that we are concerned about for this problem) is as follows -
        - 1 Plant can contain multiple Equipments.
        - 1 Equipment can contain multiple Sensors.
        - A Sensor is at the lowest level in this hierarchy.
    - An Equipment uses sensor in a bounded time-range. i.e Equipment E1 uses Sensor1 from time1 to time2 and Sensor2 from time2 to time3 and so on.

```
Example :
- Total Equipments - E1, E2, E3, E4
- Total Sensors - S1, S2, S3, S4, S5, S6
- Sensor Mapping -
    - E1 -> S1 (10:00 - 15:00), S2 (11:00 - 14:30) , S3 (03:45 - 21:00)
    - E2 -> S2 (11:00 - 12:00), S4 (07:00 - 14:30), S5 (13:45 - 20:45), S6 (19:45 - 2
0:45)
    - E3 -> S1 (01:00 - 18:00), S3 (10:10 - 17:30) , S4 (10:05 - 21:30)
    - E4 -> S2 (14:30 - 19:00), S4 (03:05 - 14:30), S5 (16:15 - 19:00)
- Plant A
    - Equipments: E1, E4
- Plant B
    - Equipments: E2, E3
```

Problem:
Design a DataStructure, Class or Service that can store the data
and efficiently handle the operation given below. You are then required to implement the operation itself.
#### Operation: Get all sensors for an equipment in a given time-range.

`def get_all_sensors(equipment_id: str, start_time: str, end_time: str) -> List`


### Input:
    1. equipment_id - Unique Id belonging to an equipment.
    2. start_time Time - Starting time for getting sensors.
    3. end_time - Ending time for getting sensors.
### Output
    - List of Sensors and the time interval overlapping with the query time-range in which the sensor will be active.
    - The sensor time-range can overlap partially or completely with the query time-range.

## Examples :
```

1. Considering the above example and inputs as :
    - Equipment Id - E2
    - Start Time - 10:30
    - End Time - 19:15
- Output :
    - S2- 11:00 to 12:00 (Sensor time-range lies completely in query time-range)
    - S4- 10:30 to 14:30 (Sensor time-range lies partially in query time-range)
    - S5- 13:45 to 19:15 (Sensor time-range lies partially in query time-range)
    (Skipped S6 since there is no partial/complete overlap b/w S6 and query time-range).
- Final Output
    - **[{"S2: ("11:00", "12:00"), "S4": ("10:30", "14:30"), "S5": ("13:45", "19:15")]**
```
```
2. Considering the above example and inputs as :
- Equipment Id - E2
    - Start Time - 11:45
    - End Time - 19:50
- Output :
    - S2 - 11:45 to 12:00
    - S4 - 11:45 to 14:30 // because S4 available in time range 07:00 - 14:30
    - S5 - 13:45 to 19:50
    - S6 - 19:45 to 19:50
    (Skipped S6 since it doesn't lie in the time-range in the query).
- Final Output
    - **[{"S2: ("11:45", "12:00"), "S4": ("11:45", "14:30"), "S5": ("13:45", "19:50"), "S6": ("19:45", "19:50")]**
```

```
3. Considering the above example and inputs as :
- Equipment Id - E4
    - Start Time - 14:36
    - End Time - 16:10
- Output :
    - S2 - 14:36 to 16:10 (Skipped S4 & S6 since it doesn't lie in the time-range in the query).
- Final Output
    - **[{"S2: ("14:36", "16:10") ]**
```


### Constraints
- Total number of Plants = 50
- Max number of Equipments = 1000
- Total number of Sensors = 1000000
- You must load the user input from a file (please find the sample input format at the bottom). This can be done in the initialization / constructor method.
- All the time-ranges are mentioned are in open close interval i.e [start_time, end_time)
    - Eg - time-range: 06:00 to 07:00 and Sensor is active from 07:00 to 08:00 . Then that sensor will not be included in the output.
- For any time-range mentioned the start_time will always be less than the end_time.
- The format for time is 24-hour Hours:Minutes i.e "%H:%M" . The hours in the input will always be within [0,23) and minutes within [0,60) .
- The query time-range can be anytime from 00:00 to 23:59.

<!-- Notes
You are free to use any programming language for this problem.
You may return the values in any format.
Please mention any/all assumption[s] made along with the submission.
You are free to add any other details that you think might be helpful to solve the
problem as long as the input and output remain unchanged.
You are not required to persist the data in any external storage.
Input
Your code will be tested against this input. -->