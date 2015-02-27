---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/8111/versions/8305/portal/pages/7000/preview
apiNotebookVersion: 1.1.66
title: Password
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



Retrieve all users



```javascript

usersResponse = client.version(VERSION).sobjects.SObjectName("User").get()

```

```javascript

TEST_USER_NAME = prompt("Please, enter name of your test user")

```



Retrieve a test user



```javascript

TEST_USER_ID = null

for(var ind in usersResponse.body.recentItems){

  var user = usersResponse.body.recentItems[ind]

  if(user.Name == TEST_USER_NAME){

    TEST_USER_ID = user.Id

    break

  }

}

```





Retrieve password expiration status



```javascript

//Starting with Spring ’12, the Self-Service portal isn’t available for new organizations. Existing organizations continue to have access to the Self-Service portal.

//selfServiceUserPasswordExpirationResponse = client.version(VERSION).sobjects.SelfServiceUser.selfServiceUserId("SELF_USER_ID").password.get()

```

```javascript

//assert.equal( selfServiceUserPasswordExpirationResponse.status, 200 )

```



Retrieve password expiration status



```javascript

//Starting with Spring ’12, the Self-Service portal isn’t available for new organizations. Existing organizations continue to have access to the Self-Service portal.

//SelfServiceUserPasswordExpirationHeadResponse = client.version(VERSION).sobjects.SelfServiceUser.selfServiceUserId(SELF_USER_ID).password.head()

```

```javascript

//assert.equal( SelfServiceUserPasswordExpirationHeadResponse.status, 200 )

```



Set the password



```javascript

//Starting with Spring ’12, the Self-Service portal isn’t available for new organizations. Existing organizations continue to have access to the Self-Service portal.

//selfServiceUserPasswordCreateResponse = client.version(VERSION).sobjects.SelfServiceUser.selfServiceUserId(SELF_USER_ID).password.post({

//  "NewPassword" : "myNewPassword1234"

//})

```

```javascript

//assert.equal( selfServiceUserPasswordCreateResponse.status, 200 )

```



Reset the password



```javascript

//Starting with Spring ’12, the Self-Service portal isn’t available for new organizations. Existing organizations continue to have access to the Self-Service portal.

//selfServiceUserPasswordDeleteResponse = client.version(VERSION).sobjects.SelfServiceUser.selfServiceUserId(SELF_USER_ID).password.delete()

```

```javascript

//assert.equal( selfServiceUserPasswordDeleteResponse.status, 200 )

```



Retrieve password expiration status



```javascript

userPasswordExpirationResponse = client.version(VERSION).sobjects.User.userId(TEST_USER_ID).password.get()

```

```javascript

assert.equal( userPasswordExpirationResponse.status, 200 )

```



Retrieve password expiration status (HEAD)



```javascript

userPasswordExpirationHeadResponse = client.version(VERSION).sobjects.User.userId(TEST_USER_ID).password.head()

```

```javascript

assert.equal( userPasswordExpirationHeadResponse.status, 200 )

```



Reset the password





```javascript

userPasswordDeleteResponse = client.version(VERSION).sobjects.User.userId(TEST_USER_ID).password.delete()

```

```javascript

assert.equal( userPasswordDeleteResponse.status, 200 )

```



Set the password



```javascript

// newPassword = prompt("Please, enter your new password. Password should contain upper and lower case, digits")

userPasswordCreateResponse = client.version(VERSION).sobjects.User.userId(TEST_USER_ID).password.post({

  "NewPassword" : "qwerty" + new Date().getMilliseconds()

})

```

```javascript

assert.equal( userPasswordCreateResponse.status, 204 )

```