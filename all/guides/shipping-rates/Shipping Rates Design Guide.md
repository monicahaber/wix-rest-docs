SortOrder: 0
# Shipping Rates

## Questions



##Open issues for Shipping Rates Provider API:

1. Can rate differ depending on identity of origin or destination - i.e. name, email or phone?

1. Calculation of tax rates
    1. Do you return rates inclusive or exclusive of tax? If its inclusive of tax, is there any way to know how much tax is calculated? In a case where tax is calculated, what info do they need on the level of the product item? or is that info held in the app? I guess in such a case the app might be acting as a Catalog SPI at same time.
    
    1. Do we ever want to actually specify that prices returned should include tax or not?

1. in the case where a product might have inventory in more than 1 origin, do we want shipping providers to decide which origin to send from, or is this somethnig that will be managed from within the shipping service, and the provider will be sent a definitive 'origin' for each product. What about in case of FBA, where they completely manage inventory of products and internally move inventory from warehouse to warehouse.....

1. Do we want to send a date-time range on the rate request to the provider? Thinking that a caller might want to communicate 'intent' that want options that are in next 7 days and not beyond, or that they only want delivery from next Wednesday till the following week.

1. Does your API return additional parts of the shipping rate such as insurance or packaging?
