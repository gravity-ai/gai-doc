# Overview
In order to submit on-demand [jobs](./glossary.md#Jobs), you will need to interact with the job-api.  
For your convenience, there's a swagger doc that you can find <a href="https://on-demand.gravity-ai.com/swagger/index.html" target="_blank"><u>here</u></a>.
<br/>
<br/>
<hr/>
Note: You must provide your api key prior to making these calls. See [<u>"Submitting On-Demand Jobs"</u>](submittingJobs.md) for more details.
<hr/>
<hr/>
Note: This only applies to *Hosted* on-demand models. This documentation has nothing to do with running tests or jobs on a docker container.
<hr/>

# API
## Getting the latest job details
### Request
Perform this call in order to fetch the details pertaining to the latest jobs run

```
GET: /api/v1/jobs/latest
```

### Response

```json
{
  "data": [
    {
      "name": "JobNameExample",
      "inputFileName": "example.csv",
      "inputMime": "text/csv; header=present",
      "billedUsage": 20,
      "recordCount": 3,
      "recordGroupCount": 1,
      "processingTimeMS": 1424,
      "status": "success",
      "errorMessage": null,
      "id": "59015703-de7f-4e92-9451-6b08210fbb16",
      "createdDateUtc": "2023-02-21T21:41:30.405Z",
      "lastUpdatedUtc": "2023-02-22T00:12:51.539Z"
    },
    ...
    ,
    {
      "name": null,
      "inputFileName": "example.csv",
      "inputMime": "text/csv; header=present",
      "billedUsage": 20,
      "recordCount": 3,
      "recordGroupCount": 1,
      "processingTimeMS": 6219,
      "status": "success",
      "errorMessage": null,
      "id": "f1ad4cda-0588-4b96-abc2-1d61af098305",
      "createdDateUtc": "2023-02-21T21:37:22.036Z",
      "lastUpdatedUtc": "2023-02-21T21:37:22.037Z"
    }
  ],
  "isError": false,
  "errorMessage": null
}
```

<ul>
<li><strong>name -- </strong>Name of the job. This will default to input file name.</li>
<li><strong>inputFileName -- </strong>Input file name.  Just in case you rename the job.</li>
<li><strong>inputMime -- </strong>The mimetype of the input data.</li>
<li><strong>billedUsage -- </strong>How much this particular job will cost. <br/> Each unit will be $0.001. So billedUsage of 20 would be 20 * $0.001 = $0.02.  Note: billedUsage is only updated during job creation...it is not impacted by GETs and PUT api calls.</li>
<li><strong>recordCount -- </strong>The amount of records in this particular job. <br/> For structured data, this might amount to the number of rows in your request.  For unstructured data, e.g. images, records would likely be 1 per job.</li>
<li><strong>recordGroupCount -- </strong>Records are grouped together prior to a price being assigned.  This field dictates the number of groupings that were processed.  E.g. say if the model be charged per group of 5 records, and your dataset containers 10 records.  This value would be 2, as there are two groupings of 5.  </li>
<li><strong>processingTimeMS -- </strong>The amount of time it took to process yoru job</li>
<li><strong>status -- </strong>Status of the job request</li>
<li><strong>errorMessage -- </strong>Error message, if applicable. null if no error</li>
<li><strong>id -- </strong>This is the jobId you will need for your subsequent job api calls.</li>
<li><strong>createdDateUtc -- </strong>The datetime the job was created</li>
<li><strong>lastUpdatedUtc -- </strong>The datetime the job was modified</li>
</ul>

## Getting a particular job's details
### Request
Perform this call in order to fetch the details pertaining to a particular job.
<br/>
As one can see, the <strong><u>jobId</u></strong> must be supplied.

```
GET: /api/v1/jobs/{jobId}
```

### Response
```json
	
Response body
Download
{
  "data": {
    "name": "JobNameExample",
    "inputFileName": "example.csv",
    "inputMime": "text/csv; header=present",
    "billedUsage": 20,
    "recordCount": 3,
    "recordGroupCount": 1,
    "processingTimeMS": 1424,
    "status": "success",
    "errorMessage": null,
    "id": "59015703-de7f-4e92-9451-6b08210fbb16",
    "createdDateUtc": "2023-02-21T21:41:30.405Z",
    "lastUpdatedUtc": "2023-02-22T00:12:51.539Z"
  },
  "isError": false,
  "errorMessage": null
}
```

## Renaming a particular job
### Request
Perform this call in order to rename a particular job.  Utilizing this call might help you find the relevant job a bit easier in the UI.
<br/>
As one can see, the <strong><u>jobId</u></strong> must be supplied.

```
PUT: /api/v1/jobs/{jobId}
```

Request Body
```json
{
  "name": "JobNameExample"
}
```

<ul>
<li><strong>name -- </strong>What to rename a particular job.  By default, the job name will be the input file name.</li>
</ul>

### Response

```json
{
  "data": {
    "name": "JobNameExample",
    "inputFileName": "example.csv",
    "inputMime": "text/csv; header=present",
    "billedUsage": 20,
    "recordCount": 3,
    "recordGroupCount": 1,
    "processingTimeMS": 1424,
    "status": "success",
    "errorMessage": null,
    "id": "59015703-de7f-4e92-9451-6b08210fbb16",
    "createdDateUtc": "2023-02-21T21:41:30.405Z",
    "lastUpdatedUtc": "2023-02-22T00:12:51.539Z"
  },
  "isError": false,
  "errorMessage": null
}
```
<ul>
<li><strong>name -- </strong>Name of the job. This will default to input file name.</li>
<li><strong>inputFileName -- </strong>Input file name.  Just in case you rename the job.</li>
<li><strong>inputMime -- </strong>The mimetype of the input data.</li>
<li><strong>billedUsage -- </strong>How much this particular job will cost. <br/> Each unit will be $0.001. So billedUsage of 20 would be 20 * $0.001 = $0.02.  Note: billedUsage is only updated during job creation...it is not impacted by GETs and PUT api calls.</li>
<li><strong>recordCount -- </strong>The amount of records in this particular job. <br/> For structured data, this might amount to the number of rows in your request.  For unstructured data, e.g. images, records would likely be 1 per job.</li>
<li><strong>recordGroupCount -- </strong>Records are grouped together prior to a price being assigned.  This field dictates the number of groupings that were processed.  E.g. say if the model be charged per group of 5 records, and your dataset containers 10 records.  This value would be 2, as there are two groupings of 5.  </li>
<li><strong>processingTimeMS -- </strong>The amount of time it took to process yoru job</li>
<li><strong>status -- </strong>Status of the job request</li>
<li><strong>errorMessage -- </strong>Error message, if applicable. null if no error</li>
<li><strong>id -- </strong>This is the jobId you will need for your subsequent job api calls.</li>
<li><strong>createdDateUtc -- </strong>The datetime the job was created</li>
<li><strong>lastUpdatedUtc -- </strong>The datetime the job was modified</li>
</ul>


## Getting the output file from a particlar job
### Request
Perform this call when you need to download the results from a particular job.  
As one can see, the <strong><u>jobId</u></strong> must be supplied.

```
GET: /api/v1/jobs/result-link/{jobId}
```

### Response
Example Response:

```json
{
  "data": "https://gai-user-data-staging.s3.amazonaws.com/staging/on_demand/3c70b31c5c644ed4885b7940241d07d9/f25b7826e70443598e0dd36da3f7b8d3/59015703de7f4e9294516b08210fbb16/output/data_59015703.csv?AWSAccessKeyId=ASIA32VAOF4IHNURYV7H&Expires=1677024795&x-amz-security-token=IQoJb3JpZ2luX2VjECwaCXVzLWVhc3QtMSJGMEQCIDzKFJ4vk9AJ%2FeIs%2B7I2D4PYqeMaFBgGswlgp2kpXTNGAiB6DANU8ChYJ5aBrVVHNVPdaussVxUU597h8I0%2Bfb%2BADyr8AwjE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAMaDDgxMzE1OTAzNDY0MCIMlBrYkNqwobStynhmKtADQ2IzWGJzNuk7m1jBi%2BUu4gfXmSRQwzu7GiWdxmR7mEAlX735MOE4we0BKoAkJWI0YT7dytwHkA8c%2Fc%2B98zcKgyz0Is8FoQS6sZNgTT8tRFQRBYOqXk1eCLuoQBFr7gHi%2B8YYcJo3dI%2FXKzDYRtgj4Kph0cZRfLwGktItQ0d1Qlk0qosfVA54%2F22wDxS9Z9YmeUhFyO12A04qQIHvTx0ytn%2FWtIV5lJEWn%2BvlmtTwWm1HwTsd%2BWtJBBkRvPV3fGsrm1HqxoQi829donuJHyXcdAslGXUyA%2BVpGY47%2FI9Ut7qoID1jdqHaEol9jpQFSDQ7LO%2FhbmbKDnbekwCmDhEzdx%2FGekI85hQc3nD8HkbVui6K1EfteTih1Fhjek4bf4rKYDbiJvAZi5ByUg35c8FO7EzURn3vQXkOlgCMYNoB%2Bnh3aW5PbyYjYwoDFY7lbWaGBRTAMT0GeCnZs1wn0C1y%2FU3NH0YDTRsx%2B1xO3psnVMb1hsLYCO20dwlZKf0WpHLniWaYfCoy6tQhy5mDjz%2FMEVhEqEVs6753gXdFOFZwo7cJoN80Pj65AnvUjcREHo11s6s8bZ9omG0CI6G%2FV%2FQoBfxm1MixX%2B5X8NspW5PMcBIw4qvUnwY6pgEVCc9YHIgM%2BVrdb73459ijWwuzNWiC6%2FitTL8QyoOOIh%2BkNMv6AV8y6ZnZDdWibVJkzPs%2Fz1IZIrEQbGQ%2B4DfYFlvQTxdrXqZ4PPrVvtw3ZR4Hm%2FBkMcd21KMw%2Bjtu%2FQO%2FapWr18xiGYqFnB1rV6fJFeBMG7ucQJ%2BUx%2FpMICnPmIIlKnJhzSyk%2BtsPmUi6GvU5ofkQ%2FR6XMlCEu1t1CLbtdPBjtkzS&Signature=%2FxRGvAbpsIZUKAJ8RZK%2FqYAqhC8%3D",
  "isError": false,
  "errorMessage": null
}
```

<ul>
<li>data -- If you want to download the resulting output file, you can copy the url provided here an paste it in a browser. The resulting output file should be downloaded.</li>
</ul>

## Submitting a job
### Request
Perform this call when you need to submit a job to the jobs api

```
POST: /api/v1/jobs
```

In order to submit a job, you must submit both a <strong><u>data</u></strong> object and <strong><u>file</u></strong>.

<strong><u>data</u></strong>:
```json
{
  "version": "string",
  "mimeType": "string",
  "name": "string",
  "mapping": [
    {
      "source": "string",
      "destination": "string",
      "defaultValue": "string"
    }
  ],
  "outputMapping": [
    {
      "source": "string",
      "destination": "string",
      "defaultValue": "string"
    }
  ]
}
```

<ul>
<li><strong>version</strong>:<em>[optional]</em> -- Project version number. Ex: "0.0.1". This can be null, in which case the most recent active version will be used</li>
<li><strong>mimeType</strong>:<em>[required]</em> -- Mime-Type for input data. 
    <ul>
        <li>Ex: "application/json"</li> 
        <li>Ex: "text/csv; header=present"</li> 
        <li>etc.</li>
    </ul>
</li>
<li><strong>name</strong>:<em>[optional]</em> -- By default, when looking at the list of jobs in the UI, it will display the input file name.  If you want it to be something else, you can set the job name here.</li>
<li><strong>mapping</strong>:<em>[optional]</em> -- This is an array that governs input mapping.  E.g. if your model accepts fields "x" and "y", and you only have a dataset with headers "x1" and "y1", you can may x1=>x and y1=>y here.
<ul>
<li>source: -- the column/feature from structured data you want to map from</li>
<li>destination: -- the column/feature from structured data you want to map to</li>
<li>defaultValue: -- a default value (must be of same datatype)</li>
</ul>
</li>
<li><strong>outputMapping</strong>:<em>[optional]</em> -- This is an array that governs output mapping. E.g. if your model generates output "a", "b", "c" but you want the resulting output file to be formatted "Alpha", "Bravo", "Charlie", you can map "a"=>"Alpha", "b"=>"Bravo", "c"=>"Charlie" here  
<ul>
<li>source: -- the column/feature from structured data you want to map from</li>
<li>destination: -- the column/feature from structured data you want to map to</li>
<li>defaultValue: -- a default value (must be of same datatype)</li>
</ul>
</li>
</ul>

<strong><u>file</u></strong>:
<br/>
The relevant input file you want to submit your job.  In swagger's UI, you can easily select your test file.  Likewise, you might be able to use <a href="https://www.postman.com/"><u>Postman</u></a> if you want to submit your job easily.  If you want to submit a request to the API via code, you will need to take into account a multipart/form-data POST and how to submit a file.  See the example python code that we have supplied if you're curious how this might work.  

### Response
Example Response:
```json
{
  "data": {
    "name": "example.csv",
    "inputFileName": "example.csv",
    "inputMime": "text/csv; header=present",
    "billedUsage": 20,
    "recordCount": 3,
    "recordGroupCount": 1,
    "processingTimeMS": 1424,
    "status": "success",
    "errorMessage": null,
    "id": "59015703-de7f-4e92-9451-6b08210fbb16",
    "createdDateUtc": "2023-02-21T21:41:30.4058498Z",
    "lastUpdatedUtc": "2023-02-21T21:41:30.4058498Z"
  },
  "isError": false,
  "errorMessage": null
}
```
<ul>
<li><strong>name -- </strong>Name of the job. This will default to input file name.</li>
<li><strong>inputFileName -- </strong>Input file name.  Just in case you rename the job.</li>
<li><strong>inputMime -- </strong>The mimetype of the input data.</li>
<li><strong>billedUsage -- </strong>How much this particular job will cost. <br/> Each unit will be $0.001. So billedUsage of 20 would be 20 * $0.001 = $0.02</li>
<li><strong>recordCount -- </strong>The amount of records in this particular job. <br/> For structured data, this might amount to the number of rows in your request.  For unstructured data, e.g. images, records would likely be 1 per job.</li>
<li><strong>recordGroupCount -- </strong>Records are grouped together prior to a price being assigned.  This field dictates the number of groupings that were processed.  E.g. say if the model be charged per group of 5 records, and your dataset containers 10 records.  This value would be 2, as there are two groupings of 5.  </li>
<li><strong>processingTimeMS -- </strong>The amount of time it took to process yoru job</li>
<li><strong>status -- </strong>Status of the job request</li>
<li><strong>errorMessage -- </strong>Error message, if applicable. null if no error</li>
<li><strong>id -- </strong>This is the jobId you will need for your subsequent job api calls.</li>
<li><strong>createdDateUtc -- </strong>The datetime the job was created</li>
<li><strong>lastUpdatedUtc -- </strong>The datetime the job was modified</li>
</ul>