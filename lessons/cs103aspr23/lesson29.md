# APIs

# Authentication
We have now seen enough Express and Mongoose to be able to read the code in ```routes/pwauth.js```
to see how username/password authentication works.


# OpenAI integratation
We can use an [openai Express API](https://github.com/openai/openai-node) to access GPT from an express app.


# API Access
We show how to access an API in Express using the [US Weather Service API](https://www.weather.gov/documentation/services-web-api) at
``` html
https://www.weather.gov/documentation/services-web-api
```
but to get the weather at an address we need to use the US Government geocoder API, e.g.
```
https://geocoding.geo.census.gov/geocoder/locations/onelineaddress?address=152%20Aspinwall%20Ave%20Brookline%20MA%2002446&benchmark=2020&format=json
```

