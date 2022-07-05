# Record

Record can be used instead of a class If the purpose is to represent data

    record Customer();
    record class Customer();
    record struct Customer();

    record Customer(string Firstname, string Lastname);
    Customer customer = new("Filip", "Ekberg");

    if(customer is (_, "Ekberg"))
    {
        ApplyDiscount();
    }


    record Customer(string Firstname, string Lastname, IList<Order> Orders);
    Customer customer = new("Filip", "Ekberg", new List<Order>());
    customer.Orders.Add(new()); //    This will work
    customer.Orders = new List<Order>(); //This will not work

    
## Record with Optional Parameters

    public record Order(
        decimal Total = 0,
        ShippingProvider ShippingProvider = default,
        IEnumerable<Item> LineItems = default,
        bool IsReadyForShipment = true)

    Order order = new() 
    {
        LineItems = items
    };

## JSON Property Names

    public record Order(

        [property: JsonPropertyName("total")]
        decimal Total = 0,

        [property: JsonIgnore]
        ShippingProvider ShippingProvider = default,

        [property: JsonIgnore]
        IEnumerable<Item> LineItems = default,

        bool IsReadyForShipment = true);

## Different Types of Records

    record Customer(string firstname, string lastname);

    record struct Customer(string firstname, string lastname);

    readonly record struct Customer(string firstname, string lastname);

## Value Based Equality

    record Customer(string Firstname, string Lastname);

    var first = new Customer("Filip", "Ekberg");
    var second = new Customer("Filip", "Ekberg");

    first == second //Values of each instance is compared and this equality comparison returns true


## Business Logic Should Use Classes

    class OrderProcessor
    {
        public void Process(IEnumerable<Order> orders) { }
    }

##  Combine Classes and Record

    class OrderProcessor
    {
        public void Process(IEnumerable<Order> orders) { }
    }

    record Order(Guid id, IEnumerable<Item> items);

    record Item(Guid id, int quantity, decimal price);

## Use init-only properties for  immutability in classes

    record Order(Guid id, IEnumerable<Item> items)
    {
        protected virtual bool PrintMembers(StringBuilder builder) { ... }
    }

    record CancelledOrder(Guid id, IEnumerable<Item> items) : Order(id, items)


    public void Deconstruct(out string Firstname, out string Lastname)
    {
        Firstname = this.Firstname;
        Lastname = this.Lastname;
    }

## Add optional parameters instead of multiple constructors

    public record Order(

        [property: JsonPropertyName("total")]
        decimal Total = 0,

        [property: JsonIgnore]
        ShippingProvider ShippingProvider = default,

        [property: JsonIgnore]
        IEnumerable<Item> LineItems = default,
        
        bool IsReadyForShipment = true
    );
