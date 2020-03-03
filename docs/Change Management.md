# **Change Management**

## **POST** /V1/CM/Binding

This API call is used to binding another vendor's ticket with a change management runbook.

### Detail Information

> **Title** : Bind a change management runbook

> **Version** : 06/26/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CM/Binding  

> **Authentication** : 

| **Type**              | **In**  | **Name**             |
| --------------------- | ------- | -------------------- |
| Bearer Authentication | Headers | Authentication token |

### Request body (***required**)

| **Name**   | **Type** | **Description**                                              |
| ---------- | -------- | ------------------------------------------------------------ |
| runbookId* | string   | ID of the Change Management Runbook                          |
| ticketId*  | string   | Other vendor's ticket number                                 |
| vendor*    | string   | Name of the vendor                                           |
| ticketName | string   | Name of the ticket, for example: in ServiceNow CHG0030015 (means "number" in ServiceNow) |
| ticketUrl  | string   | The full URL of the tichet, for example: in ServiceNow: https://dev65813.service-now.com/nav_to.do?uri=%2Fincident.do%3Fsys_id%3D1ecf1235dbe2330093890d53ca9619a2%26sysparm_record_target%3Dincident%26sysparm_record_row%3D1%26sysparm_record_rows%3D67%26sysparm_record_list%3DORDERBYDESCnumber |

> ***Example***

```python
body = {
    'vendor': 'serviceNow',
    'ticketId': "00008",
    'runbookId': '7652cb62-c5e6-d0a3-3f22-29972d03ad4c',
    'ticketUrl': 'https://dev65813.service-now.com/nav_to.do?uri=%2Fincident.do%3Fsys_id%3D1ecf1235dbe2330093890d53ca9619a2%26sysparm_record_target%3Dincident%26sysparm_record_row%3D1%26sysparm_record_rows%3D67%26sysparm_record_list%3DORDERBYDESCnumber'
}
```

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
  "statusCode":0,
  "statusDescription":"Success.",
  "runbookUrl":""
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
token = "5e372bc9-3b2e-48fc-b86b-e9c651968f85"
nb_url = "http://192.168.28.173"
full_url = nb_url + "/ServicesAPI/API/V1/CM/Binding"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

body = {
    'vendor': 'serviceNow',
    'ticketId': "00008",
    'runbookId': '7652cb62-c5e6-d0a3-3f22-29972d03ad4c',
    'ticketUrl': 'https://dev65813.service-now.com/nav_to.do?uri=%2Fincident.do%3Fsys_id%3D1ecf1235dbe2330093890d53ca9619a2%26sysparm_record_target%3Dincident%26sysparm_record_row%3D1%26sysparm_record_rows%3D67%26sysparm_record_list%3DORDERBYDESCnumber'
}

try:
    response = requests.post(full_url, data = json.dumps(body), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Binding ticket failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    { "statusCode":790200, "statusDescription":"Success.", "runbookUrl":"http://192.168.28.173/map.html?t=823e096b-093a-10f1-1471-21a9a5ff509c&d=af4581fd-a705-4ddf-a878-fd4c6f304b96&id=378d70a2-f3e1-76c1-42fa-2bb088c1bda2&rba=9948608d-c755-d1e7-b5d1-325b917952b0"}

> **cURL Code from Postman**


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CM/Binding \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: f8ebb763-3a05-45d6-b4ff-1873f5e01b2d' \
  -H 'cache-control: no-cache' \
  -H 'token: 1c52cd65-3247-44ad-91e6-cd73fc6c64a6' \
  -d '{
    "runbookId": "e8f1ab35-b763-bb1b-f921-ac99683ae476",
    "ticketId": "ticketId",
    "vendor": "vendor",
    "ticketName": "ticketName"
}'
```

## **POST** /V1/CM/Approval/State

This API call is used to set CM Runbook approval state example - approve or reject.

### Detail Information

> **Title** : Approve a change management request

> **Version** : 06/26/2019

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CM/Approval/State 

> **Authentication** :

| **Type**              | **In**  | **Name**             |
| --------------------- | ------- | -------------------- |
| Bearer Authentication | Headers | Authentication token |

### Request body (***required**)

| **Name**   | **Type** | **Description**                                              |
| ---------- | -------- | ------------------------------------------------------------ |
| runbookId* | string   | ID of the Change Management Runbook                          |
| ticketId*  | string   | Other vendor's ticket number                                 |
| vendor*    | string   | Name of the vendor                                           |
| state*     | integer  | 0(planning)/1(pending)/2(approved) /3(rejected) /5(archived) |
| ticketName | string   | Name of the ticket, for example: in ServiceNow CHG0030015 (means "number" in ServiceNow) |
| ticketUrl  | string   | The full URL of the tichet, for example: in ServiceNow: https://dev65813.service-now.com/nav_to.do?uri=%2Fincident.do%3Fsys_id%3D1ecf1235dbe2330093890d53ca9619a2%26sysparm_record_target%3Dincident%26sysparm_record_row%3D1%26sysparm_record_rows%3D67%26sysparm_record_list%3DORDERBYDESCnumber |

> ***Example***

```python
body = {
    'vendor': 'serviceNow',
    'ticketId': "00008",
    'runbookId': '7652cb62-c5e6-d0a3-3f22-29972d03ad4c',
    'ticketUrl': 'https://dev65813.service-now.com/nav_to.do?uri=%2Fincident.do%3Fsys_id%3D1ecf1235dbe2330093890d53ca9619a2%26sysparm_record_target%3Dincident%26sysparm_record_row%3D1%26sysparm_record_rows%3D67%26sysparm_record_list%3DORDERBYDESCnumber',
    'state': 2
}
```

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
token = "5e372bc9-3b2e-48fc-b86b-e9c651968f85"
nb_url = "http://192.168.28.173"
full_url = nb_url + "/ServicesAPI/API/V1/CM/Approval/State"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

body = {
    'vendor': 'serviceNow',
    'ticketId': "00008",
    'runbookId': '7652cb62-c5e6-d0a3-3f22-29972d03ad4c',
    'ticketUrl': 'https://dev65813.service-now.com/nav_to.do?uri=%2Fincident.do%3Fsys_id%3D1ecf1235dbe2330093890d53ca9619a2%26sysparm_record_target%3Dincident%26sysparm_record_row%3D1%26sysparm_record_rows%3D67%26sysparm_record_list%3DORDERBYDESCnumber',
    'state': 2
}

try:
    response = requests.post(full_url, data = json.dumps(body), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Ticket change management failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {"runbookUrl":"http://192.168.28.173/map.html?t=823e096b-093a-10f1-1471-21a9a5ff509c&d=af4581fd-a705-4ddf-a878-fd4c6f304b96&id=378d70a2-f3e1-76c1-42fa-2bb088c1bda2&rba=9948608d-c755-d1e7-b5d1-325b917952b0","statusCode":790200,"statusDescription":"Success."}

> **cURL Code from Postman**


```python
curl -X POST \
  http://192.168.28.173/ServicesAPI/API/V1/CM/Approval/State \
  -H 'Accept: */*' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/json' \
  -H 'Host: 192.168.28.173' \
  -H 'Postman-Token: 6417cc52-66ee-44b7-9718-dad31ef898b7,1c24d1d5-441a-4a1f-870e-3127a913d67c' \
  -H 'User-Agent: PostmanRuntime/7.13.0' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache' \
  -H 'content-length: 389' \
  -H 'token: 5e372bc9-3b2e-48fc-b86b-e9c651968f85' \
  -d '{
    "vendor": "serviceNow",
    "ticketId": "00008",
    "runbookId": "7652cb62-c5e6-d0a3-3f22-29972d03ad4c",
    "ticketUrl": "https://dev65813.service-now.com/nav_to.do?uri=%2Fincident.do%3Fsys_id%3D1ecf1235dbe2330093890d53ca9619a2%26sysparm_record_target%3Dincident%26sysparm_record_row%3D1%26sysparm_record_rows%3D67%26sysparm_record_list%3DORDERBYDESCnumber",
    "state": 2
}'
```

POST /V1/CMDB/CM/Tasks/ScheduledTask
------------------------------------

Call this API to create a Scheduled Task (ST) for an APPROVED Network Change
(NC).

**Note:**

-   Only 1 ST can be created on each NC. If there is an existing ST, create
    request is not allowed.

-   ST can only be added to APPROVED NC, so that ST is restricted to be created
    on unapproved NC.

-   APPROVED NC with executed ST doesn’t allow ST creation request.

-   Follow NetBrain system user privileges

### Detail Information

>Title: Change Management Scheduled Task API

>Version: 09/30/2019

>API Server URL: http(s)://IP Address of NetBrain Web API
>Server/ServicesAPI/API/V1/CMDB/CM/Tasks/ScheduledTask

>Authentication:

| **Type**              | **In**  | **Name**                                  |
| --------------------- | ------- | ----------------------------------------- |
| Bearer Authentication | Headers | Authentication token                      |
| Token                 | String  | Authentication token, get from login API. |


### Request body (\*required)

| **Name**               | **Type** | **Description**                                              |
| ---------------------- | -------- | ------------------------------------------------------------ |
| Runbook_id\*           | string   | NC runbook ID                                                |
| ticket_id\*            | string   | 3rd party ITSM ticket ID.                                    |
|                        |          | Note: use either runbook_id or ticket_id. If both are provided, runbook_id has higher priority. |
| execution_time\*       | date     | ST start time. (input time format must follow the UTC structure: 2020-01-17T20:06:00.000Z) |
| do_not_execute_after\* | date     | ST end time. (input time format must follow the UTC structure: 2020-01-17T20:06:00.000Z) |

### Query Parameters (\*required)

>No parameters required.

### Headers

>Data Format Headers

| **Name**     | **Type** | **Description**            |
| ------------ | -------- | -------------------------- |
| Content-Type | String   | Support “application/json” |
| Accept       | String   | Support “application/json” |

>Authorization Headers

| **Name** | **Type** | **Description**                           |
| -------- | -------- | ----------------------------------------- |
| Token    | String   | Authentication token, get from login API. |

### Response

| **Name**          | **Type** | **Description**                                |
| ----------------- | -------- | ---------------------------------------------- |
| scheduled_task_id | String   | ID of the created scheduled task               |
| statusCode        | Integer  | The returned status code of executing the API. |
| statusDescription | String   | The explanation of the status code.            |

> ***Note: Error Code clarification***

| **Code** | **Message**                                                  |
| -------- | ------------------------------------------------------------ |
| 790200   | Success                                                      |
| 795012   | License is expired.                                          |
| 793001   | System framework level error.                                |
| 791000   | The runbook_id or ticket_id is required.                     |
| 791006   | The Change Management runbook is not found.                  |
| 798808   | Schedule can only be applied to approved Change Management runbook. |
| 794011   | Scheduled task of this Change Management runbook is existed. |
| 794011   | OperationFailed.                                             |
| 793001   | InternalError.                                               |


>**Example**


```python
{
    "scheduled_task_id" : "d5c5bb3c-1aad-ae2c-a591-347ede7d71a9",
    "statusCode" : 790200,
    "statusDescription" : "Success"
}
```

### Full Example


```python
# import python modules 
import requests
import time
import urllib3
import pprint
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

token = "87ccc8aa-6550-41cd-a54b-ae11b521a837" 
full_url = "http://192.168.28.139/ServicesAPI/API/V1/CM/Tasks/ScheduleTask" 
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}  
headers['token'] = token

body = {
    "Runbook_id":"453ac967-12ad-f8f1-5158-648500fa67fb",
    "ticket_id":"562d351f1b5e44502fd68774cc4bcb51",
    "execution_time":"2020-01-17T20:06:00.000Z",
    "do_not_execute_after":"2020-01-17T21:06:00.000Z"
}

try:
    # Do the HTTP request
    response = requests.post(full_url, headers=headers, data = json.dumps(body), verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Create CM task failed! - " + str(response.text))
except Exception as e:
    print (str(e))
    
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}

> **cURL Code from Postman**


```python
curl -X POST \
  http://192.168.28.143/ServicesAPI/API/V1/CMDB/CM/Tasks/ScheduledTask \
  -H 'Accept: */*' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Length: 194' \
  -H 'Content-Type: application/json' \
  -H 'Host: 192.168.28.143' \
  -H 'Postman-Token: 1501a820-2ec7-4971-a668-9ee3d742da52,e47745d9-d9f3-408a-9c93-7d0429c7d293' \
  -H 'User-Agent: PostmanRuntime/7.15.2' \
  -H 'cache-control: no-cache' \
  -H 'token: ce4a589c-99d9-4a0e-abb6-5a91d424fad6' \
  -d '{
    "Runbook_id":"d5c5bb3c-1aad-ae2c-a591-347ede7d71a9",
    "ticket_id":"1d9e4d500fe32b4046ddc5ece1050e7e",
    "execution_time":"2020/01/15",
    "do_not_execute_after":"2020/01/16"
}
'
```

DELETE /V1/CMDB/CM/Tasks/ScheduledTask
--------------------------------------

Call this API to delete a “Waiting” status Scheduled Task (ST) of an APPROVED
Network Change (NC).

**Note:**

-   ST on “Running” or “Executed” status cannot be deleted.

-   Follow NetBrain system user privileges

-   Stop a “Running” task?

### Detail Information

> Title: Change Management Scheduled Task API

> Version: 09/30/2019

> API Server URL: http(s)://IP Address of NetBrain Web API
> Server/ServicesAPI/API/V1/CM/Tasks/ScheduleTask

> Authentication:

| **Type**              | **In**  | **Name**             |
| --------------------- | ------- | -------------------- |
| Bearer Authentication | Headers | Authentication token |

### Request body (\*required)

> No body required.

### Query Parameters (\*required)

| **Name**     | **Type** | **Description**           |
| ------------ | -------- | ------------------------- |
| Runbook_id\* | string   | NC runbook ID             |
| ticket_id\*  | string   | 3rd party ITSM ticket ID. |

> ***Note:*** Runbook_id and ticket_id only one should be provided by customer.

### Headers

> Data Format Headers

| **Name**     | **Type** | **Description**            |
| ------------ | -------- | -------------------------- |
| Content-Type | String   | Support “application/json” |
| Accept       | String   | Support “application/json” |

> Authorization Headers

| **Name** | **Type** | **Description**                           |
| -------- | -------- | ----------------------------------------- |
| Token    | String   | Authentication token, get from login API. |

### Response

| **Name**          | **Type** | **Description**                                |
| ----------------- | -------- | ---------------------------------------------- |
| statusCode        | Integer  | The returned status code of executing the API. |
| statusDescription | String   | The explanation of the status code.            |

> ***Note: Error Code clarification***

| **Code** | **Message**                                                  |
| -------- | ------------------------------------------------------------ |
| 790200   | Call running successfully.                                   |
| 791000   | runbook id or ticket id required.                            |
| 791006   | The Change Management runbook is not found.                  |
| 798808   | Schedule can only be applied to approved Change Management runbook. |
| 798808   | Task Schedule is running now.                                |
| 794011   | Failed to delete.                                            |
| 794011   | The scheduled task is running.                               |
| 793001   | System Exception Message.                                    |


> ***Example***

```python
{  
    "statusCode" : "790200",
    "statusDescription" : "Success"
}
```

### Full Example


```python
# import python modules 
import requests
import time
import urllib3
import pprint
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

token = "87ccc8aa-6550-41cd-a54b-ae11b521a837" 
 
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}  
headers['token'] = token
runbook_id = "453ac967-12ad-f8f1-5158-648500fa67fb"
full_url = "http://192.168.28.139/ServicesAPI/API/V1/CM/Tasks/ScheduleTask/"
param = {
    "runbook_Id":runbook_id
}

try:
    # Do the HTTP request
    response = requests.delete(full_url, headers=headers, params = param, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Delete CM task failed! - " + str(response.text))
except Exception as e:
    print (str(e))
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}

> **cURL Code Postman**


```python
curl -X DELETE \
  'http://192.168.28.139/ServicesAPI/API/V1/CM/Tasks/ScheduleTask?runbook_Id=453ac967-12ad-f8f1-5158-648500fa67fb' \
  -H 'Accept: */*' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Length: ' \
  -H 'Content-Type: application/json' \
  -H 'Host: 192.168.28.139' \
  -H 'Postman-Token: 97f4fb3c-aeda-4a85-b8ea-5fe90b5efea8,1c24702f-17a2-4b07-a46c-9fc999ff536a' \
  -H 'User-Agent: PostmanRuntime/7.15.2' \
  -H 'cache-control: no-cache' \
  -H 'token: 87ccc8aa-6550-41cd-a54b-ae11b521a837'
```

GET /V1/CMDB/CM/Tasks/ScheduledTask
-----------------------------------

Call this API to get a Scheduled Task (ST) of an APPROVED Network Change (NC).

**Note:**

-   ST can only be added to APPROVED NC, so that unapproved NC doesn’t have an
    ST. Return NC status and error message in response.

-   Follow NetBrain system user privileges

### Detail Information

>Title: Change Management Scheduled Task API

>Version: 09/30/2019

>API Server URL: http(s)://IP Address of NetBrain Web API
>Server/ServicesAPI/API/V1/CMDB/CM/Tasks/ScheduledTask/

>Authentication:

| **Type**              | **In**  | **Name**             |
| --------------------- | ------- | -------------------- |
| Bearer Authentication | Headers | Authentication token |

### Request body (\*required)

>No body required.

### Query Parameters (\*required)

| **Name**     | **Type** | **Description**           |
| ------------ | -------- | ------------------------- |
| Runbook_id\* | string   | NC runbook ID             |
| ticket_id\*  | string   | 3rd party ITSM ticket ID. |

> ***Note:*** Runbook_id and ticket_id only one should be provided by customer.


### Headers

>Data Format Headers

| **Name**     | **Type** | **Description**            |
| ------------ | -------- | -------------------------- |
| Content-Type | String   | Support “application/json” |
| Accept       | String   | Support “application/json” |

>Authorization Headers

| **Name** | **Type** | **Description**                           |
| -------- | -------- | ----------------------------------------- |
| Token    | String   | Authentication token, get from login API. |

### Response

| **Name**              | **Type** | **Description**                                |
| --------------------- | -------- | ---------------------------------------------- |
| runbook_id            | String   | NC runbook ID                                  |
| scheduled_task_id     | String   | ID of the created scheduled task               |
| scheduler             | String   | User name of the scheduler                     |
| scheduled_task_status | String   | Status of the created scheduled task           |
| execution_time        | date     | ST start time                                  |
| do_not_execute_after  | date     | ST end time                                    |
| status                | String   | Status of scheduled task                       |
| statusCode            | Integer  | The returned status code of executing the API. |
| statusDescription     | String   | The explanation of the status code.            |

> ***Note: Error Code clarification***

| **Code** | **Message**                                                  |
| -------- | ------------------------------------------------------------ |
| 790200   | Success.                                                     |
| 791000   | The runbook_id or ticket_id is required.                     |
| 791006   | The Change Management runbook is not found.                  |
| 798808   | Schedule can only be applied to approved Change Management runbook. |
| 794011   | Task Schedule can not be found.                              |
| 793001   | System Exception Message.                                    |
| 793001   | You're not privileged to get the Network Change.             |

> ***Example***


```python
{
    "runbook_id" : "d5c5bb3c-1aad-ae2c-a591-347ede7d71a9",
    "scheduled_task_id" : "de7d71a9-1aad-ae2c-a591-347ed5c5bb3c",
    "scheduler" : "netbrain",
    "scheduled_task_status" : "pending",
    "execution_time" : "2020/01/15",
    "do_not_execute_after" : "2020/01/16",
    "status" : "pending"
    "statusCode" : "790200",
    "statusDescription" : "Success"
}
```


### Full Example


```python
# import python modules 
import requests
import time
import urllib3
import pprint
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

token = "87ccc8aa-6550-41cd-a54b-ae11b521a837" 
 
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}  
headers['token'] = token
runbook_id = "453ac967-12ad-f8f1-5158-648500fa67fb"
full_url = "http://192.168.28.139/ServicesAPI/API/V1/CM/Tasks/ScheduleTask/"
param = {
    "runbook_Id":runbook_id
}

try:
    # Do the HTTP request
    response = requests.get(full_url, headers=headers, params = param, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Get CM task failed! - " + str(response.text))
except Exception as e:
    print (str(e))
    
```

    {'runbook_id': '453ac967-12ad-f8f1-5158-648500fa67fb', 'scheduled_task_id': 'c0777853-ec19-488e-b207-43c221d1a93e', 'scheduler': 'gongdai.liu', 'execution_time': '2020-01-17T20:06:00Z', 'do_not_execute_after': '2020-01-17T21:06:00Z', 'status': 'waiting', 'statusCode': 790200, 'statusDescription': 'Success.'}

> **cURL Code from Postman**


```python
curl -X GET \
  http://192.168.28.139/ServicesAPI/API/V1/CMDB/CM/Tasks/ScheduledTask/453ac967-12ad-f8f1-5158-648500fa67fb \
  -H 'Accept: */*' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/json' \
  -H 'Host: 192.168.28.139' \
  -H 'Postman-Token: ef87ecaa-beee-44f1-911f-030031433bc6,0da2ebe3-e67c-474c-8e6c-f4f49fa51678' \
  -H 'User-Agent: PostmanRuntime/7.15.2' \
  -H 'cache-control: no-cache' \
  -H 'token: 87ccc8aa-6550-41cd-a54b-ae11b521a837'
```

PUT /V1/CMDB/CM/Tasks/ScheduledTask
-----------------------------------

Call this API to update a Scheduled Task (ST) of an APPROVED Network Change
(NC).

**Note:**

-   Use PUT method to update an ST that is on “Waiting” status.

-   If an ST is on “Running” or “Executed” status, update request is not
    allowed.

-   Follow NetBrain system user privileges

### Detail Information

> Title: Change Management Scheduled Task API

> Version: 09/30/2019

> API Server URL: http(s)://IP Address of NetBrain Web API
> Server/ServicesAPI/API/V1/CMDB/CM/Tasks/ScheduledTask/\<runbook_id/ticket_id\>

> Authentication:

| **Type**              | **In**  | **Name**             |
| --------------------- | ------- | -------------------- |
| Bearer Authentication | Headers | Authentication token |

### Request body (\*required)

| **Name**               | **Type** | **Description**           |
| ---------------------- | -------- | ------------------------- |
| Runbook_id\*           | string   | NC runbook ID             |
| ticket_id\*            | string   | 3rd party ITSM ticket ID. |
| execution_time\*       | date     | ST start time             |
| do_not_execute_after\* | date     | ST end time               |

> ***Note:*** Runbook_id and ticket_id only one should be provided by customer.

### Query Parameters (\*required)

> No parameters required.

### Headers

>  Data Format Headers

| **Name**     | **Type** | **Description**            |
| ------------ | -------- | -------------------------- |
| Content-Type | String   | Support “application/json” |
| Accept       | String   | Support “application/json” |

> Authorization Headers

| **Name** | **Type** | **Description**                           |
| -------- | -------- | ----------------------------------------- |
| Token    | String   | Authentication token, get from login API. |

### Response

| **Name**              | **Type** | **Description**                                |
| --------------------- | -------- | ---------------------------------------------- |
| runbook_id            | String   | NC runbook ID                                  |
| scheduler             | String   | User name of the scheduler                     |
| scheduled_task_status | String   | Status of the created scheduled task           |
| execution_time        | date     | ST start time                                  |
| do_not_execute_after  | date     | ST end time                                    |
| status                | String   | Status of scheduled task                       |
| statusCode            | Integer  | The returned status code of executing the API. |
| statusDescription     | String   | The explanation of the status code.            |

> ***Note: Error Code clarification***

| **Code** | **Message**                                                  |
| -------- | ------------------------------------------------------------ |
| 790200   | Success.                                                     |
| 791000   | The runbook_id or ticket_id is required.                     |
| 791006   | The Change Management runbook is not found.                  |
| 798808   | Schedule can only be applied to approved Change Management runbook. |
| 794011   | Failed to update schedule.                                   |
| 794011   | Task Schedule can not be found.                              |
| 793001   | System Exception Message.                                    |

> ***Example:***


```python
{
    "runbook_id" : "d5c5bb3c-1aad-ae2c-a591-347ede7d71a9",
    "scheduler" : "netbrain",
    "scheduled_task_status" : "pending",
    "execution_time" : "2020/01/15",
    "do_not_execute_after" : "2020/01/16",
    "status" : "pending"
    "statusCode" : "790200",
    "statusDescription" : "Success"
}
```


      File "<ipython-input-2-16544f92b52d>", line 8
        "statusCode" : "790200",
                     ^
    SyntaxError: invalid syntax

### Full Example


```python
# import python modules 
import requests
import time
import urllib3
import pprint
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

token = "87ccc8aa-6550-41cd-a54b-ae11b521a837" 
full_url = "http://192.168.28.139/ServicesAPI/API/V1/CM/Tasks/ScheduleTask"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}  
headers['token'] = token

body = {
    "Runbook_id":"453ac967-12ad-f8f1-5158-648500fa67fb",
#     "ticket_id":"562d351f1b5e44502fd68774cc4bcb51",
    "execution_time":"2020-01-17T20:06:00.000Z",
    "do_not_execute_after":"2020-01-17T21:06:00.000Z"
}

try:
    # Do the HTTP request
    response = requests.put(full_url, headers=headers, data = json.dumps(body), verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Update CM task failed! - " + str(response.text))
except Exception as e:
    print (str(e))
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}

> **cURL Code from Postman**


```python
curl -X PUT \
  http://192.168.28.139/ServicesAPI/API/V1/CMDB/CM/Tasks/ScheduledTask \
  -H 'Accept: */*' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Length: 141' \
  -H 'Content-Type: application/json' \
  -H 'Host: 192.168.28.139' \
  -H 'Postman-Token: 13924834-e736-4274-a598-d36728cc3fa5,29c8b926-d190-4f37-baf7-ff2ca4ca3639' \
  -H 'User-Agent: PostmanRuntime/7.15.2' \
  -H 'cache-control: no-cache' \
  -H 'token: 87ccc8aa-6550-41cd-a54b-ae11b521a837' \
  -d '{
    "Runbook_id":"d5c5bb3c-1aad-ae2c-a591-347ede7d71a9",
    "execution_time":"2020/01/15",
    "do_not_execute_after":"2020/01/16"
}
'
```
