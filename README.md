# UI grid with pagination and sorting
A simple grid built in angular js with pagination and sort that gets the data from the server on every pagination or sort request



# Dependencies

 

 - Bootstrap 
 -  Angularjs 
 - Server to handle get request and respond with data

The angular-grid.js file contains a module named **yangular-grid** that can be defined as a dependency in any angular project where the grid is to be used




#Prerequisite
A server which handles get calls and responds with correct data.
The endpoint should define 4 query string parameters (whose names are configurable) namely

 1. **sort** - The parameter on which sorting is applied. This parameter name should be the as the name of the column in your db or name of the key in case of json. 
 *[configurable]*
 2. **order**- This parameter can take two values namely 'asc' and 'desc'. 
 *[configurable]*
 3. **page** - This parameter defines the page current number. 
 *[configurable]*
 4. **size** - This parameter defines the number of items currently being displayed in the grid.  
 *[configurable]*

All the above parameters can be used by the server to generate appropriate data and pass onto the client as a json object. 

The server should respond with the following data

 1. **rows** -  array of data
 2. **page** - Current page
 3. **count** - Total number of records
 4. **size** - Size being sent by the client

The sample structure of the response should be

    {
    	rows : [],
    	count : 24,
    	page:1,
    	size:10
    }

The **rows** property name in the response is configurable and the server can send data in any other property name like `tickets` or `users`.
Other properties names should have the same name as defined above.



#Usage


The grid can be used like

    <yag config="config"></yag>

#Configuration

A config object needs to be assigned to the grid to set up things.
Following are the configurations

 - **url** - This should have the api end point url from which data needs to be loaded. 
 *NOTE - In case the API url is not in the same domain as the hosted app, CORS needs to be enabled by setting proper headers in the server response*
 - **sortQuerystringParam** - Name of the query string **sort** parameter being used at server side
 -  **orderQuerystringParam** - Name of the query string **order** parameter being used at server side
 -  **pageQuerystringParam** - Name of the query string **page** parameter being used at server side
 -  **sizeQuerystringParam** - Name of the query string **size** parameter being used at server side
 - **columns** - array of object which contains details about the columns of the grid.
 Each object should have 4 properties namely `key`as the name of the column in db/json and `name`as the name to be displayed in the HTML for that column, `cssClass` as the css class for the column and `render` as a call back method that has access to the data returned from the server which can be used to modify the data before it is rendered
 - **objectName** - The property name in which the server sends the actual data in response. This was set to **rows** in the above example.
 - - **onRowClick** - A callback method that has access to the row properties and can be used to add click events to each row.

#Sample Configuration

Sample url of the api end point - 
`http://localhost:3000/tickets/get?sort=id&order=asc&page=1&size=10`


    var renderDate = function(date){
    return new Date(Date.parse(date)).toLocaleDateString();
    };
    var renderDescription = function(desc){
    return desc ? String(desc).replace(/<[^>]+>/gm, '') : '';
    };
    var renderAssignee = function(assignee){
    if(assignee === null || typeof assignee === 'undefined')
    return 'Unassigned'
    };
    $scope.config = {};
    $scope.config.url = '/tickets/get';
    $scope.config.columns = [];
    $scope.config.columns.push({key : 'id', name : 'ID', cssClass:"col-md-1"});
    $scope.config.columns.push({key : 'title', name : 'Title', cssClass: "col-md-2"});
    $scope.config.columns.push({key : 'description', name : 'Description', cssClass: "col-md-2", render : renderDescription});
    $scope.config.columns.push({key : 'type', name : 'Type', cssClass: "col-md-1"});
    $scope.config.columns.push({key : 'assignee', name : 'Assignee', cssClass: "col-md-2", render: renderAssignee});
    $scope.config.columns.push({key : 'createdDate', name : 'Created Date', cssClass: "col-md-2", render : renderDate});
    $scope.config.columns.push({key : 'createdBy', name : 'Created By', cssClass: "col-md-2"});

    $scope.config.sortQuerystringParam = 'sort';
    $scope.config.orderQuerystringParam = 'order';
    $scope.config.pageQuerystringParam = 'page';
    $scope.config.sizeQuerystringParam = 'size';

    $scope.config.onRowClick = function(row){
    $location.path('ticket/view/' + row.id);
    };

    $scope.config.objectName = 'tickets';


![Alt text](/screenshots/sample_screenshot1.PNG?raw=true "Screen Shot 1")

![Alt text](/screenshots/sample_screenshot2.PNG?raw=true "Screen Shot 2")
