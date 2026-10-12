# C# -Dot-Net-Core-key-concepts-and-interview-QnA
C# &amp; Dot Net Core key concepts and interview QnA

# C# Variables
string productName = "Organic Wheat";
decimal price = 120m;
bool isAvailable = true;

The key difference is that C# is statically typed. The type of a variable is checked by the compiler
For prices and financial calculations, C# commonly uses decimal rather than double because decimal arithmetic is better suited to monetary values.

# C# Methods
static decimal CalculateTotal(decimal price, int quantity)
{
    return price * quantity;
}
Notice the return type (decimal) and parameter types (decimal, int). C# methods specify these types explicitly.

--------------------------------------------------
Classes and objects
public class Product
{
    public int Id { get; set; };
    public string Name { get; set; } = string.Empty;
    public decimal Price { get; set; }
}


--------------------------------------------------
Create an object:
The class represents the structure of a product in our marketplace. Later, we'll use similar classes as the basis for database entities and API models, while keeping those responsibilities separate when appropriate.

Product product = new Product
{
    Id = 1,
    Name = "Organic Wheat",
    Price = 120m
};
Console.WriteLine(product.Name);
Here:
- Product is the class.
- product is an object created from that class.
- Id, Name, and Price are properties.
- { get; set; } defines a property with a getter and setter.
- 120m is a decimal literal.
--------------------------------------------------

Q. Why use decimal instead of double for a product price?
Decimal represents base-10 fractions more appropriately for financial calculations. Double uses binary floating-point and can introduce rounding differences. Decimal is not a substitute for appropriate currency rounding rules.

Q. What is a class, and what is an object?
A class defines a type and its members. An object is an instance of that class with its own state.

Q. What is the difference between a method parameter and a return type?
Parameters are the inputs accepted by a method. The return type specifies the kind of value the method returns; void means it returns no value.


Q. What is an interface in C#?
An interface defines a contract: it specifies which operations a class must provide, without defining the implementation of those operations.

Q. Dependency injection (DI): a must-know interview topic
Dependency injection means that a class receives the objects it depends on instead of creating those objects itself.
Compare these two approaches:
Approach A — tightly coupled
ProductService service = new ProductService();


The calling code decides which concrete service to create.

Approach B — dependency injection
public class ProductsController
{
    private readonly IProductService _service;

    public ProductsController(IProductService service)
    {
        _service = service;
    }
}


ASP.NET Core supplies an implementation of IProductService when it creates the controller.


The second approach makes the controller depend on the interface rather than directly creating ProductService.

# Common service lifetimes you'll need to know:
Lifetime	Meaning	Typical use
Transient	A new instance each time it's requested	Lightweight, stateless services
Scoped	One instance per HTTP request	Business services and EF Core DbContext
Singleton	One instance for the application's lifetime	Shared, thread-safe services
Interview warning: Never register a service as a singleton if it depends on a scoped service such as the standard Entity Framework Core DbContext. That lifetime mismatch can cause runtime errors or incorrect behavior.

# Understand the project files
After creating the project, your main files will look similar to this:
FarmConnect.Api

Program.cs
Configures services and the HTTP request pipeline.

Controllers
Receives HTTP requests and returns HTTP responses.\

Models
Defines the structure of our product data.

Services
Contains the product operations and business logic.






# understand code line by line.
Code	What it does
WebApplication.CreateBuilder(args)	Creates the application builder and loads configuration.
builder.Services.AddControllers()	Registers controller-related services.
AddSingleton<IProductService, ProductService>()	Registers the implementation that ASP.NET Core will inject when IProductService is requested.
builder.Build()	Builds the configured application.
UseHttpsRedirection()	Redirects HTTP requests to HTTPS where configured.
UseAuthorization()	Adds authorization middleware to the pipeline. It does not, by itself, authenticate users.
MapControllers()	Maps controller routes to HTTP endpoints.
Run()	Starts the application.




# Common errors and how to troubleshoot them
Error	What to check
404 Not Found	Check the URL and controller route. A missing product ID intentionally returns 404.
500 Internal Server Error	Check the Visual Studio Output window and exception details.
Connection refused	Make sure the API is running and the port is correct.
HTTPS certificate error	Check the local development certificate and use the correct HTTPS URL.
CS0246 type or namespace not found	Check using statements, namespaces, and project references.
DI error saying a service cannot be resolved	Confirm AddSingleton<IProductService, ProductService>() exists and the interface and implementation are public.
POST returns 400 Bad Request	Check the request JSON, content type, and any model-validation rules.

# What is ASP.NET Core Web API?
It is a framework for building HTTP APIs that expose application functionality to clients such as Angular, mobile apps, and other services.

# What is the difference between GET and POST?
GET retrieves a resource and should not change application state. POST submits data or requests a state-changing operation, such as creating a product.

# What is the purpose of ControllerBase?
It provides common API controller functionality, including HTTP response helpers such as Ok(), NotFound(), and CreatedAtAction().

# What is routing in ASP.NET Core?
Routing matches an incoming request's URL and HTTP method to an endpoint. In our example, attributes define the product routes.


# What is dependency injection?
It is a technique where a class receives its dependencies from an external provider. ASP.NET Core has a built-in DI container that creates and supplies registered services.

# Why did we use Singleton for ProductService?
The service holds an in-memory collection that must survive across requests. Singleton keeps the same instance across requests in this demo. In the database-backed application, we'll normally use a scoped service with a scoped DbContext.

# What is the difference between 200, 201, 400, and 404?
200 means the request succeeded; 201 means a resource was created; 400 means the request is invalid; and 404 means the requested resource or endpoint wasn't found.

# Learn LINQ: search products like a real application
Imagine a customer wants to search for wheat products. We shouldn't fetch every product and ask Angular to do all the filtering, especially when the database eventually contains thousands of records.
LINQ provides methods to query collections and, through Entity Framework Core, database data.
For now, we'll query our in-memory list.

# What is model validation in ASP.NET Core?
Model validation checks incoming data against rules such as Required, StringLength, and Range. With [ApiController], invalid model state normally produces an automatic 400 Bad Request response.

# What is LINQ?
LINQ stands for Language Integrated Query. It provides a consistent way to filter, project, sort, and query collections and supported data sources using methods such as Where, Select, OrderBy, and FirstOrDefault.

# What is the difference between IEnumerable and IQueryable?
IEnumerable represents a sequence that can be enumerated. IQueryable represents a query that a provider can translate and execute, often in a database. With EF Core, query composition before materialization can allow SQL Server to perform filtering. Neither interface alone guarantees that a query runs in the database; the provider and when the query is executed matter.

# Should validation happen only in Angular?
No. Frontend validation improves user experience, but it can be bypassed. The backend must validate incoming requests and enforce business rules and authorization.

# What is the difference between FirstOrDefault and First?
First returns the first matching element and throws if there is no element. FirstOrDefault returns the default value when no element is found, which is null for reference types such as Product.
