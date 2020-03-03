# **Benchmark Task Management**

## ***POST*** /V1/CMDB/Benchmark/Tasks

This API call is used to add a benchmark task to a domain. 

Note that, as the key, task name should be unique system wide. 

The option for data to be retrieved in this task is 'Built-in Live Data' which the user can see from UI.

### Detail Information

> **Title** : Add Benchmark Task API

> **Version** : 01/24/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Benchmark/Tasks	

> **Authentication** : 

| **Type**              | **In**  | **Name**             |
| --------------------- | ------- | -------------------- |
| Bearer Authentication | Headers | Authentication token |

### Request body (***required**)

| **Name**                            | **Type**       | **Description**                                              |
| ----------------------------------- | -------------- | ------------------------------------------------------------ |
| taskName*                           | string         | The name of the task.                                        |
| description                         | string         | The description of the task. This field is optional.         |
| startDate*                          | string         | The date when the task starts to run. The standard time format is required, for example, '2017-07-13', '2017/07/13'. This field is optional. Current date will be used by default. |
| schedule*                           | object         | The schedule to run the task. The following sub parameters are included in this object: <br>▪ frequency* (string) - the frequency to run the task. This field is required and includes ”once”, “hourly”,” daily”, “weekly” and “monthly” options.<br>▪ interval(string) - the interval to run the task (optional). This field is only valid for “hourly”,” daily”, and “weekly” options and the default value is 1, such as every 1 hour, 1 week.<br>▪ startTime* (string) - the time to run the task. This field is required and startTime should be in format: ["HH:mm:ss"], if you put date time format such as "2018/04/04 19:20:20 ", "19:20:20" will be used and the date part "2018/04/04" will be ignored.<br> **Note:** Set the time according to your IIS server time zone since the time zone of your ISS server rather than your physical time zone is adopted by the benchmark task.<br>▪ weekday(integer) - the day of the week to run the task. This field is optional and only valid when the frequency is weekly.  0 stands for Sunday, 6 for Saturday and 1-5 for Monday to Friday respectively.<br>▪ dayOfMonth(integer) - which day of a month to run the task. This field is optional and only valid when the frequency is monthly. The default is 1.<br>▪ Months(integer) - which month to run the task. This field is optional and only valid when the frequency is monthly. The default is all 12 months. |
| deviceScope*                        | string         | The devices included in this task.                           |
| deviceScope.scopeType               | string         | scope type options:<br>"all" for all devices of current domain, deviceScope.scopes will be ignored if this field is set to "all";<br>"deviceGroup" for specified group, if set deviceScope.scopes would be list of full path to device groups, such as \["Public/devgrp1", "Private/devgrp2", "System/devgrp3"\];<br>"site" for a particular site. if set deviceScope.scopes would be list of full path to sites, for example: \["My Networks/US/MA/Boston", "My Networks/US/ME/Portland"\] |
| deviceScope.scopes                  | list of string | ignored if deviceScope.scopeType is set to "all";<br>full path to device groups, such as \["Public/devgrp1", "Private/devgrp2", "System/devgrp3"\] if deviceScope.scopeType is set to "deviceGroup";<br>full path to sites, such as \["My Networks/US/MA/Boston", "My Networks/US/ME/Portland"\] if deviceScope.scopeType is set to "site"; |
| limitRunMins                        | string         | The time used to retrieve the data. When it reaches the specified time, the task will stop retrieving more data. This field is optional. |
| cliCommands                         | string         | The customized CLI commands to retrieve data (for example, ["show version", "show arp"]. This field is optional. |
| isBuildIPv4L3Topo                   | bool           | Determine whether to build IPv4 L3 topology. This field is optional and the default value is false. |
| isBuildIPv6L3Topo                   | bool           | Determine whether to build IPv6 L3 topology. This field is optional and the default value is false. |
| isBuildL2Topo                       | bool           | Determine whether to build L2 topology. This field is optional and the default value is false. |
| isBuildIPsecVPNTopology             | bool           | Determine whether to build IPsecVPN topology. This field is optional and the default value is false. |
| isRecalculateDynamicDeviceGroups    | bool           | Determine whether to recalculate dynamic device groups. This field is optional and the default value is false. |
| sRecalculateSite                    | bool           | Determine whether to rebuild sites. This field is optional and the default value is false. |
| isRecalculateMPLSVirtualRouteTables | bool           | Determine whether to recalculate MPLS Virtual Route Table. This field is optional and the default value is false. |
| isbuildDefaultDeviceDataView        | bool           | Determine whether to build default device data view. This field is optional and the default value is false. |
| isEnable                            | bool           | Determine whether to enable the task. This field is optional and the default value is true. |

> ***Example***


```python
body = {
        "taskName":taskName, #The name of the task.
        "startDate":startDate, #The date when the task starts to run. The standard time format is required, for example, '2017-07-13', '2017/07/13'.
        "schedule":{
            "frequency":frequency, #The frequency to run the task. This field is required and includes ”once”, “hourly”,” daily”, “weekly” and “monthly” options.
            "startTime":[startTime] #The time to run the task. This field is required and startTime should be in format: ["HH:mm:ss"], if you put date time format such as "2018/04/04 19:20:20 ", "19:20:20" will be used and the date part "2018/04/04" will be ignored.
            },
        "deviceScope" : {
            "scopeType" : scopeType
            }
        }
```

### Parameters (***required**)

> No parameters required.

### Headers

> **Data Format Headers**

| **Name**     | **Type** | **Description**            |
| ------------ | -------- | -------------------------- |
| Content-Type | string   | support "application/json" |
| Accept       | string   | support "application/json" |

> **Authorization Headers**

| **Name** | **Type** | **Description**                           |
| -------- | -------- | ----------------------------------------- |
| token    | string   | Authentication token, get from login API. |


### Response

| **Name**          | **Type** | **Description**                                              |
| ----------------- | -------- | ------------------------------------------------------------ |
| statusCode        | integer  | Code issued by NetBrain server indicating the execution result. |
| statusDescription | string   | The explanation of the status code.                          |

> ***Example***


```python
{
    "statusCode": 790200,
    "statusDescription": "Success."
}
```

### Full Example 


```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request inputs
token = "dbd4e523-5964-4c2d-ba8f-da018cfb6299"
nb_url = "http://192.168.28.79"
taskName = "Scheduled System DiscoveryGDL11"
startDate = "2019-01-16"
frequency = "once"
startTime = "14:40:20"
scopeType = "all"

# Add a new Benchmark
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Benchmark/Tasks"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

body = {
    "taskName":taskName, 
    "startDate":startDate, 
    "schedule":{
        "frequency":frequency, 
        "startTime":[startTime]
        },
    "deviceScope" : {
        "scopeType" : scopeType
        }
}

try:
    response = requests.post(full_url, data=json.dumps(body), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Benchmark Task added Failed! - " + str(response.text))

except Exception as e:
    print (str(e))

```

    {'statusCode': 790200, 'statusDescription': 'Success.'}

> **cURL Code from Postman**


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Benchmark/Tasks \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: a6b7d2b7-f331-4418-bcc0-d1a90f01dbd8' \
  -H 'Token: c00de805-9210-44a9-9a26-f0c1e944ea36' \
  -H 'cache-control: no-cache' \
  -d 'body = {
                "taskName":"Scheduled System DiscoveryGDL1", 
                "startDate":"2019-01-16", 
                "schedule":{
                    "frequency":"once", 
                    "startTime":["14:40:20"]
                    },
                "deviceScope" : {
                    "scopeType" : "all"
                    }
            }'
```

> **Error Examples**


```python
###################################################################################################################    

"""Error 1: empty taskName"""

Input:
    
    taskName = "" 
    startDate = "2019-01-16"
    frequency = "once"
    startTime = "14:40:20"
    scopeType = "all"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'taskName' cannot be null."
        }"

###################################################################################################################    

"""Error 2: tesk name duplicated"""

Input:
    
    taskName = "Scheduled System Discovery" #Benchmark with task name "Scheduled System Discovery" already exist in system.
    startDate = "2019-01-16"
    frequency = "once"
    startTime = "14:40:20"
    scopeType = "all"
    
Response:        
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791007,
            "statusDescription":"Benchmark task with name Scheduled System Discovery already exists."
        }"

###################################################################################################################    

"""Error 1: empty startData"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = ""
    frequency = "once"
    startTime = "14:40:20"
    scopeType = "all"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'startDate' cannot be null."
        }"
        
###################################################################################################################    

"""Error 1: wrong startData format"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = "20190116"
    frequency = "once"
    startTime = "14:40:20"
    scopeType = "all"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'startDate' cannot be null."
        }"
        
###################################################################################################################    

"""Error 1: empty frequency"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = "2019-01-16"
    frequency = ""
    startTime = "14:40:20"
    scopeType = "all"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'schedule frequency' cannot be null."
        }"
        
###################################################################################################################    

"""Error 1: wrong frequency input"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = "2019-01-16"
    frequency = "1"
    startTime = "14:40:20"
    scopeType = "all"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791001,
            "statusDescription":"Invalid  parameter: the parameter 'schedule.frequency' is invalid.  
            "Options: once, hourly, daily, weekly, monthly "
        }"
        
###################################################################################################################    

"""Error 1: empty startTime"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = "2019-01-16"
    frequency = "once"
    startTime = ""
    scopeType = "all"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'schedule startTime' cannot be null."
        }"
        
###################################################################################################################    

"""Error 1: wrong format startTime"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = "2019-01-16"
    frequency = "once"
    startTime = "14/40/20"
    scopeType = "all"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'schedule startTime' cannot be null."
        }"
        
###################################################################################################################    

"""Error 1: empty scopeType"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = "2019-01-16"
    frequency = "once"
    startTime = "14:40:20"
    scopeType = ""
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":792009,
            "statusDescription":"Invalid data type. deviceScope.scopeType, options: all,deviceGroup,site"
        }"
        
###################################################################################################################    

"""Error 1: wrong scopeType input"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = "2019-01-16"
    frequency = "once"
    startTime = "14:40:20"
    scopeType = "whole"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":792009,
            "statusDescription":"Invalid data type. deviceScope.scopeType, options: all,deviceGroup,site"
        }"
```



## ***DELETE*** /V1/CMDB/Benchmark/Tasks/{task_name}

This API call is used to delete a benchmark task definition by task name. It doesn't impact running task.

### Detail Information

> **Title** : Delete Benchmark Task API

> **Version** : 01/24/2019.

> **API Server URL** : http(s)://"IP address of NetBrain Web API Server"/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/{task_name}

> **Authentication** : 

| **Type**              | **In**  | **Name**             |
| --------------------- | ------- | -------------------- |
| Bearer Authentication | Headers | Authentication token |

### Request body (***required**)

>No request body

### Path Parameters (***required**)

| **Name**  | **Type** | **Description**                                              |
| --------- | -------- | ------------------------------------------------------------ |
| taskname* | string   | The name of benchmark task which created by user calling the Add Benchmark API |

### Headers

> **Data Format Headers**

| **Name**     | **Type** | **Description**            |
| ------------ | -------- | -------------------------- |
| Content-Type | string   | support "application/json" |
| Accept       | string   | support "application/json" |

> **Authorization Headers**

| **Name** | **Type** | **Description**                           |
| -------- | -------- | ----------------------------------------- |
| token    | string   | Authentication token, get from login API. |

### Response

| **Name**          | **Type** | **Description**                                              |
| ----------------- | -------- | ------------------------------------------------------------ |
| statusCode        | integer  | Code issued by NetBrain server indicating the execution result. |
| statusDescription | string   | The explanation of the status code.                          |

> ***Example***


```python
{
    "statusCode": 790200,
    "statusDescription": "Success."
}
```

### Full Example 


```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

token = "dbd4e523-5964-4c2d-ba8f-da018cfb6299"
task_name = "Scheduled System DiscoveryGDL"
nb_url = "http://192.168.28.79"

full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/" + task_name

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

try:
    response = requests.delete(full_url, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Benchmark Task running Failed! - " + str(response.text))
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}

> **cURL Code from Postman**


```python
curl -X DELETE \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/Scheduled%20System%20DiscoveryGDL \
  -H 'Postman-Token: 2e8959b4-202f-40a3-bd4b-e13c6d02a728' \
  -H 'Token: 35c83b3a-2c2c-4332-9d73-e21f2174904f' \
  -H 'cache-control: no-cache'
```

> **Error Examples**


```python
###################################################################################################################    

"""Error 1: empty task_name"""

Input:
    task_name = ""

Response:
    "Benchmark Task running Failed! - 
        {
            "statusCode":793405,
            "statusDescription":"Method is not supported"
        }"
        
###################################################################################################################    

"""Error 2: the benchmark task with provided task_name does not exist"""

Input:
    task_name = "Scheduled System DiscoveryGDL"

Response:
    "Benchmark Task running Failed! - 
        {
            "statusCode":794004,
            "statusDescription":"Task 'Scheduled System DiscoveryGDL' does not exist."
        }"
```



## ***GET*** /V1/CMDB/Benchmark/Tasks/{taskname}/Runs

This API call returns historical executions of a  scheduled benchmark task .

### Detail Information

> **Title** : Get Benchmark Task Running History API

> **Version** : 01/25/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/{taskname}/Runs

> **Authentication** : 

| **Type**              | **In**  | **Name**             |
| --------------------- | ------- | -------------------- |
| Bearer Authentication | Headers | Authentication token |

### Request body (***required**)

> No Request Body.

### Path Parameters (***required**)

| **Name**  | **Type** | **Description**                                              |
| --------- | -------- | ------------------------------------------------------------ |
| taskname* | string   | The name of benchmark task which created by user calling the Add Benchmark API |

### Headers

> **Data Format Headers**

| **Name**     | **Type** | **Description**            |
| ------------ | -------- | -------------------------- |
| Content-Type | string   | support "application/json" |
| Accept       | string   | support "application/json" |

> **Authorization Headers**

| **Name** | **Type** | **Description**                           |
| -------- | -------- | ----------------------------------------- |
| token    | string   | Authentication token, get from login API. |

### Response

| **Name**          | **Type**       | **Description**                                              |
| ----------------- | -------------- | ------------------------------------------------------------ |
| runs              | list of object | One scheduled task can be executed many times, periodically or manually by user. Each exection creates a run record. |
| runId             | string         | ID of this execution. which can be used as input of get results of one specific run. |
| startTime         | string         | start time                                                   |
| endTime           | string         | end time, if not end yet, this field would not present       |
| status            | integer        | Status of this exection of scheduled task.<br>Possible values:<br>2, Running<br>10, Succeeded<br>11, Succeeded with warnning<br>20, Failed<br>30, Manually stopped<br>31, Automatically stopped due to timeout set by user or other system setting |
| isFinished        | bool           | true or false                                                |
| isStopByUser      | bool           | true or false                                                |
| statusCode        | integer        | Code issued by NetBrain server indicating the execution result. |
| statusDescription | string         | The explanation of the status code.                          |

> ***Example***


```python
{
    'runs': [
        {
            'runId': 'f99c21ba-6f92-4958-9964-37597ec65b69', 
            'startTime': '2019-01-25T14:04:30Z', 
            'endTime': '2019-01-25T14:04:42Z', 
            'status': 10, 
            'isFinished': True
        }, 
        {
            'runId': '7a43e9fc-a7e2-4cb2-8b33-6412c2ab551d', 
            'startTime': '2019-01-25T14:07:03Z', 
            'endTime': '2019-01-25T14:07:16Z', 
            'status': 10, 
            'isFinished': True
        }, 
        {
            'runId': 'c34a2b3b-c0b6-465f-8008-f7a16c757202', 
            'startTime': '2019-01-25T14:07:38Z', 
            'endTime': '2019-01-25T14:07:51Z', 
            'status': 10, 
            'isFinished': True
        }, 
        {
            'runId': '8dca23b8-89d7-427a-8007-72c87529cb16', 
            'startTime': '2019-01-25T14:08:06Z', 
            'endTime': '2019-01-25T14:08:18Z', 
            'status': 10, 
            'isFinished': True
        }, 
        {
            'runId': 'edce37a6-093d-438a-836b-18114cb92785', 
            'startTime': '2019-01-25T14:12:55Z', 
            'endTime': '2019-01-25T14:13:08Z', 
            'status': 10, 
            'isFinished': True
        },
        {
            'runId': '7e3843ac-8f07-45dc-8335-b1bbb163f8ef', 
            'startTime': '2019-01-25T14:13:17Z', 
            'endTime': '2019-01-25T14:13:29Z', 
            'status': 10, 
            'isFinished': True
        }, 
        {
            'runId': '476cd7ea-bb26-43e0-a3a6-8fd1a6e70a58', 
            'startTime': '2019-01-25T17:58:20Z', 
            'endTime': '2019-01-25T17:58:32Z', 
            'status': 10, 
            'isFinished': True
        }
    ], 
    
    'statusCode': 790200, 
    'statusDescription': 'Success.'
}
```

### Full Example 


```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request inputs
token = "e074d192-3f21-4ae8-b5f1-405d240b65ca"
nb_url = "http://192.168.28.79"
taskName = "Scheduled System DiscoveryGDL11"

full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/" + taskName + "/Runs"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

try:
    response = requests.get(full_url, headers=headers, verify=False)
    if response.status_code == 200:
        res = response.json()
        print (res)
    else:
        print ("Benchmark Task running Failed! - " + str(response.text))

except Exception as e:
        print (str(e)) 
```

    {'runs': [{'runId': 'f99c21ba-6f92-4958-9964-37597ec65b69', 'startTime': '2019-01-25T14:04:30Z', 'endTime': '2019-01-25T14:04:42Z', 'status': 10, 'isFinished': True}, {'runId': '7a43e9fc-a7e2-4cb2-8b33-6412c2ab551d', 'startTime': '2019-01-25T14:07:03Z', 'endTime': '2019-01-25T14:07:16Z', 'status': 10, 'isFinished': True}, {'runId': 'c34a2b3b-c0b6-465f-8008-f7a16c757202', 'startTime': '2019-01-25T14:07:38Z', 'endTime': '2019-01-25T14:07:51Z', 'status': 10, 'isFinished': True}, {'runId': '8dca23b8-89d7-427a-8007-72c87529cb16', 'startTime': '2019-01-25T14:08:06Z', 'endTime': '2019-01-25T14:08:18Z', 'status': 10, 'isFinished': True}, {'runId': 'edce37a6-093d-438a-836b-18114cb92785', 'startTime': '2019-01-25T14:12:55Z', 'endTime': '2019-01-25T14:13:08Z', 'status': 10, 'isFinished': True}, {'runId': '7e3843ac-8f07-45dc-8335-b1bbb163f8ef', 'startTime': '2019-01-25T14:13:17Z', 'endTime': '2019-01-25T14:13:29Z', 'status': 10, 'isFinished': True}, {'runId': '476cd7ea-bb26-43e0-a3a6-8fd1a6e70a58', 'startTime': '2019-01-25T17:58:20Z', 'endTime': '2019-01-25T17:58:32Z', 'status': 10, 'isFinished': True}], 'statusCode': 790200, 'statusDescription': 'Success.'}

> **cURL Code from Postman**


```python
curl -X GET \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/Scheduled%20System%20DiscoveryGDL11/Runs \
  -H 'Postman-Token: 6cd44976-3232-4d79-a0a3-adf8290e2ea0' \
  -H 'cache-control: no-cache' \
  -H 'token: e074d192-3f21-4ae8-b5f1-405d240b65ca'
```

> **Error Examples**


```python
###################################################################################################################    

"""Error 1: empty task name"""

Input:
    
    taskName = ""

Response:
    
    "Benchmark Task running Failed! - 
        {
            "statusCode":793405,
            "statusDescription":"Method is not supported"
        }"

###################################################################################################################    

"""Error 1: Benchmark Task dosn't exist"""

Input:
    
    taskName = ""

Response:
    
    "Benchmark Task running Failed! - 
        {
            "statusCode":794004,
            "statusDescription":"Task 'Scheduled System DiscoveryGDL' does not exist."
        }"
```



## ***POST*** /V1/CMDB/Benchmark/Tasks/{taskname}/Run

This API call is used to run a  benchmark task right away, specified by task name. Error would return if the task is already running.

### Detail Information

> **Title** : Run Benchmark Task Now API

> **Version** : 01/25/2019.

> **API Server URL** : http(s)://"IP address of NetBrain Web API Server"/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/{taskname}/Run

> **Authentication** : 

| **Type**              | **In**  | **Name**             |
| --------------------- | ------- | -------------------- |
| Bearer Authentication | Headers | Authentication token |

### Request body (***required**)

> No Request Body.

### Path Parameters (***required**)

| **Name**  | **Type** | **Description**                                              |
| --------- | -------- | ------------------------------------------------------------ |
| taskname* | string   | The name of benchmark task which created by user calling the Add Benchmark API |

### Headers

> **Data Format Headers**

| **Name**     | **Type** | **Description**            |
| ------------ | -------- | -------------------------- |
| Content-Type | string   | support "application/json" |
| Accept       | string   | support "application/json" |

> **Authorization Headers**

| **Name** | **Type** | **Description**                           |
| -------- | -------- | ----------------------------------------- |
| token    | string   | Authentication token, get from login API. |

### Response

| **Name**          | **Type** | **Description**                                              |
| ----------------- | -------- | ------------------------------------------------------------ |
| statusCode        | integer  | Code issued by NetBrain server indicating the execution result. |
| statusDescription | string   | The explanation of the status code.                          |

> ***Example***


```python
{
    "statusCode": 790200,
    "statusDescription": "Success."
}
```

### Full Example


```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request inputs
token = "35c83b3a-2c2c-4332-9d73-e21f2174904f"
nb_url = "http://192.168.28.79"
taskName = "Scheduled System DiscoveryGDL11"

full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/" + taskName + "/Run"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

try:
    response = requests.post(full_url, headers=headers, verify=False)
    if response.status_code == 200:
        res = response.json()
        print(res) 
    else:
        print ("Benchmark Task running Failed! - " + str(response.text))

except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}

> **cURL Code from Postman**


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/Scheduled%20System%20DiscoveryGDL11/Run \
  -H 'Postman-Token: 5dca2710-942f-4a1d-a133-cc38b6f4a9a1' \
  -H 'cache-control: no-cache' \
  -H 'token: 35c83b3a-2c2c-4332-9d73-e21f2174904f'
```

> **Error Examples**


```python
###################################################################################################################    

"""Error 1: trigger a running benchmark task"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL11" #This task is already running before calling this API
    
Response:
    
    "Benchmark Task running Failed! - 
        {
            "statusCode":794005,
            "statusDescription":"Failed to start task This benchmark task is running currently. 
                                "It cannot be submitted again until it is completed."
        }"
        
###################################################################################################################    

"""Error 2: empty task name"""

Input:
    
    taskName = "" 
    
Response:
    
    "Benchmark Task running Failed! - 
        {
            "statusCode":793405,
            "statusDescription":"Method is not supported"
        }"
        
###################################################################################################################    

"""Error 3: wrong task name"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" #user type the wrong name of a benchmark task.
    
Response:
    
    "Benchmark Task running Failed! - 
        {
            "statusCode":794004,
            "statusDescription":"Task 'Scheduled System DiscoveryGDL' does not exist."
        }"
        
###################################################################################################################    

"""Error 4: trigger a benchmark which end date has passed and the corresponding job not exist anymore."""

Input:
    
    taskName = "Basic System Benchmark" # The end date of this task was in 2016 and the corresponding job has been deleted.
    
Response:
    
    "Benchmark Task running Failed! - 
        {
            "statusCode":794005,
            "statusDescription":"Failed to start task Failed to run benchmark task due to exception \"
            Job 58121100-20fa-4461-8d40-31e4923e00e2 not found in database.\"."
        }"
```



## ***PUT*** /V1/CMDB/Benchmark/Tasks

This API call is used to update an existing benchmark task.

### Detail Information

> **Title** : Update Benchmark Task API

> **Version** : 01/24/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Benchmark/Tasks	

> **Authentication** : 

| **Type**              | **In**  | **Name**             |
| --------------------- | ------- | -------------------- |
| Bearer Authentication | Headers | Authentication token |

### Request body (***required**)

| **Name**                            | **Type**       | **Description**                                              |
| ----------------------------------- | -------------- | ------------------------------------------------------------ |
| taskName*                           | string         | The name of the task.                                        |
| newTaskName                         | string         | the new task name, optional.                                 |
| description                         | string         | The description of the task. This field is optional.         |
| startDate*                          | string         | The date when the task starts to run. The standard time format is required, for example, '2017-07-13', '2017/07/13'. This field is optional. Current date will be used by default. |
| schedule*                           | object         | The schedule to run the task. The following sub parameters are included in this object: <br>▪ frequency* (string) - the frequency to run the task. This field is required and includes ”once”, “hourly”,” daily”, “weekly” and “monthly” options.<br>▪ interval(string) - the interval to run the task (optional). This field is only valid for “hourly”,” daily”, and “weekly” options and the default value is 1, such as every 1 hour, 1 week.<br>▪ startTime* (string) - the time to run the task. This field is required and startTime should be in format: ["HH:mm:ss"], if you put date time format such as "2018/04/04 19:20:20 ", "19:20:20" will be used and the date part "2018/04/04" will be ignored.<br> **Note:** Set the time according to your IIS server time zone since the time zone of your ISS server rather than your physical time zone is adopted by the benchmark task.<br>▪ weekday(integer) - the day of the week to run the task. This field is optional and only valid when the frequency is weekly.  0 stands for Sunday, 6 for Saturday and 1-5 for Monday to Friday respectively.<br>▪ dayOfMonth(integer) - which day of a month to run the task. This field is optional and only valid when the frequency is monthly. The default is 1.<br>▪ Months(integer) - which month to run the task. This field is optional and only valid when the frequency is monthly. The default is all 12 months. |
| deviceScope*                        | string         | the devices included in this task.                           |
| deviceScope.scopeType               | string         | scope type options:<br>"all" for all devices of current domain, deviceScope.scopes will be ignored if this field is set to "all";<br>"deviceGroup" for specified group, if set deviceScope.scopes would be list of full path to device groups, such as \["Public/devgrp1", "Private/devgrp2", "System/devgrp3"\];<br>"site" for a particular site. if set deviceScope.scopes would be list of full path to sites, for example: \["My Networks/US/MA/Boston", "My Networks/US/ME/Portland"\] |
| deviceScope.scopes                  | list of string | ignored if deviceScope.scopeType is set to "all";<br>full path to device groups, such as \["Public/devgrp1", "Private/devgrp2", "System/devgrp3"\] if deviceScope.scopeType is set to "deviceGroup";<br>full path to sites, such as \["My Networks/US/MA/Boston", "My Networks/US/ME/Portland"\] if deviceScope.scopeType is set to "site"; |
| limitRunMins                        | string         | The time used to retrieve the data. When it reaches the specified time, the task will stop retrieving more data. This field is optional. |
| cliCommands                         | string         | The customized CLI commands to retrieve data (for example, ["show version", "show arp"]. This field is optional. |
| isBuildIPv4L3Topo                   | bool           | Determine whether to build IPv4 L3 topology. This field is optional and the default value is false. |
| isBuildIPv6L3Topo                   | bool           | Determine whether to build IPv6 L3 topology. This field is optional and the default value is false. |
| isBuildL2Topo                       | bool           | Determine whether to build L2 topology. This field is optional and the default value is false. |
| isBuildIPsecVPNTopology             | bool           | Determine whether to build IPsecVPN topology. This field is optional and the default value is false. |
| isRecalculateDynamicDeviceGroups    | bool           | Determine whether to recalculate dynamic device groups. This field is optional and the default value is false. |
| sRecalculateSite                    | bool           | Determine whether to rebuild sites. This field is optional and the default value is false. |
| isRecalculateMPLSVirtualRouteTables | bool           | Determine whether to recalculate MPLS Virtual Route Table. This field is optional and the default value is false. |
| isbuildDefaultDeviceDataView        | bool           | Determine whether to build default device data view. This field is optional and the default value is false. |
| isEnable                            | bool           | Determine whether to enable the task. This field is optional and the default value is true. |

> **Example**


```python
# Update task name:
body = {
        "taskName":taskName, #The name of the task.
        "newTaskName":newTaskName, #The new task name.
        "startDate":startDate, #The date when the task starts to run. The standard time format is required, for example, '2017-07-13', '2017/07/13'.
        "schedule":{
            "frequency":frequency, #The frequency to run the task. This field is required and includes ”once”, “hourly”,” daily”, “weekly” and “monthly” options.
            "startTime":[startTime] #The time to run the task. This field is required and startTime should be in format: ["HH:mm:ss"], if you put date time format such as "2018/04/04 19:20:20 ", "19:20:20" will be used and the date part "2018/04/04" will be ignored.
            },
        "deviceScope" : {
            "scopeType" : scopeType
            }
        }


# Update other attribute:
body = {
        "taskName":taskName, #The name of the task.
        "startDate":newStartDate, #The date when the task starts to run. The standard time format is required, for example, '2017-07-13', '2017/07/13'.
        "schedule":{
            "frequency":newFrequency, #The frequency to run the task. This field is required and includes ”once”, “hourly”,” daily”, “weekly” and “monthly” options.
            "startTime":[startTime] #The time to run the task. This field is required and startTime should be in format: ["HH:mm:ss"], if you put date time format such as "2018/04/04 19:20:20 ", "19:20:20" will be used and the date part "2018/04/04" will be ignored.
            },
        "deviceScope" : {
            "scopeType" : scopeType
            }
        }

```

### Parameters (***required**)

> No parameters required.

### Headers

> **Data Format Headers**

| **Name**     | **Type** | **Description**            |
| ------------ | -------- | -------------------------- |
| Content-Type | string   | support "application/json" |
| Accept       | string   | support "application/json" |

> **Authorization Headers**

| **Name** | **Type** | **Description**                           |
| -------- | -------- | ----------------------------------------- |
| token    | string   | Authentication token, get from login API. |


### Response

| **Name**          | **Type** | **Description**                                              |
| ----------------- | -------- | ------------------------------------------------------------ |
| statusCode        | integer  | Code issued by NetBrain server indicating the execution result. |
| statusDescription | string   | The explanation of the status code.                          |

> ***Example***


```python
{
    "statusCode": 790200,
    "statusDescription": "Success."
}
```

### Full Example 


```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request parameters
token = "dbd4e523-5964-4c2d-ba8f-da018cfb6299"
nb_url = "http://192.168.28.79"
taskName = "Scheduled System DiscoveryGDL"
startDate = "2019-01-16"
#frequency = "weekly" #Update the benchmark running frequency.
startTime = "14:40:20"
scopeType = "all"

full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Benchmark/Tasks"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

body = {
    "taskName":taskName, #The name of the task.
    "startDate":startDate, #The date when the task starts to run. The standard time format is required, for example, '2017-07-13', '2017/07/13'.
    "schedule":{
        "frequency":frequency, #The frequency to run the task. This field is required and includes ”once”, “hourly”,” daily”, “weekly” and “monthly” options.
        "startTime":[startTime] #The time to run the task. This field is required and startTime should be in format: ["HH:mm:ss"], if you put date time format such as "2018/04/04 19:20:20 ", "19:20:20" will be used and the date part "2018/04/04" will be ignored.
        },
    "deviceScope" : {
        "scopeType" : scopeType
        }
}
    
try:
    response = requests.put(full_url, data=json.dumps(body), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print(result)
    else:
        print ("Benchmark Task updated Failed! - " + str(response.text))

except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}

> **cURL Code from Postman**


```python
curl -X PUT \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Benchmark/Tasks \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: b277650f-f646-474d-954c-104a028f9a9f' \
  -H 'cache-control: no-cache' \
  -H 'token: c00de805-9210-44a9-9a26-f0c1e944ea36' \
  -d 'body = {
    "taskName":"Scheduled System DiscoveryGDL1", 
    "startDate":"2020-01-16", 
    "schedule":{
        "frequency":"weekly", 
        "startTime":["14:40:20"]
        },
    "deviceScope" : {
        "scopeType" : "all"
        }
}'
```


```python
newTaskName = ""
frequency = ""

body = {
    "taskName":taskName, #The name of the task.
    "newTaskName":newTaskName,
    "startDate":startDate, #The date when the task starts to run. The standard time format is required, for example, '2017-07-13', '2017/07/13'.
    "schedule":{
        "frequency":frequency, #The frequency to run the task. This field is required and includes ”once”, “hourly”,” daily”, “weekly” and “monthly” options.
        "startTime":[startTime] #The time to run the task. This field is required and startTime should be in format: ["HH:mm:ss"], if you put date time format such as "2018/04/04 19:20:20 ", "19:20:20" will be used and the date part "2018/04/04" will be ignored.
        },
    "deviceScope" : {
        "scopeType" : scopeType
        }
}
    
try:
    response = requests.put(full_url, data=json.dumps(body), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print(result)
    else:
        print ("Benchmark Task updated Failed! - " + str(response.text))

except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}

> **Error Examples and Note**


```python
###################################################################################################################    

"""Error 1: new task name same with original task name"""

Input:
    taskName = "Scheduled System DiscoveryGDL"
    newTaskName = "Scheduled System DiscoveryGDL"
    startDate = "2019-01-16"
    frequency = "weekly" 
    startTime = "14:40:20"
    scopeType = "all"


Response:
    "{
        "statusCode":791007,
        "statusDescription":"newTaskName already exists."
     }"
    
###################################################################################################################    

"""Error 2: one or some required attributs update as null"""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
    taskName = "Scheduled System DiscoveryGDL"
    newTaskName = "" #new task name update as null
    startDate = "2019-01-16"
    frequency = "" #benchmart running frequency update as null
    startTime = "14:40:20"
    scopeType = "all"


Response:
    "{
        "statusCode": 790200,
        "statusDescription": "Success."
     }"

###################################################################################################################    

"""Error 3: update date or time with wrong format""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
    taskName = "Scheduled System DiscoveryGDL"
    startDate = "20190116" #wrong format of start date
    frequency = "weekly" 
    startTime = "14:40:20"
    scopeType = "all"


Response:
    "{
        "statusCode": 790200,
        "statusDescription": "Success."
     }"

###################################################################################################################    

"""Note 1: no further update(not consider "newTaskname")"""

Input:
    taskName = "Scheduled System DiscoveryGDL"
    startDate = "2019-01-16"
    frequency = "weekly" 
    startTime = "14:40:20"
    scopeType = "all"
    #All inputs same with original benchmark informations.

Response:
    "{
        'statusCode': 790200, 
        'statusDescription': 'Success.'
     }"
        
```
