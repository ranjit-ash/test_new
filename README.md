# Restaurant Score Service 


## Getting Started

This project uses Python 3.7.

### Developer Environment Setup

Create a virtualenv using the Python [venv module](https://docs.python.org/3/library/venv.html) or [pew](https://github.com/berdario/pew) (*P*ython *E*nvironment *W*rapper):

```
# Using venv
python -m venv venv

# The virtualenv won't be activated by default so activate it with:
. venv/bin/activate
```

Now install all dependencies:

```
pip install -r requirements.txt
```

To deactivate the virtualenv:

```
# Using venv
deactivate
```

Kubernetes Environment for DB

```
# Perference minikube
minikube start
```
### PyCharm

If using PyCharm, configure the project with the following:

- Preferences > Tools > Python Integrated Tools
  - Package requirements file: `requirements.txt`


## Deployment and use

### DB setup
Setting up DB [POSTGRES]
```
# start postgress in kubernetes
kubectl apply -f k8s/postgres/postgres_wl.yaml
```

Setting up DB [PGADMIN]
```
# start postgress in kubernetes
kubectl apply -f k8s/pgadmin/pgadmin_wl.yaml
```
### RestAPI setup
Set PYTHONPATH
```
# absolute resturant folder path
# linux
export=~/Restaurant/

# windows
set PYTHONPATH=D:\Restaurant\
```
Update constant variable
 ```
USER = "postgres"
PASSWORD = "root"
HOST = <DB server IP>
PORT = "30432" 
```

### Start RestAPI
Update constant variable
 ```
Restaurant/rest_api/app.py
```
### Create table
To create table and insert sample data run script
```
python Restaurant/script/db_table.py
```
#### Run Unit Tests

Run all unit tests.

```
\Restaurant>pytest "scripts\unit_test" -v
```

## API calls in Restaurant

  a. `GET`  / `Welcome screen`
  
  Example-:
  
  ```
  http://localhost:5000/
  ```
  
  Response:
  ```
   Welcome to restaurant API

  ```
   
  b. `GET`  /{table}/select `Get all rows`
  
  Example-:
  
  ```
  http://127.0.0.1:5000/restaurant_tb/select
  ```
  
  Response:
  
  ```
  {
  "result": [
    {
      "business_address": "2 Marina Blvd Fort Mason", 
      "business_city": "San Francisco", 
      "business_id": "101192", 
      "business_latitude": null, 
      "business_location": null, 
      "business_longitude": null, 
      "business_name": "Cochinita #2", 
      "business_phone_number": "+14150429222", 
      "business_postal_code": null, 
      "business_state": "CA", 
      "inspection_date": "Thu, 06 Jun 2019 00:00:00 GMT", 
      "inspection_id": "101192_20190606", 
      "inspection_score": null, 
      "inspection_type": "New Ownership", 
      "risk_category": null, 
      "status": null, 
      "violation_description": null, 
      "violation_id": null
    }, 
    {...},
    {...},
    ]
  } 
  ```

  c. `GET`  /{table}/select `Get table rows before modified date (YYYY-MM-DD) `

  `Note:` Id will be provided by package list api and the file download path here is configurable and is set to `/data` by default.

  
  Example-:
  
  ```
  http://127.0.0.1:5000/restaurant_tb/select/2017-01-01
  ```
  
  Response:
  
  ```
    {
        "result": [
          {
            "business_address": "798 Eddy St",
            "business_city": "San Francisco",
            "business_id": "85986",
            "business_latitude": null,
            "business_location": null,
            "business_longitude": null,
            "business_name": "Pronto Pizza",
            "business_phone_number": null,
            "business_postal_code": "94109",
            "business_state": "CA",
            "inspection_date": "Tue, 11 Oct 2016 00:00:00 GMT",
            "inspection_id": "85986_20161011",
            "inspection_score": null,
            "inspection_type": "New Ownership",
            "risk_category": "High Risk",
            "status": null,
            "violation_description": "High risk vermin infestation",
            "violation_id": "85986_20161011_103114"
           }
        ],
    }

   ```

  d. `PUT`  ​/{table}/update?{key}={value} `update row`
      payload{}
   `Note:` The upload/download API response will contain an id of that package that will be uploaded/downloaded.

   Example-:

   ```
    http://127.0.0.1:5000/restaurant_tb/update?business_id=99999
    payload = {
      "business_address": "1362 Stockton St update update", 
      "business_city": "San Francisco update"
       }
   ```
   
   Response:
   
   ```
    {
    "result": "UPDATE 1"
    }
   ```

  e. `DELETE` /<table>/delete `delete row`
  
  Example-:
 
   ```
        http://127.0.0.1:5000/restaurant_tb/delete?business_id=99999
   ```
   
  Response:
   
    ```
    {
    "result": "DELETE 1"
    }
    ```


  f. `POST` /{table}/insert `insert row`
   
  Example-:
  
   ```
    http://127.0.0.1:5000/restaurant_tb/insert
    payload = {
      "business_address": "1362 Stockton St update",
      "business_city": "San Francisco update2",
      "business_id": "99999",
      "business_latitude": null,
      "business_location": null,
      "business_longitude": null,
      "business_name": "Fancy Wheatfield Bakery",
      "business_phone_number": null,
      "business_postal_code": "94133",
      "business_state": "CA",
      "inspection_id": "69618_20190304",
      "inspection_score": null,
      "inspection_type": "Complaint",
      "risk_category": "Moderate Risk",
      "violation_description": "Inadequate sewage or wastewater disposal",
      "violation_id": "69618_20190304_103130"
    }
   ```
   
   Response:
   
   ```
    {
        "result": "INSERT 0 1"
    }

   ```
   ## Design
   ###Architectural and design choices you made for writing this code and the reasoning for these.
   Architecture
   ```
    APP->RestAPI<->[client]<->[DB server]
   ```
   Package/APP choice
   ```
   Postgres: Postgres a free and open-source relational database management system 
        emphasizing extensibility and SQL compliance

   Flask: Flask is a popular micro framework for building web applications.
       Since it is a micro-framework, it is very easy to use and lacks most
       of the advanced functionality which is found in a full-fledged framework. 
       Therefore, building a REST API in Flask is very simple.

   psycopg2: python package to connect postgres db
   ```

   ## Trade-offs
   ### Leftout and additional code
   Leftout
   ```
    Multiple unittest/test scenarios are missed out 

   ```
   Additional code
   ```
    Multiple unittest to get maximum code coverage 
    increse RestAPI calls

   ```
   ## 3rd party Code
   ###Code which does not belong to me
   ```
   Kubernetes code for postgres and pgadmin is from net
   ```
       

   