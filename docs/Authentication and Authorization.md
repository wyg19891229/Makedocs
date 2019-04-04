# Authentication and Authorization API

## ***GET*** /V1/CMDB/Domains/{?tenantId}
Use this function returns a list of accessible domains in a specific tenant. The returned accessible domains vary by the user privileges you use to log in. To retrieve a full list of domains in a specified tenant, you must log in with system admin or tenant admin permissions. 

###Detail Information

> **Title** : Get all accessible domains of a tenants API<br>

> **Version** : 01/23/2019.

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server /ServicesAPI/API/V1/CMDB/Domains

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|Bearer Authentication| Parameters | Authentication token | 

###Request body(****required***)

>No request body.

###Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
| tenantId* | string  | Unique identifier for the tenant from which you desire to retrieve the domain information. tenantId can be retrieved from get all accessible tenants.<br> **Note:** If user don't have the privilege to visit all tenants, specific tenantId is required for this API. |

###Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| Content-Type | string  | support "application/json" |
| Accept | string  | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| token* | string  | Authentication token, get from login API. |

###Response

|**Name**|**Type**|**Description**|
|------|------|------|
|domains | array | A list of all accessible domains. |
|domainId| string | The domain ID.  |
|domainName| string | The domain name. |
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***


```python
{
    'domains': [
        {
            'domainId': '850ff5e9-c639-404d-85a3-d920dbee509c', 
            'domainName': 'Support and Service'
        }, 
        {
            'domainId': '0201adc4-ae96-46f0-ae3d-01cdba9e41d6', 
            'domainName': 'GE Test'
        }
    ], 
    
    'statusCode': 790200, 
    'statusDescription': 'Success.'
}
```

###Full Example 


```python
# import python modules 
import requests
import time
import urllib3
import pprint
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

# Set the request inputs
token = "4f257785-d5f9-42d4-b896-d21f0cb62e6f"
tenantId = "fb24f3f0-81a7-1929-4b8f-99106c23fa5b"
full_url = "http://192.168.28.79/ServicesAPI/API/V1/CMDB/Domains"

# Set proper headers
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

data = {"tenantId": tenantId}

try:
    # Do the HTTP request
    response = requests.get(full_url, params=data, headers=headers, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        result = response.json()
        print (result)
    else:
        print ("Get domains failed! - " + str(response.text))

except Exception as e: print (str(e))
```

    {'domains': [{'domainId': '850ff5e9-c639-404d-85a3-d920dbee509c', 'domainName': 'Support and Service'}, {'domainId': '0201adc4-ae96-46f0-ae3d-01cdba9e41d6', 'domainName': 'GE Test'}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    

cURL Code from Postman


```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Domains?token=c00de805-9210-44a9-9a26-f0c1e944ea36&tenantId=fb24f3f0-81a7-1929-4b8f-99106c23fa5b' \
  -H 'Postman-Token: ee6dda7c-cbcc-43b8-8957-9c4f2d2a4b5b' \
  -H 'cache-control: no-cache'
```

Error Examples


```python
###################################################################################################################    

"""Error 1: empty tenantId"""

Input:
    token = "4f257785-d5f9-42d4-b896-d21f0cb62e6f"
    tenantId = ""
    full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Domains"

Response:
    "{'domains': [], 'statusCode': 790200, 'statusDescription': 'Success.'}"

###################################################################################################################    

"""Error 2: wrong tenantId"""

Input:
    token = "4f257785-d5f9-42d4-b896-d21f0cb62e6f"
    tenantId = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
    full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Domains"

Response:
    """Get domains failed! - 
    {"statusCode":791006,
    "statusDescription":"tenant with id aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa does not exist."}"""
```

## ***GET*** /V1/CMDB/Tenants/
This method returns a list of accessible tenants (including tenant ID and names). The returned tenants list varies by the privileges of different user roles. To retrieve a full list of all available tenants, users must log in with admin role. 

### Detail Information

> **Title** : Get All Asseccible Tenants API<br>

> **Version** : 01/23/2019.

> **API Server URL** : http(s):// < NetBrain Web API Server Address > /ServicesAPI/API/V1/CMDB/Tenants

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|Bearer Authentication| Parameters | Authentication token | 

### Request body(****required***)

>No request body.

### Parameters(****required***)

> No parameters required

### Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| Content-Type | string  | support "application/json" |
| Accept | string  | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| token* | string  | Authentication token, get from login API. |


### Response

|**Name**|**Type**|**Description**|
|------|------|------|
|tenants | array | A list of all accessible tenants.  |
|tenantId| string | The tenant ID.  |
|tenantName| string | The tenant name. |
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example*** :


```python
{
    'tenants': [
        {
            'tenantId': 'fb24f3f0-81a7-1929-4b8f-99106c23fa5b', 
            'tenantName': 'Initial Tenant'
        }
    ], 
    'statusCode': 790200, 
    'statusDescription': 'Success.'
}
```

### Full Example  


```python
# import python modules 
import json
import requests
import time
import urllib3
import pprint
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

token = "828a4561-5ee5-40ac-bf98-01788be48330" 
full_url = "http://192.168.28.79/ServicesAPI/API/V1/CMDB/Tenants"

# Set proper headers
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

try:
    # Do the HTTP request
    response = requests.get(full_url, headers=headers, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        result = response.json()
        print (result)
    else:
        print ("Get tenants failed! - " + str(response.text))
except Exception as e: return (str(e))

```




    {'tenants': [{'tenantId': 'fb24f3f0-81a7-1929-4b8f-99106c23fa5b',
       'tenantName': 'Initial Tenant'}],
     'statusCode': 790200,
     'statusDescription': 'Success.'}



cURL Code from Postman


```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Tenants?token=c00de805-9210-44a9-9a26-f0c1e944ea36' \
  -H 'Postman-Token: b3ecf2c4-d94a-4059-a01f-bcd21fc8a286' \
  -H 'cache-control: no-cache'
```

Error Example : 


```python
###################################################################################################################    

"""Error 1: empty url"""

Input:
    token = "828a4561-5ee5-40ac-bf98-01788be48330" 
    
    full_url = ""  

Response:
    "Invalid URL'': No schema supplied. Perhaps you meant http://"

###################################################################################################################    

"""Error 2: wrong url"""

Input:
    token = "828a4561-5ee5-40ac-bf98-01788be48330" 
    
    full_url = "http://192.1688.28.79/ServicesAPI/API/V1/CMDB/Tenants"  

Response:
    """HTTPConnectionPool(host='192.1688.28.79', port=80): 
       Max retries exceeded with url: /ServicesAPI/API/V1/Session (Caused by NewConnectionError(
           '<urllib3.connection.HTTPConnection object at 0x0000022F203C79B0>: 
           Failed to establish a new connection: [Errno 11001] getaddrinfo failed'))"""
    
###################################################################################################################    

"""Error 3: empty token"""

Input:
    token = "" 
    
    full_url = "http://192.168.28.79/ServicesAPI/API/V1/CMDB/Tenants"  

Response:
    { "statusCode": 795005, "statusDescription": "Invalid token. }
     
###################################################################################################################    

"""Error 4: wrong token"""

Input:
    token = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa" 
    
    full_url = "http://192.168.28.79/ServicesAPI/API/V1/CMDB/Tenants"  

Response:
    { "statusCode": 795005, "statusDescription": "Invalid token. }
```

## ***POST*** /V1/Session

 This method creates an authentication token and starts a session with user's body information and netbrain server url.


### Detail Information

> **Title** : Log in API<br>

> **Version** : 01/23/2019.

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session

> **Authentication** : Not Required.


### Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|username* | string  | the username to log into your NetBrain domain.  |
|password* | string  | the password to log into your NetBrain domain.  |
|authentication_id | string  | This body parameter is only required for an external user through SSO, LDAP/AD or TACACS and the value must same with the name of external authentication which the user created by admin role during system management under "User Account" section. |

> ***Example*** : 


```python
body = {
          "username": "NetBrain",
          "password": "NetBrain"
       }

```

### Parameters(****required***)

>No parameters required.


### Headers

**Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| Content-Type | string  | support "application/json" |
| Accept | string  | support "application/json" |


### Response

|**Name**|**Type**|**Description**|
|------|------|------|
|token | string | The returned authentication token.  |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code. |
 
> ***Example*** :


```python
{
    'token': 'fc6bc6ea-a46a-4e9b-8906-c623f78474b6',
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
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

body = {
    "username" : "Netbrain",      
    "password" : "Netbrain"  
}
    
full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"           

# Set proper headers
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}    

try:
    # Do the HTTP request
    response = requests.post(full_url, headers=headers, data = json.dumps(body), verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Get token failed! - " + str(response.text))
except Exception as e:
    print (str(e))
    
```

    {'token': '9b9715e8-7274-4a28-9692-e00ad315a283', 'statusCode': 790200, 'statusDescription': 'Success.'}
    

Example For External user


```python
body = {
    "username" : "Netbrain",      
    "password" : "Netbrain",
    "authentication_id" : "net-brain" 
}
    
full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"           

# Set proper headers
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}    

try:
    # Do the HTTP request
    response = requests.post(full_url, headers=headers, data = json.dumps(body), verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Get token failed! - " + str(response.text))
except Exception as e:
    print (str(e))
    
```

    {'token': '5e9af6f4-efa8-4a19-9d42-add069c67c99', 'statusCode': 790200, 'statusDescription': 'Success.'}
    

cURL Code from Postman


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/Session \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: ba5d854d-80ec-4a63-be98-65dc92c74a7a' \
  -H 'cache-control: no-cache' \
  -d '{
    "username": "Netbrain",
    "password": "Netbrain"
    }'
```

Error Example : 


```python
###################################################################################################################    

"""Error 1: empty url"""

Input:
    body = {
        "username" : "NetBrain",      
        "password" : "NetBrain"  
    }
    
    full_url = ""  

Response:
    "Invalid URL '': No schema supplied. Perhaps you meant http://?"
    
###################################################################################################################    

"""Error 2: wrong url"""

Input:
    body = {
        "username" : "NetBrain",      
        "password" : "NetBrain"  
    }
    
    full_url = "http://IP address of your NetBrain Web API ServerXXXXXXXXXXX%%%%%%%%/ServicesAPI/API/V1/Session"  

Response:
    """HTTPConnectionPool(host='192.1688.28.79', port=80): 
       Max retries exceeded with url: /ServicesAPI/API/V1/Session (Caused by NewConnectionError(
           '<urllib3.connection.HTTPConnection object at 0x0000022F203C79B0>: 
           Failed to establish a new connection: [Errno 11001] getaddrinfo failed'))"""
    
################################################################################################################### 

"""Error 3: empty body"""

Input:
    body = {
        "username" : "",      
        "password" : ""  
    }
    
    full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"  

Response:
    "Get token failed! - {"statusCode":795000,"statusDescription":"Invalid username or password."}"
    
################################################################################################################### 

"""Error 4: wrong body information"""

Input:
    body = {
        "username" : "wwwwwww",      
        "password" : "wwwwwww"  
    }
    
    full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"  

Response:
    "Get token failed! - {"statusCode":795000,"statusDescription":"Invalid username or password."}"
    
################################################################################################################### 

"""Error 4: for external user, empty authentication id"""

Input:
    body = {
        "username" : "Netbrain",      
        "password" : "Netbrain",
        "authentication_id" : ""
    }
    
    full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"  

Response:
    {
        "statusCode": 795000,
        "statusDescription": "Invalid username or password."
    }
    
################################################################################################################### 

"""Error 4: for external user, wrong authentication id"""

Input:
    body = {
        "username" : "Netbrain",      
        "password" : "Netbrain",
        "authentication_id" : "XXXXXXXXX"
    }
    
    full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"  

Response:
    {
        "statusCode": 795000,
        "statusDescription": "Invalid username or password."
    }
```


## ***DELETE*** /V1/Session
Use this API to log out from the current session.

### Detail Information

> **Title** : Log out API<br>

> **Version** : 01/23/2019.

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server /ServicesAPI/API/V1/Session

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|Bearer Authentication| Headers | Authentication token | 

### Request body(****required***)

>No request body.

### Parameters(****required***)

> No parameters required.

### Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| Content-Type | string  | support "application/json" |
| Accept | string  | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| token | string  | Authentication token, get from login API. |

### Response

|**Name**|**Type**|**Description**|
|------|------|------|
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code. |

> ***Example*** :


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
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

# Set the request inputs
token = "a63c6610-1a44-4907-bb57-784179d30ba3"
full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"
    
# Set proper headers
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["token"] = token

try:
    # Do the HTTP request
    response = requests.delete(full_url, headers=headers, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Session logout failed! - " + str(response.text))

except Exception as e:
    print (str(e))
    

```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

cURL Code from Postman


```python
curl -X DELETE \
  http://192.168.28.79/ServicesAPI/API/V1/Session \
  -H 'Postman-Token: d6de8cb3-ca3b-4bde-b9c7-be800e902d2c' \
  -H 'cache-control: no-cache' \
  -H 'token: 7480e46f-6a25-470e-9c61-351f6b7d86fa'
```

Error Exampes


```python
###################################################################################################################    

"""Error 1: empty url"""

Input:
    token = "a63c6610-1a44-4907-bb57-784179d30ba3"
    
    full_url = ""  

Response:
    "Invalid URL '': No schema supplied. Perhaps you meant http://?"
    
###################################################################################################################    

"""Error 2: wrong url"""

Input:
    token = "a63c6610-1a44-4907-bb57-784179d30ba3"
    
    full_url = "http://IP address of your NetBrain Web API Serveraaaaaaaaaaaaaa/ServicesAPI/API/V1/Session"  

Response:
    """HTTPConnectionPool(host='192.1688.28.79', port=80): 
       Max retries exceeded with url: /ServicesAPI/API/V1/Session (Caused by NewConnectionError(
           '<urllib3.connection.HTTPConnection object at 0x0000022F203C79B0>: 
           Failed to establish a new connection: [Errno 11001] getaddrinfo failed'))"""
    
################################################################################################################### 

"""Error 3: empty token"""

Input:
    token = "" 
    
    full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"  

Response:
    { "statusCode": 795005, "statusDescription": "Invalid token. }
     
###################################################################################################################    

"""Error 4: wrong token"""

Input:
    token = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa" 
    
    full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"  

Response:
    { "statusCode": 795005, "statusDescription": "Invalid token. }
```

## ***PUT*** /V1/Session/CurrentDomain
Use this API to specify a domain to work on to get or set NetBrain data by associating domainID to the current session. 

### Detail Information

> **Title** : Specify a domain to work on API<br>

> **Version** : 01/23/2019.

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server /ServicesAPI/API/V1/Session/CurrentDomain

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|Bearer Authentication| Parameters | Authentication token | 

### Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|tenantId* | string  | Unique identifier for the tenant from which you desire to retrieve the domain information. tenantId can be retrieved from get all accessible tenants.  |
|domainId | string  | Input the ID of the target domain. Get a domain ID by using the API [Get all accessible domains of a tenant.](https://www.netbraintech.com/docs/ie71/help/get-all-accessible-domains-of-tenant.htm)<br> **Note**: This parameter is optioanl if the following operations aim only on tenant. |

> ***Example***


```python
{
    "tenantId": "fb24f3f0-81a7-1929-4b8f-99106c23fa5b",
    "domainId": "0201adc4-ae96-46f0-ae3d-01cdba9e41d6"
}
```

### Parameters(****required***)

> No parameters required.

### Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| Content-Type | string  | support "application/json" |
| Accept | string  | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| token | string  | Authentication token, get from login API. |

### Response

|**Name**|**Type**|**Description**|
|------|------|------|
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

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
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

# Set the request inputs
token = "4f257785-d5f9-42d4-b896-d21f0cb62e6f"
full_url = "http://192.168.28.79/ServicesAPI/API/V1/Session/CurrentDomain"
tenantId = "fb24f3f0-81a7-1929-4b8f-99106c23fa5b"
domainId = "0201adc4-ae96-46f0-ae3d-01cdba9e41d6"
    
# Set proper headers
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

body = {
        "tenantId": tenantId,
        "domainId": domainId
    }

try:
    # Do the HTTP request
    response = requests.put(full_url, data=json.dumps(body), headers=headers, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        result = response.json()
        print (result)
    elif response.status_code != 200:
        print ("Login failed! - " + str(response.text))

except Exception as e: print (str(e))

```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

cURL Code from Postman


```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Domains?token=c00de805-9210-44a9-9a26-f0c1e944ea36&tenantId=fb24f3f0-81a7-1929-4b8f-99106c23fa5b' \
  -H 'Postman-Token: ee6dda7c-cbcc-43b8-8957-9c4f2d2a4b5b' \
  -H 'cache-control: no-cache'
```

Error Examples:


```python
###################################################################################################################    

"""Error 1: empty tenantId and domainId"""

Input:
    token = "4f257785-d5f9-42d4-b896-d21f0cb62e6f"
    full_url = "http://192.168.28.79/ServicesAPI/API/V1/Session/CurrentDomain"
    tenantId = ""
    domainId = ""

Response:
    "{
        "statusCode": 791000,
        "statusDescription": "Null parameter: the parameter 'tenantId' cannot be null."
     }"

###################################################################################################################    

"""Error 1: wrong tenantId"""

Input:
    token = "4f257785-d5f9-42d4-b896-d21f0cb62e6f"
    full_url = "http://192.168.28.79/ServicesAPI/API/V1/Session/CurrentDomain"
    tenantId = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
    domainId = "0201adc4-ae96-46f0-ae3d-01cdba9e41d6"

Response:
    "{
        "statusCode": 791004,
        "statusDescription": "Invalid tenant id."
    }"
```