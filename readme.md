# How to use the Circus General Purpose API v1.0.0

## What?!?!
What's this for? Well, sometimes you need to build a thing, and you only want to worry about the front-end. But if it needs to store data on a server somewhere, you need a back-end right? So what do you do? Well, you send the data to be stored on the Circus general purpose API, of course!

The Circus general purpose API will store whatever string or number you want. Later, you can retrieve or modify that string or number. All via AJAX.

## URLs
Construct your URLs like this:
`http://circuslabs.net:3000/data/your-key-here`
The same URL is used for getting and setting data. Replace `your-key-here` with whatever name you want your data to be stored in. Make it something that is unlikely to be used by another student.

## Sending data to the API
Do a POST request for the above URL, with a data object describing the data you want to store or update. If data already exists, it will be overwritten or modified. If it doesn't, it will be created. In the case of numbers, they default to 0.

```javascript
$.ajax("http://circuslabs.net:3000/data/example-key-12345", {
  method: "POST",
  data: {
    type: "string",
    content: "Hello World"
  }
}).done(function(data, textStatus, jqXHR) {
  console.log(data);
}).fail(function(jqXHR, textStatus, errorThrown) {
  console.warn(jqXHR.responseText);
})
```

Inside the data object, you need to specify a `type`. Valid types are `string` and `number`.

If you are storing a `string`, you also need to provide a `content` property, with the string you want to store.
```javascript
{
  type: "string",
  content: "Hello World"
}
```

If you are storing a `number`, you also need to provide an `action` property, with action you want to perform on the number, and sometimes a `quantity` property, with the amount you want to perform said action. See the examples if this doesn't make sense.
```javascript
{
  type: "number",
  action: "++" // increases the number by 1
}
```
```javascript
{
  type: "number",
  action: "--" // decreases the number by 1
}
```
```javascript
{
  type: "number",
  action: "+=", // increases the number by the quantity below
  quantity: 5
}
```
```javascript
{
  type: "number",
  action: "=", // sets the number to equal the quantity below
  quantity: 5
}
```


## Getting data from the API
Do a GET request for the above URL, with a data object describing the data you want to store/update.

```javascript
$.ajax("http://circuslabs.net:3000/data/example-key-12345", {
  method: "GET"
}).done(function(data, textStatus, jqXHR) {
  console.log(data);
}).fail(function(jqXHR, textStatus, errorThrown) {
  console.warn(jqXHR.responseText);
})
```

## Using Fetch instead of jQuery

### GET data from server
```javascript
  // key = the key name under which your data is stored
  let key = 'my-key';
  fetch("http://circuslabs.net:3000/data/" + key, {
    method: "GET",
    headers: {
      "Content-Type": "application/json"
    }
  })
  .then(response => {
    if (response.status === 200) {
      return response.json();
    }
    return "";
  })
  .then(data => {
    console.log("here is the response data for key:", key, data);
    }
  })
  .catch(function(err) {
    console.warn("error!", err);
  });
```

### POST data to server

```javascript

  // key = the key name under which to store your data
  // value = the value to save (this example assumes a string)
  let key = 'my-key';
  let jsonData = {
    type: "string",
    content: value
  };
  fetch("http://circuslabs.net:3000/data/" + key, {
    method: "POST",
    body: JSON.stringify(jsonData),
    headers: {
      "Content-Type": "application/json"
    }
  })
  .then(response => response.json())
  .then(data => {
    console.log("here is the saved response data!", data);
  })
  .catch(function(err) {
    console.warn("error!", err);
  });
```


## Using [Axios](https://github.com/axios/axios)

Axios is a project that simplifies the use of the browser's fetch function while also giving you more flexibility and options. 
Just include the CDN script tag in your page or site:

```
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

### GET data from server
```javascript
  // key = the key name under which your data is stored
  let key = 'my-key';
  axios.get("http://circuslabs.net:3000/data/" + key)
  .then(function (response) {
    console.log('here is the response data for key:', response);
  })
  .catch(function (error) {
    console.warn('axios encountered an error!', error);
  }); 
```

### POST data to server

```javascript

  // key = the key name under which to store your data
  // value = the value to save (this example assumes a string)
  let key = 'my-key';
  let jsonData = {
    type: "string",
    content: value
  };
  axios.get("http://circuslabs.net:3000/data/" + key, jsonData)
  .then(function (data) {
    console.log('here is the saved response data!', data);
  })
  .catch(function (error) {
    console.warn('axios encountered an error!', error);
  }); 
```

