# AngularJS-MVC
AngularJS MVC Repository Dispose



I strongly recommend you to read this article:
https://www.codeproject.com/Articles/869433/AngularJS-MVC-Repository-Dispose

is to clear AngularJS technology and Repository which is one of the useful design pattern in order to developing web applications easy and more professional. There are some fundamental difficulties with the Jquery such as sticking among unstructured code or tangling in deadlocks but AngularJS is able to solve some barriers in Jquery. AngularJS is enough power, specially for CRUD (Create, Read, Update and Delete) web applications, it follows MVC pattern, so our code is neat and clear. Besides it enables us to write less code and testing code has better condition in AngularJS. Albeit these factors do not diminish Jquery power for writing rest of other types of application such as Game or etc.
The other major in this article is to explain repository pattern which encapsulates data access layer and draw a separate line between actual database and other parts of data access layer. It has some advantages for developers for examples; it makes changing and updating data context more convenient and testing frameworks do not run for physical data access code, so this isolation makes it more flexible.
Last but not least is how to debug scripts in web browser for beginners.
I will describe more about each of sections widely, I will open these technologies and pattern within real project in order to having a clear concept from them. 
 
Background
What is Repository
Repository is one of the useful design pattern which hides physical data from the rest of the other sections of data access layers. professionally speaking Repository give us an instruction to how encapsulates our data context from other database queries and data access logic. It is such as a line which isolates actual database from other parts, eventually it makes our web application more convenient to test it layer by layer. Moreover it supplies an in memory like collection http://www.codeproject.com/Articles/832189/List-vs-IEnumerable-vs-IQueryable-vs-ICollection-v for accessing domain objects which ignore persistence and enhance performance. So we prefer to use repository in MVC application to have test ability and speed with the flexibility to future changing.  
 We have two choices for repository structure. One is building one Repository class and IRepository interface for each entity and another way is creating one GenericRepository class for all of entities and defining basic CRUD (Create, Read, Update, Delete) operations and one UnitOfWork class so that all of repository for each entity should be built on UnitOfWork class. Definitely second way is approved when having too much entities. 
As we make for each entity one controller in MVC as a “XController”. We create a folder and name it GenericRepository and make one class “GenericRepository.cs”. Class “GenericRepository.cs” has CRUD functions deal with data context and then create a folder and name it "UnitOfWork" and make one class “UnitOfWork.cs”. you should write inside "UnitOfWork.cs" list of repositories for each entities and moreover "Save()" and "Dispose()" methods. Controllers just have access to UnitOfWork. Therefore you separated your business layer from data layer and whenever you want to change your policy and prefer to use another ORM such as NHibernate you will not have any trouble with high layer such as business layer.
 
Why Repository with Entity Framework:
It is good question that if we use Entity Framework so why we need to establish another repository above Data Context. Because Data Context is repository itself. Image one day you want to use in Data Access Layer another approaches such as NHibernate, ADO.NET or even XML files or suppose you have to change database from sql to oracle, so your trouble is very small and you have to change just a bit in your code instead of changing almost the whole of your structure and write it again. Therefore Repository Pattern enables us to decouple layers properly and comes in handy when we will change our section specially data layer in future. On the other hand testing will be easily.
What is AngularJS
AngularJS is a JavaScript framework for organizing HTML code in a more structural, clear and succinct mode. This client-side technology provides easy and light code in order to make developing and even designing as a simple task. AngularJS eliminates most of redundant code specially in CRUD web application. It is a MV(Whatever you like) pattern and not just a MVC.
There are two kinds of accessories which help us to make a bridge between user interface and the core of business and data layer.
Library: These types are set of functions such as JQuery and whenever you use some of them ($.ajax) it will call related function to handle functions.
Frameworks: These types are specific structure for implementation application and developer have to follow these structure. Such as durandal and ember.
AngularJS has different approach to work out our needs, angular embeds new properties inside HTML tags such as “ng-model”, “ng-repeat” which are known as Directives in order to fade our barriers to match and transform data.
On the other hand AngularJS has mingled Ajax and HTML DOM in a good structure which I will describe it more as follow:
 
Angular Advantages:
It is very good for CRUD web application
It makes testing easy in CRUD web application
You need to write less code
You can run your code more quickly
One of the most popular and famous features in the AngularJS is two way and simple binding
    <input type="text" ng-model="simpleBinding" /><span>{{simpleBinding}}</span>
You do not need to modify HTML DOM
By the aid of directives your tasks are more easy and time saving
By its architecture in MVC or in the better word MVW, decoupling is real and excellent
Design Guidance:
As you see in the above picture, you should download angular files from https://angularjs.org/ and put jquery and angular js files inside script folder. Inside Module.js write name for angular app such as "MyApp" this name relates your angular file to each other and you should use it inside html tag for your view, in this scenario I have used "_Layout.cshtml" as my master view (according to MVC Pattern), in this html tag you should write a directive angular ng-app='MyApp' which hold angular files.
Then you have to introduce these file into your view by <script src=?>.
Index.cshtml inherits from _Layout.cshtml, now by adding another directive as ng-controller='angularCtrl' inside a div, you make a relation between your view and controller.js because angularCtrl is name of Controller.js.
You just need to use simple html tag such as <input type='text'> and add ng-model='empid' directive to this input tag and whenever you want to refer to this input (set or get data) from Controller.js, call it by $Scope.empid .
This same story repeat for <input type='button'> and add ng-click='add()' directive to this input tag and whenever you want to refer to this input (set or get data) from Controller.js  call it by $Scope.add().
For representing data inside table in angular you can use simple table tag by the aid of
ng-repeat="employee in employees" now whenever you want to  (set or get data) from Controller.js  you just need to use:  $Scope.employees and use expressions such as {{employee.ID}} or {{employee.FirstName}} or other fields.
<table>
<tr ng-repeat="employee in employees">
<td>
{{employee.ID}}
</td>
</tr>
</table>
 
If you want to write function inside controller.js you should call ServiceName.FunctionName  such as angularservice.getEmp(), "angularservice" is the name of  Service.js. Now in the "Service.js" you can continue function { "getEmp()" } with $http.get("/ControllerName/ActionName"), or for adding function which you use pass data from view to controller use:
Inside Controller.js to pass data to service.
 var Employee = {
            FirstName: $scope.FirstName ,
            LastName: $scope.LastName ,
            UserName: $scope.UserName
        };
angularService.Add(Employee) 
 
Inside Service.js to pass data to EmployeeController.cs.
this.Add = function (employee) {
            var response = $http({
                method: "post",
                url: "/Employee/Add",
                data: JSON.stringify(employee),
                dataType: "json"
                
            });
            return response;
        }
 
Angularjs vs Jquery
Barriers in the Jquery and Solution in AngularJS:
Jquery Problem: If you want to use multiple entities you are not allowed to use multiple model in one view, therefore you have to write viewmodel to support it.
AngularJS Solution: You can handle it by using $Scope object. This tiny and awesome object can supply data and do changes.
 
Jquery Problem: You have problem to use foreach loop if you are going to use viewmodel, of that you have to use for loop.
AngularJS Solution: You can solve it by using ng-repeat directive inside table {tr tag}
  
Jquery Problem: You should manipulate on HTM DOM and customized your requirement according to your model architecture and writing something like that:                                 Html.EditorFor(model => model.ArticleTitle)
AngularJS Solution: But by the aid of AngularJS you can use <input> and just by adding specific directives, you will reach your goal.


Dispose Pattern
In order to know Dispose pattern, it is necessary to know Garbage Collection.
Garbage Collection
When you create object from data type Common Language Runtime (CRL) assigns specific space inside memory for your new object, in this process Managed Heap helps to CLR to allocate object to memory. Memory space is finite and it is not possible that many objects as consumers use resources. Indeed we need a tool to release memory from objects which do not use memory for long time and they were not called by applications for a while. Garbage Collector is proper tool to manage memory and release space from old and useless objects.
IDisposable is an interface to release resources for particular reclaiming resources whose their life time is well known for programmer. Its advantages are high speed allocation service without long term fragmentation problems.
In the below picture you see three stages, first one objects with these names "A", "B", "C" has taken resources as it is obvious "A" and "B" are dead whereas "C" is live, on the other hand our new objects "D", "E" and "F" are in the queue to use resources. Suppose if there is no tools to stop and release memory or resources so we other objects must wait long in the queue and the performance comes down. Garbage Collection comes here to help us and gather "A" and "B" and takes them from resource due to they are old and no application uses them in long term. So in second stage we have more free space and "D", "E" and "F" will come in third stage. Dispose method or in a better word "Dispose Pattern" helps us to have more performance. Especially when we know life time of the resource (context). 

