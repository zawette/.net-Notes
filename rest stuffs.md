# REST
## REST constraints
+ Client-Server: client and server are separated ( they can evolve separately)
+ Statelessness: state is contained within the request ( when a client requests a ressource, that request contains all the necessary information (this means that a combination of uri,header,method , body is sufficient for the api to fully understand the request ))
+ Cacheable: each response must explicitly state wether it can be cached or not (if you check the response headers of a rest api get call for example , you'll find some cache related headers like cache-Control , Expires,last modified )
+ Layered system: allows you to use a layered system architecture where you deploy the APIs on server A, and store data on server B and authenticate requests in Server C , and a client cannot tell what layer its connected to
## Best Practises
+ Nouns ,things, not actions ( ~~api/getauthors~~ ; correct way: GET api/authors)
+ representing hierarchy when naming ressources : api/authors/authorId/courses/courseId
+ Filters,sorting,orders ... aren't ressources: ( ~~api/authors/orderby/name~~ ; correct way: GET api/authors?orderby=name)
## Status codes
+ returning the correct status code is important to let the consumer know whether or not the request worked out as expected and what is repsonsible for a failed request
+ Level 2xx means that the request went well, the most common ones are
    + level 200 - ok
    + level 201 - created
    + level 204 - No content (successful request that shouldn't return anything, like when you delete something)
+ level 4xx lets the consumer knows that he did something wrong
    + level 400 - bad Request 
    + level 401 - unauthorized (means that no or invalid authentification details were provided)
    + level 403 - forbidden (means that the authentification succeded but the user dosen't have access to the ressource) 
    + level 404 - ressource dosen't exist
    + level 405 - method not allowed ( for example sending a post request when only get is implemented)
    + level 406 - Not acceptable ( consumer makes a request in a format that isn't supported by the api (for example in xml format while the api only supports json))
+ level 5xx these are server mistakes
    + 500 - internal server error
## http methods
+ get for getting the ressource
+ post for creating a new ressource
+ put for updating a ressource
+ patch for partialy updating a ressource
+ head is identical to get , the only difference is that it shouldn't return a response body

## Method safety and idempotency
|HTTP Method|Idempotent|Safe|
|------------|----------|------|
|GET|yes|yes|
|OPTIONS|yes|yes|
|HEAD|yes|yes|
|POST|no|no|
|PUT|yes|no|
|DELETE|yes|no|
|PATCH|no|no|
