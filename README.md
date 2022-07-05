# LInq2022

# Tuples and Deconstruct

-   Tuple : “The tuples feature provides concise syntax to group multiple data elements in a lightweight data structure“


        var group = (order.OrderNumber, order.LineItems);

-   Tuple vs Anonymous Type

| Tuple | Anonymous Type |
| --- | ----------- |
| Value type | Reference type |
| Values stored in fields | Values stored in properties with backing fields |    
| Not immutable  | Read-only |    

-   Tuple field names only tracked by the compiler
-   Tuples are not read-only


## Creating and Using a Tuple

    // Tuple assignment
    var summary = (order.OrderNumber, order.Items, Sum: GetSumFor(order))
    
    reference above: ValueTuple<Guid, IEnumerable<Item>, decimal>

    (Guid, IEnumerable<Item>, decimal) summary = (order.OrderNumber, order.Items, Sum: GetSumFor(order));

## Tuple Assignment

    var summary = (order.OrderNumber, order.Items, Sum: GetSumFor(order))
    Guid, IEnumerable<Item>, decimal) summary = (OrderNumber, Items, Sum);

**The order of the elements is important!**

**The right hand side is assigned into the corresponding position on the left**

    Guid orderNumber;
    decimal sum;
    (orderNumber, var items, sum) = (OrderNumber, Items, Sum)

    var group = new { order.OrderNumber, order.Total, Sum = order.Sum };
    var group = (order.OrderNumber, order.Total, Sum: order.Sum)

    public (Guid, int, decimal, IEnumerable<Item>) Process(...)
    {
        return (order.OrderNumber, amountOfItems, GetTotalFor(order), items);
    }

    if(order is (total: > 100, ready: true))
    {
    }

    var order = new Order();
    var (total, isReady) = order;

## Introducing the Deconstruct Method

    public void Deconstruct(out decimal total, out bool ready)
    {
        total = Total;
        ready = IsReadyForShipment;
    }

You can have multiple deconstruct methods They need to have a different number of parameters



-   A sequence of elemenets

        var summary = (orderNumber, total, items)

-   Support named parameters

        (id: GetId(), total: GetTotal(), items: GetItems())

-   Compiles to a generic struct

        System.ValueTuple<T1,T2,T3,...>

-   Discard

    (orderNumber, items, _) summary = Process();
