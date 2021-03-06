# ConsoleUFOODO

This project is REST API for automatic generating food menu considering parametres and objectives of person.

###1) To work with this API the first thing you need is Task.
**Task format (JSON) :**

```json
{
"userID" : "some_username",
"sex" : "male/female (default: male)",
"age" : "years_integer (default: 25)",
"height" : "cm/inches_double (default: 175cm)",
"weight" : "kg/pounds_double (default: 75kg)",
"activity_level" : "one_of: sedentary,light,moderate,high,extra_high (default:light)",
"body_type" : "one_of: fat,normal,athlete,thin (default: normal)",
"foodItems" : ["food_item_name1", "..."],
"metricUS" : "0/1 (default: 0 - metric[kg,cm])"
}
```

**Example :**

```json
{ 
"userID" : "masha90", 
"sex" : "female", 
"age" : 18, 
"height" : 160.0, 
"weight" : 53.0, 
"activity_level" : "light", 
"body_type" : "normal", 
"foodItems" : ["apple", "potato", "chicken"],
"metricUS" : 0
}
```

### 2) Next thing you need to get desired food menu is: make a call to API using Task above

**API endpoint :**
<table>
  <tr>
    <td>Endpoint</td>
    <td>Request type</td>
    <td>Consumes</td>
    <td>Produces</td>
  </tr>
  <tr>
    <td>https://ufoodo.com/mongorest/control/get-menu</td>
    <td>PUT</td>
    <td>application/json;charset=utf-8</td>
    <td>text/plain</td>
  </tr>
</table>

**Example output of this API call with example task above :**
```json
{ 
"userID" : "masha90", 
"task" : 
  { 
  "properties" : 
    { 
    "sex" : "female", 
    "age" : 18, 
    "height" : 160.0, 
    "weight" : 53.0, 
    "activity_level" : "light", 
    "body_type" : "normal", 
    "metricUS" : 0 
    }, 
  "foodItems" : ["apple", "potato", "chicken"] 
  }, 
  "menu0" : 
    { 
    "calories" : 480.0, 
    "proteins" : 180.0, 
    "carbohydrates" : 185.0, 
    "fats" : 100.0, 
    "dishes" : [
                { 
                "name" : "soup", 
                "calories" : 290.0, 
                "proteins" : 100.0, 
                "carbohydrates" : 130.0, 
                "fats" : 60.0, 
                "food_items" : [
                                { 
                                "name" : "chicken", 
                                "calories" : 200.0, 
                                "proteins" : 50.0, 
                                "carbohydrates" : 100.0, 
                                "fats" : 50.0, 
                                "quantity" : 1.0 
                                }, 
                                { 
                                "name" : "potato", 
                                "calories" : 90.0, 
                                "proteins" : 50.0, 
                                "carbohydrates" : 30.0, 
                                "fats" : 10.0, 
                                "quantity" : 1.0 
                                }] 
                }, 
                { 
                "name" : "apple salat", 
                "calories" : 100.0, 
                "proteins" : 30.0, 
                "carbohydrates" : 25.0, 
                "fats" : 30.0, 
                "food_items" : [
                                { 
                                "name" : "apple", 
                                "calories" : 100.0, 
                                "proteins" : 30.0, 
                                "carbohydrates" : 25.0, 
                                "fats" : 30.0, 
                                "quantity" : 1.0 
                                }] 
                }, 
                { 
                "name" : "boiled potatoes", 
                "calories" : 90.0, 
                "proteins" : 50.0, 
                "carbohydrates" : 30.0, 
                "fats" : 10.0, 
                "food_items" : [
                                { 
                                "name" : "potato", 
                                "calories" : 90.0, 
                                "proteins" : 50.0, 
                                "carbohydrates" : 30.0, 
                                "fats" : 10.0, 
                                "quantity" : 1.0 
                                }] 
                }] 
    } 
  }
```

## Other features

### Add new food_item or dish to server's Database :

**API endpoints :**
<table>
  <tr>
    <td>Endpoint</td>
    <td>Request type</td>
    <td>Consumes</td>
    <td>Produces</td>
  </tr>
  <tr>
    <td>https://ufoodo.com/mongorest/control/admin/add-fooditem</td>
    <td>PUT</td>
    <td>application/json;charset=utf-8</td>
    <td>text/plain</td>
  </tr>
  <tr>
    <td>https://ufoodo.com/mongorest/control/admin/add-dish</td>
    <td>PUT</td>
    <td>application/json;charset=utf-8</td>
    <td>text/plain</td>
  </tr>
</table>

**Example food_item to insert :**
```json
{ 
"name" : "chicken", 
"calories" : 200.0, 
"proteins" : 50.0, 
"carbohydrates" : 100.0, 
"fats" : 50.0, 
"quantity" : 1.0 
}
```

**Example dish to insert :**
* 1) full json
```json
{ 
"name" : "boiled potatoes", 
"calories" : 90.0, 
"proteins" : 50.0, 
"carbohydrates" : 30.0, 
"fats" : 10.0, 
"food_items" : [
                { 
                "name" : "potato", 
                "calories" : 90.0, 
                "proteins" : 50.0, 
                "carbohydrates" : 30.0, 
                "fats" : 10.0, 
                "quantity" : 1.0 
                }
                ] 
}
```
* 2) short json (once this dish will be included in some menu it will be automatically replaced in Database with full form)
```json
{ 
"name" : "boiled potatoes", 
"calories" : 90.0, 
"proteins" : 50.0, 
"carbohydrates" : 30.0, 
"fats" : 10.0, 
"food_items" : ["potato"] 
}
```

**As result you will get answer like :**
```json
{ "message" : "dish inserted"}
```

or

```json
{ "message" : "food_item inserted"}
```

### Get all food_items or dishes in Database

**API endpoints :**
<table>
  <tr>
    <td>Endpoint</td>
    <td>Request type</td>
    <td>Consumes</td>
    <td>Produces</td>
  </tr>
  <tr>
    <td>https://ufoodo.com/mongorest/control/get-fooditems</td>
    <td>GET</td>
    <td>application/json;charset=utf-8</td>
    <td>text/plain</td>
  </tr>
  <tr>
    <td>https://ufoodo.com/mongorest/control/get-dishes</td>
    <td>GET</td>
    <td>application/json;charset=utf-8</td>
    <td>text/plain</td>
  </tr>
</table>

**Result will look like :**
```json
{"food_items" : []}
```

or

```json
{"dishes" : []}
```

### Search food_items or dishes in Database:

**API endpoints :**

<table>
  <tr>
    <td>Endpoint</td>
    <td>Request type</td>
    <td>Consumes</td>
    <td>Produces</td>
  </tr>
  <tr>
    <td>https://ufoodo.com/mongorest/control/search-fooditems</td>
    <td>PUT</td>
    <td>application/json;charset=utf-8</td>
    <td>text/plain</td>
  </tr>
  <tr>
    <td>https://ufoodo.com/mongorest/control/search-dishes</td>
    <td>PUT</td>
    <td>application/json;charset=utf-8</td>
    <td>text/plain</td>
  </tr>
</table>

**Request example :**

```json
{ "name" : "ap" }
```

**Result of resuest above :**

```json
{ 
"found_items" : [
                { 
                "_id" : { "$oid" : "56b899675daf813030836654" }, 
                "name" : "apple", 
                "calories" : 100, 
                "proteins" : 30, 
                "carbohydrates" : 25, 
                "fats" : 30, 
                "quantity" : 1 
                }] 
}
```


### Full list of endpoints :
<table>
  <tr>
    <td>Endpoint</td>
    <td>Request type</td>
    <td>Consumes</td>
    <td>Produces</td>
  </tr>
  <tr>
    <td>https://ufoodo.com/mongorest/control/get-menu</td>
    <td>PUT</td>
    <td>application/json;charset=utf-8</td>
    <td>text/plain</td>
  </tr>
  <tr>
    <td>https://ufoodo.com/mongorest/control/admin/add-fooditem</td>
    <td>PUT</td>
    <td>application/json;charset=utf-8</td>
    <td>text/plain</td>
  </tr>
  <tr>
    <td>https://ufoodo.com/mongorest/control/admin/add-dish</td>
    <td>PUT</td>
    <td>application/json;charset=utf-8</td>
    <td>text/plain</td>
  </tr>
  <tr>
    <td>https://ufoodo.com/mongorest/control/get-fooditems</td>
    <td>GET</td>
    <td>application/json;charset=utf-8</td>
    <td>text/plain</td>
  </tr>
  <tr>
    <td>https://ufoodo.com/mongorest/control/get-dishes</td>
    <td>GET</td>
    <td>application/json;charset=utf-8</td>
    <td>text/plain</td>
  </tr>
  <tr>
    <td>https://ufoodo.com/mongorest/control/search-fooditems</td>
    <td>PUT</td>
    <td>application/json;charset=utf-8</td>
    <td>text/plain</td>
  </tr>
  <tr>
    <td>https://ufoodo.com/mongorest/control/search-dishes</td>
    <td>PUT</td>
    <td>application/json;charset=utf-8</td>
    <td>text/plain</td>
  </tr>
</table>

