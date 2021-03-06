## Scopes admin

- Only superadmin can call these methods

### GET /admin/scopes
```json
[{
  "id": "id",
  "name": "Name",
  "status": 1,
  "multi":false
}]
```

### POST /admin/scopes

Sample for multi scope
```json
{
  "name": "Name",
  "location": [37.237364, -5.103308],
  "zoom": 15,
  "multi": true
}
```
Sample for non-multi inside a multi
```json
{
  "name": "Name",
  "location": [37.237364, -5.103308],
  "zoom": 15,
  "multi" : false,
  "parent_id" : "pre_id"
}
```

Sample for non-multi
```json
{
  "name": "Name",
  "location": [37.237364, -5.103308],
  "zoom": 15,
  "multi": false
}
```

if ok it returns the scope id:
```json
{
  id: "id"
}
```

Notes:
- Primary key and dbschema will be autogenerated.
- Status will be set to 0.

### GET /admin/scopes/:id_scope

```json
{
  "id": "id",
  "name": "Name",
  "location": [37.237364, -5.103308],
  "zoom": 15,
  "multi":false,
  "parent_id": "pre_id",
}
```

### PUT /admin/scopes/:id_scope
```json
{
  "name": "Name",
  "location": [37.237364, -5.103308],
  "zoom": 15,
  "dbschema": "schema",
  "status":1
}
```

If error it returns a error status code and a message:
```json
[
  {
    "param": "dbschema",
    "msg": "already used"
  }
]
```

if ok it returns:
```json
{
  status: "ok"
}
```
Notes:
 - It only updates the fields provided
 - Multi and parent_id fields are not editable.

### DELETE /admin/scopes/:id_scope

Delete the scope.

if ok it returns:
```json
{
  status: "ok"
}
```

### GET /admin/scopes/:id_scope/multi/children

It returns the list of scopes who belong to the requested scope
```json
[{
  "id": "id",
  "name": "Name",
  "status": 1
}]
```

### POST /admin/scopes/:id_scope/categories

It adds a category to the scope
```json
{
  "id": "vertical",
  "name": "Name"
}
```
### DELETE /admin/scopes/:id_scope/categories/:id_category

It removes a category from the scope

### POST /admin/scopes/:id_scope/entities

It adds an entity to the scope
```json
{
  "id": "<id_entity>",
  "name" : "Entity name",
  "id_category": "id_category",
  "table": "DB Table",
}
```

### DELETE /admin/scopes/:id_scope/entities/:id_entity

It removes an entity from the scope

### POST /admin/scopes/:id_scope/entities/:id_entity/import/csv

It imports data into an entity from a CSV file

**Params**

* options (mandatory): string with json format
  * fields (mandatory): string[]
  * delimiter (mandatory): string
  * hasHeaders (optional): boolean
* file (mandatory): CSV file upload 

**Sample Request**

```json
{
    "options": "{
        "fields": ["id", "field_1", "field_2"], 
        "delimiter": ",",
        "hasHeaders": true
    }",
    "file": <csv_file>
}
```

### POST /admin/scopes/:id_scope/variables

It adds a variable to the scope
```json
{
   "id": "<id_variable>",
   "id_entity": "id_entity",
   "name": "<var_name>",
   "units": "<var_units>",
   "var_thresholds": [],
   "var_agg": ["SUM","MAX","MIN","AVG"],
   "reverse": false,
   "config": {}
 }
```


### PUT /admin/scopes/:id_scope/variables/:id

Edits a scope variable
```json
{
   "id_entity": "id_entity",
   "name": "<var_name>",
   "units": "<var_units>",
   "var_thresholds": [],
   "var_agg": ["SUM","MAX","MIN","AVG"],
   "reverse": false,
   "config": {}
 }
```

### DELETE /admin/scopes/:id_scope/variables/:id
Delete a variable

### GET /admin/scopes/:id_scope/permissions/:id_resource

List of users_ids who are allowed to the provided scope and resource.

```json
[{
  "id": 1,
  "name" : "User name",
  "surname" : "User surname",
  "superadmin" : true
}]
```

### PUT /admin/scopes/:id_scope/permissions/:id_resource

Params:

- add: List of users_ids who will be granted to.
- rm: List of users_ids to be banned.

Sample request.
```json
{
  "add" : [1,2,3,4],
  "rm" : [5,6]
}
```