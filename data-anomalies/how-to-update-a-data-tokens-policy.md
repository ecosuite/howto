# How to update a Data Token's policy

![](../.gitbook/assets/0.png)

How to update a Data Token's policy

v.2024.06.22a

Overview:

This document will describe how to update a policy of an existing target Data Token to allow that token to be used with more node ID and source ID values.

Relevant Roles for this document:

* Asset Manager
* Database Administrator

Tools and Information required:

* Web browser
* Text editor
* Credentials to SolarNetwork
* The [API Explorer](https://go.solarnetwork.net/dev/api/)

Step by Step procedure

Step &#x31;**.** Login to SolarNetwork and click on Security Tokens and scroll down to Data Tokens

Login at [https://data.solarnetwork.net/solaruser/](https://data.solarnetwork.net/solaruser/)

Step &#x32;**.** Identify and test the data token that you wish to update

You will see a list of tokens, and the source IDs that they give access to. We wish to expand a token’s policy to include new source IDs and possibly ones from new node IDs. Let’s first test the token against some data it can access, and then try it with some data it cannot yet access to review the results sets:

If we use an existing token with access to node ID: 350 and source ID: AE 500NX 1 then a SolarQuery expression that delivers results would be:

**/solarquery/api/v1/sec/datum/mostRecent?nodeId=350\&sourceIds=AE%20500NX%201**

As shown here in [API Explorer](https://go.solarnetwork.net/dev/api/):

![](<../.gitbook/assets/1 (5).jpeg>)

And we can tell that this token does NOT work with the a different node ID and source ID, namely 338 and /LN/RC/S1/GEN/1 using this SolarQuery expression:

**/solarquery/api/v1/sec/datum/mostRecent?nodeId=338\&sourceIds=/D2/WH/S1/GEN/1**

![](<../.gitbook/assets/2 (2).jpeg>)￼

As you can see you are not authorized to view this data, and that’s because the policy for your target token does not include that node ID and source ID - so let’s merge a new policy into that existing policy to expand this Data Token’s ability to access datum.

Step &#x33;**.** Create policy patch object

Let’s create the expression of the additional node IDs and source IDs we’d like to grant access to for this token. In our case if we wanted to add the following node IDs and source IDs to this policy, the JSON expression we will use will be:

{

"nodeIds": \[

338

],

"sourceIds": \[

"/D2/WH/S1/GEN/1",

"/D2/WH/S1/INV/1",

"/D2/WH/S1/INV/2"

]

}

Note: Editing JSON documents can get tricky/subtle and worth using a JSON-aware editor to validate the JSON content like the example above is worthwhile before trying to use it. One online JSON-aware editor can be found at:

[https://jsoneditoronline.org/](https://jsoneditoronline.org/)

This document contains all the information that will allow us to expand the policy of this token. The way we will use this expression is to paste it into API Explorer

Step &#x34;**.** Use API Explorer to modify the policy of the target token you have identified

Note: to accomplish this step you must log into API Explorer using an administrator token that has rights to edit this target token’s policy. That means that the Token and Secret values you use cannot be the ones whose policy you are trying to edit - so you are essentially “re-logging in” to API Explorer as an administrator that can create Data Tokens on this SolarNetwork account and edit policies of the existing Data Tokens.

Note: The value for the token you use in the next expression should be a URL-encoded value. That means that while a Token ID could only have valid alpha-numeric characters allowed to be used in URLs, most if not all generated token IDs will contain special characters that cannot be used in URLs. The solution here is that the literal text string expression of the Token ID needs to be URL-Encoded to be passed as a valid string to the REST API. A good way to URL-encode your token ID is to use a tool like:

[https://www.urlencoder.org/](https://www.urlencoder.org/)

In the Service text box of API explorer, you will use the expression:

**/solaruser/api/v1/sec/user/auth-tokens/policy?tokenId=ABCD1234**

Where the token ID in this case is the shown as a literal value:

**ABCD1234**

But if it was for example:

**ABC;D12,34**

The URL-encoded value would be:

**ABC%3BD12%2C34**

You can find a URLEncoding tool here: [https://www.urlencoder.org/](https://www.urlencoder.org/)

Method should be set to PATCH

Output should be set to JSON

And in the Upload textbox, you want to paste your policy merge object from above that defines the node IDs and source IDs you want this token’s policy to include.

![](<../.gitbook/assets/3 (1).jpeg>)

You should see a result showing **“success”: true**, and the new policy shown for that token ID.

![](<../.gitbook/assets/4 (2).jpeg>)

Step &#x35;**.** Using the target token now, confirm the SolarQuery that did not work earlier now does

Note that we are now using API Explorer as the user with this target token, so the Token and Secret values should be the ones of the **target token**, not the administrator token.

When issuing the SolarQuery that did not work before:

**/solarquery/api/v1/sec/datum/mostRecent?nodeId=338\&sourceIds=/D2/WH/S1/GEN/1**

You can now see that it does return a result set now, and that is because we have expanded the policy of this Data Token to include the new node IDs and the new source IDs.
