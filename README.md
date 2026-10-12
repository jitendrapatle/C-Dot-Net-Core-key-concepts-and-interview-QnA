# C-Dot-Net-Core-key-concepts-and-interview-QnA
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
