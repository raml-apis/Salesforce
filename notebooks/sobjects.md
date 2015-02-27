---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/8111/versions/8305/portal/pages/6998/preview
apiNotebookVersion: 1.1.66
title: SObjects
---

```javascript

load('https://github.com/chaijs/chai/releases/download/1.9.0/chai.js')

```



See http://chaijs.com/guide/styles/ for assertion styles



```javascript

assert = chai.assert

```

```javascript

CLIENT_ID = prompt("Please, enter clientId of your SalesForce application.")

CLIENT_SECRET = prompt("Please, enter clientSecret of your SalesForce application.")

```

```javascript

// Read about the SalesForceRAML API at https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/8111/versions/8305/contracts

API.createClient('initializationClient', '/apiplatform/repository/public/organizations/30/apis/8111/versions/8305/definition',{baseUriParameters: {

  domain: 'na1.salesforce.com'

}});

```

```javascript

API.authenticate(initializationClient,"oauth_2_0",{

  clientId: CLIENT_ID,

  clientSecret: CLIENT_SECRET

})

```

```javascript

versions = initializationClient("/").get()

```



Testing sales force api version.





```javascript

VERSION = "v" + versions.body[versions.body.length-1].version

```



Domain for connection, returned after authorization.



```javascript

DOMAIN = $4.response.instance_url.substr(8)

```

```javascript

// Read about the SalesForceRAML API at https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/8111/versions/8305/contracts

API.createClient('client', '/apiplatform/repository/public/organizations/30/apis/8111/versions/8305/definition',{baseUriParameters: {

  domain: DOMAIN

}});

```

```javascript

API.authenticate(client,"oauth_2_0",{

  clientId: CLIENT_ID,

  clientSecret: CLIENT_SECRET

})

```

```javascript

function getShiftDate(monthShift){

  var today = new Date();

  var dd = today.getDate();

  var mm = today.getMonth()+1+monthShift; //January is 0!

  var hh = today.getHours();

  var min = today.getMinutes();

  var sec = today.getSeconds();

  

  var yearShift = 0;

  if(mm>12){

    while (mm>12){

    	mm = mm - 12;

      yearShift = yearShift + 1;

    }

  }

  if(mm<1){

    while (mm<1){

    	mm = mm + 12;

      yearShift = yearShift - 1;

    }

  }

  var yyyy = today.getFullYear()+yearShift;

  if(dd<10){dd='0'+dd}

  if(mm<10){mm='0'+mm}

  if(hh<10){hh='0'+hh}

  if(min<10){min='0'+min}

  if(sec<10){sec='0'+sec}

  today = yyyy+'-'+mm+'-'+dd+'T'+hh+":"+min+":"+sec+"+00:00";

  return today;

}

```



Object type operated by the notebook



```javascript

OBJECT_NAME_VALUE = "Account"

```



Lists available resources for the specified API version, including resource name and URI



```javascript

versionResponse = client.version(VERSION).get()

```

```javascript

assert.equal( versionResponse.status, 200 )

```



Executes the specified SQL query.



```javascript

queryResponse = client.version(VERSION).query.get( {

  "q": "SELECT Name FROM Account"

})

```

```javascript

assert.equal( queryResponse.status, 200 )

queryIdentifier = queryResponse.body.nextRecordsUrl

```



If the query results are too large, the response contains the first batch of results and a query identifier in the nextRecordsUrl

field of the response. The identifier can be used in an additional request to retrieve the next batch.



```javascript

if(queryIdentifier){

  queryIdentifierResponse = client.version(VERSION).query.queryIdentifier(queryIdentifier).get()

}

```

```javascript

if(queryIdentifier){

  assert.equal( queryIdentifierResponse.status, 200 )

}

```



Gets the most recently accessed items that were viewed or referenced by the current user.



```javascript

recentlyItemsResponse = client.version(VERSION).recent.get()

```

```javascript

assert.equal( recentlyItemsResponse.status, 200 )

```



Get a List of Object



```javascript

objectsResponse = client.version(VERSION).sobjects.get()

```

```javascript

assert.equal( objectsResponse.status, 200 )

```







Describes the individual metadata for the specified object.





```javascript

objectMetadataResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).get()

```

```javascript

assert.equal( objectMetadataResponse.status, 200 )

```



Creating a new record



```javascript

objectNameCreateResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).post({

  "Name" : OBJECT_NAME_VALUE + " " + new Date().getMilliseconds()

})

```

```javascript

assert.equal( objectNameCreateResponse.status, 201 )

ID_VALUE = objectNameCreateResponse.body.id

```



Retrieves the list of individual records that have been deleted within the given timespan for the specified object.



```javascript

deletedRecordResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).deleted.get( {

  "start": getShiftDate(0),

  "end": getShiftDate(1)

})

```

```javascript

assert.equal( deletedRecordResponse.status, 200 )

```



Accesses records based on the specified object ID



```javascript

objectRecordResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).id(ID_VALUE).get()

```

```javascript

assert.equal( objectRecordResponse.status, 200 )

```



Update records based on the specified object ID



```javascript

//Not supported

// patchResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).id(ID_VALUE).patch(  {

//   "Name" : "NewName"

// })

```

```javascript

//assert.equal( patchResponse.status, 200 )

```



Retreive Id of some document



```javascript

documentsResponse = client.version(VERSION).sobjects.SObjectName("Document").get()

documentId = documentsResponse.body.recentItems[0].Id

```



Retrieves the specified blob field from an individual record.



```javascript

blobFieldResponse = client.version(VERSION).sobjects.SObjectName("Document").id(documentId).blobField("Body").get()

```

```javascript

assert.equal(blobFieldResponse.status, 200)

```



Delete records.



```javascript

objectDeleteResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).id(ID_VALUE).delete()

```

```javascript

assert.equal(objectDeleteResponse.status, 204)

```



Create one more object



```javascript

objectNameCreateResponse2 = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).post({

  "Name" : OBJECT_NAME_VALUE + " " + new Date().getMilliseconds()

})

ID_VALUE = objectNameCreateResponse2.body.id

```



Returns a existing records (upserts records) based on the value of a specified external ID field.



```javascript

externalFieldValueResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).fieldName("Id").fieldValue(ID_VALUE).get()

```

```javascript

assert.equal(externalFieldValueResponse.status, 200)

```



Returns a existing records (upserts records) based on the value of a specified external ID field. (HEAD)



```javascript

externalFieldValueHeadResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).fieldName("Id").fieldValue(ID_VALUE).head()

```

```javascript

assert.equal( externalFieldValueHeadResponse.status, 200 )

```



Creates new records or updates existing records (upserts records) based on the value of a specified external ID field.



```javascript

//Not supported

// fieldNameFieldValuePatchResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).fieldName("Id").fieldValue(ID_VALUE).patch({

//   "Name" : "NewAcc"

//   }

// )

```

```javascript

//assert.equal( fieldNameFieldValuePatchResponse.status, 200 )

```



Delete a existing records (upserts records) based on the value of a specified external ID field.



```javascript

externalFieldValueDeleteResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).fieldName("Id").fieldValue(ID_VALUE).delete()

```

```javascript

assert.equal( externalFieldValueDeleteResponse.status, 204 )

```



Retrieves the list of individual records that have been updated (added or changed) within the given timespan for the specified object.



```javascript

updatedRecordsResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).updated.get( {

  "start": getShiftDate(0),

  "end": getShiftDate(1)

})

```

```javascript

assert.equal( updatedRecordsResponse.status, 200 )

```



Completely describes the individual metadata at all levels for the specified object.



```javascript

metadataDescribeResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).describe.get()

```

```javascript

assert.equal( metadataDescribeResponse.status, 200 )

```



Retrieves information about alternate named layouts for a given object.



```javascript

layoutsDescribeResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).describe.layouts.get()

```

```javascript

assert.equal( layoutsDescribeResponse.status, 200 )

```



Retrieves information about alternate named layouts for a given object. (HEAD)



```javascript

layoutsHeadResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).describe.layouts.head()

```

```javascript

assert.equal( layoutsHeadResponse.status, 200 )

```



Returns a list of compact layouts for a specific object. 



```javascript

compactLayoutsDescribeResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).describe.compactLayouts.get()

```

```javascript

assert.equal( compactLayoutsDescribeResponse.status, 200 )

```



Returns a list of compact layouts for a specific object. (HEAD)



```javascript

compactLayoutsDescribeHeadResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).describe.compactLayouts.head()

```

```javascript

assert.equal( compactLayoutsDescribeHeadResponse.status, 200 )

```



Returns a list of layouts and descriptions, including for publisher actions. The list of fields and the layout name are returned.

This resource is available in REST API version 28.0 and later. When working with publisher actions, also refer to "Quick Actions".



```javascript

GlobalDescribeLayoutsResponse = client.version(VERSION).sobjects.Global.describe.layouts.get()

```

```javascript

assert.equal( GlobalDescribeLayoutsResponse.status, 200 )

```



Returns a list of layouts and descriptions, including for publisher actions. (HEAD)



```javascript

GlobalDescribeLayoutsHeadResponse = client.version(VERSION).sobjects.Global.describe.layouts.head()

```

```javascript

assert.equal( GlobalDescribeLayoutsHeadResponse.status, 200 )

```