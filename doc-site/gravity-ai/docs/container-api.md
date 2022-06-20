# gravityAI Container API
 
Models containerized by gravityAI are bundled with a REST API that allows developers to easily integrate them into their applications. 
 
Using the container API developers can:

* Retrieve mime types supported by the container
* Add new jobs to be processed
* Check on the status submitted jobs
* Delete submitted jobs
* Retrieve the output of completed jobs
* Inspect the health of the container
* Retrieve the license key submitted to the container
* Upload new license keys for the container
 
 
The example API calls in the documentation use the Textblob Sentiment Analysis container run at URL http://localhost:7000

<hr/>

# Container Health
 
Clients can retrieve the health of the container by making a GET request to the /health endpoint
 
### Curl
`curl -X GET "http://localhost:7000/health" -H "accept: */*"`
 
### Python
```python
>> import requests
>> response = requests.get("http://localhost:7000/health")
>> response.text
```
 
### Output
Health check is ok
 
### Parameters:
No parameters
 
### Returns
Health status as plain text

<hr/>

# Supported Mime Types
 
Clients can retrieve the mime types supported by the container by making a GET request to the /data/supported-mime-types
 
### Curl
`curl -X GET "http://localhost:7000/data/supported-mime-types" -H "accept: */*"`
 
### Python
```python
>> import requests
>> response = requests.get("http://localhost:7000/health")
>> response.json()
```
 
### Output
```json
{
  "inputTypes": [],
  "outputTypes": [
    "application/json",
    "text/json",
    "text/csv"
  ]
}
```
 
### Parameters
No parameters
 
### Returns
A JSON object containing the supported input and output mime types

<hr/>

# Job Status
 
Clients can retrieve the status of submitted jobs by making a GET request to the /data/status/{id} end point
 
### Curl
`curl -X GET "http://localhost:7000/data/status/313ee043-3a95-48b7-a9b3-076f5a5b1df0" -H "accept: */*"`
 
### Python
```python
>> import requests
>> response = requests.get("http://localhost:7000/data/status/313ee043-3a95-48b7-a9b3-076f5a5b1df0")
>> response.json()
```
 
### Output
```json
{
  "data": {
    "id": "313ee043-3a95-48b7-a9b3-076f5a5b1df0",
    "status": "Complete",
    "errorMessage": null
  },
  "isError": false,
  "errorMessage": null
}
```

### Parameters
| Name      | Description                 |
| --------- | --------------------------- |
| `id`      | The ID of the submitted job |

<br/>
 
### Returns
A JSON object containing the status of the submitted job and any error messages that occurred.

<hr/>

# Job Result
 
## Results as Data File
 
Clients can retrieve the results of submitted jobs by making a GET request to the /data/result/{id} end point
 
### Curl
`curl -X GET "http://localhost:7000/data/result/313ee043-3a95-48b7-a9b3-076f5a5b1df0" -H "accept: */*"`
 
### Python
```python
>> import requests
>> response = requests.get("http://localhost:7000/data/result/313ee043-3a95-48b7-a9b3-076f5a5b1df0")
```
 
### Output
```json
{"polarity": -0.6, "subjectivity": 0.9}
```

### Parameters
| Name      | Description                 |
| --------- | --------------------------- |
| `id`      | The ID of the submitted job |

<br/>
 
### Returns
The results of the submitted job
 
## Results as JSON
 
Clients can retrieve a JSON object containing result of submitted jobs by making a GET request to the /data/result/json/{id} end point
 
### Curl
`curl -X GET "http://localhost:7000/data/result/json/313ee043-3a95-48b7-a9b3-076f5a5b1df0" -H "accept: */*"`
 
### Python
```python
>> import requests
>> response = requests.get("http://localhost:7000/data/result/json/313ee043-3a95-48b7-a9b3-076f5a5b1df0")
>> response.json()
```
 
### Output
```json
{
  "polarity": -0.6,
  "subjectivity": 0.9
}
```
 
### Parameters
| Name      | Description                 |
| --------- | --------------------------- |
| `id`      | The ID of the submitted job |

<br/>

### Returns
A JSON object containing the results of the submitted job
 
## Results as CSV
 
Clients can retrieve a CSV file containing result of submitted jobs by making a GET request to the /data/result/json/{id} end point
 
### Curl
`curl -X GET "http://localhost:7000/data/result/json/313ee043-3a95-48b7-a9b3-076f5a5b1df0" -H "accept: */*"`
 
### Python
```python
>> import requests
>> response = requests.get("http://localhost:7000/data/result/json/313ee043-3a95-48b7-a9b3-076f5a5b1df0")
>> response.text()
```
 
### Output
polarity,subjectivity
-0.6,0.9
 
### Parameters
| Name      | Description                 |
| --------- | --------------------------- |
| `id`      | The ID of the submitted job |
 
 <br/>

### Returns
A CSV file containing the results of the submitted job

<hr/> 

# Delete Job
 
Clients can delete jobs by making a DELETE request to the /data/delete/{id} end point
 
### Curl
`curl -X DELETE "http://localhost:7000/data/delete/313ee043-3a95-48b7-a9b3-076f5a5b1df0" -H "accept: */*"`
 
### Python
```python
>> import requests
>> response = requests.delete("http://localhost:7000/data/delete/313ee043-3a95-48b7-a9b3-076f5a5b1df0")
>> response.json()
```
 
### Output
```json
{
  "data": {
    "id": "313ee043-3a95-48b7-a9b3-076f5a5b1df0",
    "status": "Deleted",
    "errorMessage": null
  },
  "isError": false,
  "errorMessage": null
}
```
 
### Parameters
| Name      | Description                 |
| --------- | --------------------------- |
| `id`      | The ID of the submitted job |

<br/>

### Returns
A JSON object containing the results of the operation

<hr/>

# Add Job
 
Clients can submit jobs by making a POST request to the /data/add-job end point
 
### Curl
`curl -X POST "http://localhost:7000/data/add-job" -H "accept: */*" -H "Content-Type: multipart/form-data" -F "Mappings=" -F "CallbackUrl=" -F "MimeType=" -F "File=@input.txt;type=text/plain"`
 
### Python
>> import requests
>> test_file = {'file': ('input.txt', '{"text": "This is so awesome!"}')}
>> response = requests.post("http://localhost:7000/data/add-job",data = {'MimeType': 'text/plain'}, files=test_file)
>> response.json()
 
### Output
```json
{
  "data": {
    "id": "837ef1a2-7342-48a1-8c69-b8eb09e0ff64",
    "status": "Created",
    "errorMessage": null
  },
  "isError": false,
  "errorMessage": null
}
```
 
### Parameters
| Name          | Description         |
| ------------- | ------------------- |
| `Mappings`    | schema mappings     |
| `CallbackUrl` | A callback url      |
| `MimeType`    | Data file mime type |
| `File`        | Binary file         |

<br/>

### Returns
A JSON object containing the results of operation

<hr/>

# Get License
 
Clients can retrieve the current api license by making a GET request to the /api/license end point
 
### Curl
`curl -X GET "http://localhost:7000/api/license" -H "accept: */*"`
 
### Python
```python
>> import requests
>> response = requests.get("http://localhost:7000/api/license")
>> response.json()
```
 
### Output
```json
{
  "data": "Licensed To: Developer - developer@test.com, Expires:Sunday, 12 June 2022 23:43:34 (UTC)",
  "isError": false,
  "errorMessage": null
}
```
 
### Parameters
No parameters
 
### Returns
A JSON object containing the license data

<hr/>

# Upload License
 
Clients can upload an api license by making a POST request to the /api/license end point
 
### Curl
`curl -X POST "http://localhost:7000/api/license" -H "accept: */*" -H "Content-Type: application/json-patch+json" -d "{ \"license\": \"<License Key Text Here>"}"`
### Python
```python
>> import requests
>> license_data = {"license": "<License Key Text Here>"}
>> response = requests.post("http://localhost:7000/api/license", json=license_data)
>> response.json()
```
 
### Output
```json
{
  "isError": false,
  "errorMessage": null
}
```
 
### Parameters
| Name         | Description                                |
| ------------ | ------------------------------------------ |
| `license`    | A JSON file containing the license key     |

<br/>
 
### Returns
A JSON object containing the license data
