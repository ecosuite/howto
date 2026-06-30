# How to request a new Location ID from SolarNetwork

![](../.gitbook/assets/0.png)

How to request a new Location ID from SolarNetwork

v.2024.08.02

Overview:

This document is intended to aid in the setup of local weather info for a new project, regardless of the weather source.

Context:

Weather is persisted on SolarNet in a way that is independent of the production or consumption energy information. Rather than be tied to a StreamId made up of nodeId and sourceId, weather information is tied to a LocationId. This is a latitude/longitude position held on SolarNet that many different projects can use. Weather can come from a variety of sources, such as OpenWeatherMap, WeatherFlow Tempest devices, Norwegian weather (Yr) and others - and can be collected by a node or by a Cloud Integration.

Relevant Roles for this document:

* Asset Manager
* Onboarding Team

Resources needed:

* Browser
* SN account credentials and a User Token
* Data Explorer App: [https://go.solarnetwork.net/dev/api/](https://go.solarnetwork.net/dev/api/)
* The full address of the location you want to get weather at

Related Resources:

* [Saving OpenWeatherMap data to a SN Location ID from a Node](https://docs.google.com/document/d/1IOtoxTteFA2HVGMmEXeMTjDQEJE6romx5lssWB3d1hA/edit?usp=sharing)
* [How to Cloud Integrate with OpenWeatherMap](https://docs.google.com/document/d/1l3DzXOebL9RPdqK8oNa2P_g2dKSI_uzlq4TGFArBPFY/edit?usp=sharing)

Step by Step Process:

Step 1: Find your location’s latitude and longitude (lat/long)

Ideally this should be done using the info already in Ecosuite for the project you are configuring.

e.g.

![](<../.gitbook/assets/1 (6).png>)

Alternatively (but only if you are not intending to use Ecosuite, or the project has not been created yet for some reason), it is also easy to use Google Maps. Go to Google Maps:

[https://www.google.co.nz/maps/preview](https://www.google.co.nz/maps/preview)

And paste in the full address of your location for example:

697 Melville Ave, Fairfield, CT 06825, USA

![](<../.gitbook/assets/2 (5).png>)

When you search for this address, in the browser it exposes the lat/long of this place:

![](<../.gitbook/assets/3 (4).png>)

In our example the values are:

**Latitude**: 41.1863234

**Longitude**: -73.2344506

Step 2: Find the nearest weather station on OpenWeatherMap

Go to: [https://openweathermap.org/](https://openweathermap.org/)

And search in our case for: **Fairfield, US**

![](<../.gitbook/assets/4 (2).png>)

You will see some results often:

![](<../.gitbook/assets/5 (3).png>)

Of the 5 that are listed here, the 4th one is correct because it matches our lat/long values. Click on the link for that 4th entry in this case and you will get a page showing a map that in our case shows the state of Connecticut.

![](<../.gitbook/assets/6 (3).png>)

The ID for this OpenWeatherMap location is: **4834157**

Step 3: Compose your request

In order to get a LocationId established on SolarNet, you need to POST a JSON document to SolarNet endpoint. Let’s compose that JSON object now using this format:

{

"sourceId": "OpenWeatherMap",

"features": \["weather", "day", "forecast"],

"location": {

"country": "US",

"zone": "America/New\_York",

"stateOrProvince": "Connecticut",

"locality": "Fairfield",

"name": "Fairfield"

}

}

Step 4:Authenticate with Data Explorer and Post the JSON object to the endpoint

Use the [API Explorer](https://go.solarnetwork.net/dev/api/index.html) using Method **POST** and Output **JSON** a request using this endpoint:

/solaruser/api/v1/sec/location/meta/request

And the contents of your JSON document in the Upload text field:

{

"sourceId": "OpenWeatherMap",

"features": \["weather", "day", "forecast"],

"location": {

"country": "US",

"zone": "America/New\_York",

"stateOrProvince": "Connecticut",

"locality": "Fairfield",

"name": "Fairfield"

}

}

And then click Execute. You should see a Success result (as shown in screenshot example below).

![](<../.gitbook/assets/7 (4).png>)

Step 4: Notify your SolarNetwork contact to approve the LocationId submission.

Step 5: Check back when the location has been created to confirm it exists

Using the Data Explorer app, compose a service request that looks for the name of your location in this form:

/solarquery/api/v1/pub/location/meta?query=\<the city name>\&tags=weather

So in our case for Fairfield, CT the service request would look like:

/solarquery/api/v1/pub/location/meta?query=fairfield\&tags=weather

And you don’t have to put in the whole name just part of the letters of the name to trigger a match:

![](<../.gitbook/assets/8 (1).jpeg>)

_Note: if you get **unauthorised** error, try with Auth = None_

You will see the value **11565198** in the result JSON document - that’s your new LocationId!

![](<../.gitbook/assets/9 (3).png>)

_This API is documented here:_ [_https://github.com/SolarNetwork/solarnetwork/wiki/SolarUser-Location-Request-API_](https://github.com/SolarNetwork/solarnetwork/wiki/SolarUser-Location-Request-API)

_Timezone info (zone) can be found here:_ [_https://en.wikipedia.org/wiki/List\_of\_tz\_database\_time\_zo&#x6E;_&#x65;s](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

_OpenWeatherMap website:_ [_https://openweathermap.org/city/5146965_](https://openweathermap.org/city/5146965)

**An example valid request “upload” looks like this:**

{

"sourceId": "OpenWeatherMap",

"features": \["weather", "day", "forecast"],

"location": {

"country": "US",

"zone": "America/Fort\_Wayne",

"stateOrProvince": "Ohio",

"locality": "Bellefontaine",

"name": "Bellefontaine"

}

}

**The response looks like this:**

created,id,jsonData,modified,status,userId

2024-08-01T06:20:54.846837Z,13,"{""features"": \[""weather"", ""forecast"", ""day""], ""location"": {""name"": ""Bellefontaine"", ""zone"": ""America/Fort\_Wayne"", ""country"": ""US"", ""locality"": ""Bellefontaine"", ""stateOrProvince"": ""Ohio""}, ""sourceId"": ""OpenWeatherMap""}",2024-08-01T06:20:54.846837Z,Submitted,385

**Ping Matt (or other SolarNetwork administrator) on slack to get the created Location ID:**

![](<../.gitbook/assets/10 (2).png>)

If Matt does not provide the location id, you can also query to find it in SN using the API Explorer as follows, note that NO authentication is required, in fact this ONLY works if you pick the “None” option for the Auth setting (query text below is “/solarquery/api/v1/pub/location/meta?query=Frederick”):

![](<../.gitbook/assets/11 (2).png>)

_For now this last step is still manual, in the future we might automate this also (_ [_https://ecogyenergy.atlassian.net/browse/EP-1318_](https://ecogyenergy.atlassian.net/browse/EP-1318) _)._
