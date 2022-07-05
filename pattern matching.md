# Pattern matching

-   Is the type a specific sub-class
-   Does it contain any specific values
-   Does a value conform to a given range

Examples:

    order is ProcessedOrder
    order is ProcessedOrder { IsReadyForShipment: true }
    order is CancelledOrder { Total: <100 }

old stle

    var processedOrder = order as ProcessedOrder;
    if(processedOrder.IsReadyForShipment)
    {
    }

pattern matching

    if(order is ProcessedOrder { IsReadyForShipment: true })
    {
    }



old
    if(order.ShippingProvider.GetType() == typeof(SwedishPostalServiceShippingProvider))
    {
        var provider = order.ShippingProvider as SwedishPostalServiceShippingProvider;
    }

pattern (can use provider directly)

    if(order.ShippingProvider is SwedishPostalServiceShippingProvider provider){
        var nextDayDelivery = provider.DeliverNextDay;
    }

## How to Use Patterns in C#

    // Using the is operator

    if(GetOrder() is ProcessedOrder) {}

    // Using a switch statement

    switch(GetOrder())
    {
        case ProcessedOrder order when order.Total > 100: break;
    }

    // Using a switch expression

    var result = GetOrder() switch {
        ProcessedOrder => ""
    }

## Pattern Matching: Property Pattern

This property pattern is a nested (recursive) pattern

    if(order.ShippingProvider is SwedishPostalServiceShippingProvider { FreightCost: >100 })
    {
    }

## Creating a Switch Expression

    decimal freightCost = order.ShippingProvider switch {
        SwedishPostalServiceShippingProvider { NextDay: true } => 100m,
        _ => 200m
    }

**No Match and No Default Case** fail over

## Pattern Matching: Type Pattern

    ShippingProvider provider = order switch
    {
        PriorityOrder => new GlobalExpressShippingProvider(),
        LowPriortyOrder => new BoatShippingProvider(),
        _ => new()
    };

    var result = instance switch
    {
        string => "This is a string",
        int => "This is an integer",
        null => "This is null",
        _ => "This is everything else that is not null",
    };

## Positional Pattern with Deconstruct

    if(order is (100, true)) { ... }
    public void Deconstruct(out decimal total, out bool ready)
    { ... }

## An Alternative Null Check

    if(order is { })
    {
    }

    if(order is { ShippingProvider.FreightCost: >100 })
    {
    }

    var cost = order.ShippingProvider switch
    {
        SwedishPostalServiceShippingProvider
        { DeliverNextDay: true } provider => provider.Freight + 50m
    }

    if (order is { 
        ShippingProvider: SwedishPostalServiceShippingProvider, 
        Total: >100
    }) 
    { }

## Logical Patterns

    //Negated not
        order is not null

    //Conjunctive and
        order is (total: >50 and <100, _)

    //Disjunctive or

        order is CancelledOrder or ShippedOrder

    if(order is not { Total: <100 }) { ... }
    if(order is not (_, false)) { ... }

### case guards

    var result = order switch
    {
        PriorityOrder when HasAvailability() => ""
        ,
        _ => ""
    };
