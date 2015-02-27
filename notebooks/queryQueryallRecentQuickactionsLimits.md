---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/8111/versions/8305/portal/pages/6999/preview
apiNotebookVersion: 1.1.66
title: Query, queryAll, recent, quickActions, limits
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



Retrieve a list of API versions



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



Object type operated by the notebook



```javascript

OBJECT_NAME_VALUE = "Account"

```



Retrieve available quick actions.



```javascript

quickActionsResponse = client.version(VERSION).quickActions.get()

```

```javascript

assert.equal( quickActionsResponse.status, 200 )

```



Retrieve available quick actions. (HEAD)



```javascript

quickActionsHeadResponse = client.version(VERSION).quickActions.head()

```

```javascript

assert.equal( quickActionsHeadResponse.status, 200 )

```



Retrieve a particular quick action



```javascript

quickActionResponse = client.version(VERSION).quickActions.quickActionName("NewContact").get()

```

```javascript

assert.equal( quickActionResponse.status, 200 )

```



Invoke a quick action.



```javascript

quickActionInvokeResponse = client.version(VERSION).quickActions.quickActionName("NewContact").post({

  "record" : {

    "LastName" : "Smith"

  }

})

```

```javascript

assert.equal( quickActionInvokeResponse.status, 201 )

```



Lists information about limits in your organization. This resource is available in REST API version 29.0 and later for API

users with the “View Setup and Configuration” permission. The resource returns these limits:

• Daily API calls

• Daily Batch Apex and future method executions

• Daily Bulk API calls

• Daily Streaming API events

• Streaming API concurrent clients

• Daily number of single emails sent to external email addresses using Apex or Force.com APIs

• Daily number of mass emails sent to external email addresses using Apex or Force.com APIs

The resource also returns these limits if the API user has the “Manage Users” permission.

• Data storage (MB)

• File storage (MB)



```javascript

limitsResponse = client.version(VERSION).limits.get()

```

```javascript

assert.equal( limitsResponse.status, 200 )

```



 QueryAll will return records that have been deleted because of a merge or delete. QueryAll will also return information about archived Task and Event records. 



```javascript

queryAllResponse = client.version(VERSION).queryAll.get( {

  "q": "SELECT Name FROM Account",

  batchSizeSpecified : true,

  batchSize : 20

})

```

```javascript

assert.equal( queryAllResponse.status, 200 )

```



If the query results are too large, the response contains the first batch of results and a query identifier in the nextRecordsUrl field of the response.



```javascript

//method not supported

//queryIdentifierResponse = client.version(VERSION).queryAll.queryIdentifier("nextRecordsUrl").get()

```

```javascript

//assert.equal( queryIdentifierResponse.status, 200 )

```



Retrieve a list of the Salesforce app drop-down menu items



```javascript

appSwitcherResponse = client.version(VERSION).appMenu.AppSwitcher.get()

```

```javascript

assert.equal( appSwitcherResponse.status, 200 )

```



Retrieve a list of the Salesforce app drop-down menu items (HEAD)



```javascript

appSwitcherHeadResponse = client.version(VERSION).appMenu.AppSwitcher.head()

```

```javascript

assert.equal( appSwitcherHeadResponse.status, 200 )

```



Retrieve a list of the Salesforce1 navigation menu items



```javascript

salesforce1MenuResponse = client.version(VERSION).appMenu.Salesforce1.get()

```

```javascript

assert.equal( salesforce1MenuResponse.status, 200 )

```



Retrieve a list of the Salesforce1 navigation menu items (HEAD)



```javascript

salesforce1MenuHeadResponse = client.version(VERSION).appMenu.Salesforce1.head()

```

```javascript

assert.equal( salesforce1MenuHeadResponse.status, 200 )

```



Gets the list of icons and colors used by themes in the Salesforce application.





```javascript

themeResponse = client.version(VERSION).theme.get()

```

```javascript

assert.equal( themeResponse.status, 200 )

```



Executes the specified SOSL search. The search string must be URL-encoded.



```javascript

searchResponse = client.version(VERSION).search.get({

  "q": "FIND {Joe Smith} IN Name Fields"

})

```

```javascript

assert.equal( searchResponse.status, 200 )

```



Returns search result layout information for the objects in the query string. For each object, this call returns the list of fields displayed on the search results page as columns, the number of rows displayed on the first page, and the label used on the search results page. This call supports bulk fetch for up to 100 objects in a query.



```javascript

layoutInformationResponse = client.version(VERSION).search.layout.get({

  "q": "Account,Contact,Lead,Asset"

})

```

```javascript

assert.equal( layoutInformationResponse.status, 200 )

```



Returns an ordered list of objects in the default global search scope of a logged-in user. 



```javascript

scopeOrderResponse = client.version(VERSION).search.scopeOrder.get()

```

```javascript

assert.equal( scopeOrderResponse.status, 200 )

```



Returns a list of Flexible Pages and their details. Information returned includes Flexible Page regions, the components within each region, and each component’s properties, as well as any associated QuickActions.



```javascript

flexiPageIdResponse = client.version(VERSION).flexiPage.idOfFlexiblePage("").get()

```

```javascript

assert.equal( flexiPageIdResponse.status, 200 )

```



Returns a list of Flexible Pages and their details (HEAD).



```javascript

flexiPageIdHeadResponse = client.version(VERSION).flexiPage.idOfFlexiblePage("").head()

```

```javascript

assert.equal( flexiPageIdHeadResponse.status, 200 )

```



 Returns a specific object’s actions and their details.



```javascript

objectQuickActionsResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.get()

```

```javascript

assert.equal( objectQuickActionsResponse.status, 200 )

QUICK_ACTION_NAME = objectQuickActionsResponse.body[0].name

```



 Returns a specific object’s actions and their details. (HEAD)



```javascript

objectQuickActionsHeadResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.head()

```

```javascript

assert.equal( objectQuickActionsHeadResponse.status, 200 )

```



 Creating a contact on Account using an action



```javascript

//quickActionsCreateResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.post()

```

```javascript

//assert.equal( quickActionsCreateResponse.status, 200 )

```



Completely describes the individual metadata at all levels for the specified object.



```javascript

//describeResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.actionName(QUICK_ACTION_NAME).describe.get()

```

```javascript

//assert.equal( describeResponse.status, 200 )

```



Completely describes the individual metadata at all levels for the specified object.(HEAD)



```javascript

//describeHeadResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.actionName(QUICK_ACTION_NAME).describe.head()

```

```javascript

//assert.equal( describeHeadResponse.status, 200 )

```



Completely describes the individual metadata at all levels for the specified object.(POST)



```javascript

//actionDescribeCreateResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.actionName(QUICK_ACTION_NAME).describe.post()

```

```javascript

//assert.equal( actionDescribeCreateResponse.status, 200 )

```



 Returns a specific action’s default values, including default field values



```javascript

//objectActionDefaultValuesResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.actionName(QUICK_ACTION_NAME).defaultValues.get()

```

```javascript

//assert.equal( objectActionDefaultValuesResponse.status, 200 )

```



Returns a specific action’s default values headers.



```javascript

//defaultValuesHeadResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.actionName(QUICK_ACTION_NAME).defaultValues.head()

```

```javascript

//assert.equal( defaultValuesHeadResponse.status, 200 )

```



Create default values.



```javascript

//defaultValuesCreateResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.actionName(QUICK_ACTION_NAME).defaultValues.post()

```

```javascript

//assert.equal( defaultValuesCreateResponse.status, 200 )

```



Evaluate the default values for an action. Returns the default values specific to the {context id} object.



```javascript

//contextIdDefaultValuesResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.actionName(QUICK_ACTION_NAME).defaultValues.contextId(ID_VALUE).get()

```

```javascript

//assert.equal( contextIdDefaultValuesResponse.status, 200 )

```



Evaluate the default values for an action. Returns the default values specific to the {context id} object.(HEAD)



```javascript

//contextIdDefaultValuesHeadResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.actionName(QUICK_ACTION_NAME).defaultValues.contextId("contextIdValue").head()

```

```javascript

//assert.equal( contextIdDefaultValuesHeadResponse.status, 200 )

```



Create default values specific to the {context id} object.



```javascript

//actionDeafaultValuesCreateResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.actionName(QUICK_ACTION_NAME).defaultValues.contextId("contextIdValue").post()

```

```javascript

//assert.equal( actionDeafaultValuesCreateResponse.status, 200 )

```



Evaluate the default values for an action.



```javascript

//actionDefaultValuesResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.actionName(QUICK_ACTION_NAME).defaultValues.parentId("parentIdValue").get()

```

```javascript

//assert.equal( actionDefaultValuesResponse.status, 200 )

```



Returns headers.



```javascript

//actionDefaultValuesHeadResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.actionName(QUICK_ACTION_NAME).defaultValues.parentId("parentIdValue").head()

```

```javascript

//assert.equal( actionDefaultValuesHeadResponse.status, 200 )

```



Create the default values for an action.



```javascript

//defaultValuesCreateResponse = client.version(VERSION).sobjects.SObjectName(OBJECT_NAME_VALUE).quickActions.actionName(QUICK_ACTION_NAME).defaultValues.parentId("parentIdValue").post()

```

```javascript

//assert.equal( defaultValuesCreateResponse.status, 200 )

```