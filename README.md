# Task Organizer

```scala
╔╦╗┌─┐┌─┐┬┌─  ╔═╗┬─┐┌─┐┌─┐┌┐┌┬┌─┐┌─┐┬─┐
 ║ ├─┤└─┐├┴┐  ║ ║├┬┘│ ┬├─┤││││┌─┘├┤ ├┬┘
 ╩ ┴ ┴└─┘┴ ┴  ╚═╝┴└─└─┘┴ ┴┘└┘┴└─┘└─┘┴└─
             -a crossbeam' task organizing app..
```

##### A fun RESTFul pplication which helps us to organize our tasks based on its dependencies.

##### Rationale
    We are given with a list of tasks, some of which depend on the others. So whenever we provide a subset of those tasks to the application, 
    the application should helps us to organize the tasks by returning an ordered list of all the  dependents tasks to run in order to achieve 
    our tasks.        

Let's say we have the following set of tasks available in the system
```json
        [
            {
                "task": "make a sandwich",
                "depends": [
                    "buy groceries"
                ]
            },
            {
                "task": "buy groceries",
                "depends": [
                    "go to the store"
                ]
            },
            {
                "task": "go to the store",
                "depends": []
            }
        ]
```
When the user provides input **["make a sandwich"]** to the application, it should return us
```json 
[ 'go to the store', 'buy groceries', 'make a sandwich' ]
```

The application should also return 
```json
[ 'go to the store', 'buy groceries', 'make a sandwich' ]
```
When the user provides input **["buy groceries" "make a sandwich"]**

##### Application Module
Application contains 3 parts (or 3 module/sibilant files), 
    1. **core** - where all the core functions & algorithms exists.
    2. **server** - where all the server features exists. This module contains all the functions to bootstrap a simple restful server and the server runs using port **8001**. 
    3. **test** - where all the tests exists.  

##### Algorithm
This application uses [**DFS(Depth First Search)**](https://en.wikipedia.org/wiki/Depth-first_search) as a core algorithm to achieve its main functionality, **DFS** is one of the most common algorithms used for traversing or searching the graph nodes or tree nodes. 
In our case the application iterates all the given sub-tasks, and for each tasks it recursively looks for any dependent tasks exists, until any of the task doesn't contains depends or empty depends. Also while backtracking its starts collecting all the tasks in a proper order.
Apart from **DFS** the application uses **memoization** technique to avoid repetitive traversing on the same tasks by storing the results of already traversed tasks in to a **map**. So that application checks for the tasks existence before traversing its dependents, if the task is already traversed and exists in the **map**, the application reads the result from the **map** instead of traversing it again. This way we achieve some performance improvements. 

##### Apis
The application comes with following apis/endpoints to interact with it.

| uri  | method | description| payload |
|------|--------|------------|---------|
|/tasks| GET    | returns all the tasks||
|determine-order|POST| accepts: sublist of tasks <br/> returns: ordered list of all the dependent tasks.| Examples: <br/> ["make a sandwich"] <br/> ["make a sandwich", "eat sandwich"]





##### Build instruction
Make sure you have sibilant installed in your machine. If not check [here](https://sibilant.org/)

###### Prerequiste
Before running the application make sure all tasks updated in the **data.json**. This file serves as the database (data source) to our application.

1. To run 
```shell
sibilant -x -f server.sibilant
```
Once the application is up & running, call the apis as below.

a. GET /tasks
```curl
curl -X GET http://localhost:8001/tasks
```
b. POST /tasks
```curl
curl -X POST http://localhost:8001/determine-order \
     -d '["make a sandwich"]'
```



2. To run tests
- sbt
```shell
sibilant -x -f test.sibilant
```
