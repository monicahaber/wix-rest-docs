SortOrder: 1
# Shipping Rates SPI

## Subheading
    //
    // This endpoint will be called by the Wix Shipping Service to registered Shipping/Delivery Providers for the matched Shipping Rule
    // based on the requested destination of the items being sent.
    //
    // <h3>Input:</h3>
    //
    // <b>Headers</b>: instance_id, locale
    //
    // <b>Body:</b>
    //
    // 1. Location Information - list of origins and the destination of the itens to be shipped
    //
    // 2. Requested time range - time range in date time for which the returned options should be valid. This is in order to filter out non-relevant options
    //
    // 3. Items - a list of items with full details such as: name, sku, full catalog id, quantity, price details, request
    // Will call with list of origins from which requested items could be shipped, and the list of items
    //
    //4. Pricing Options - options for how the price of shipping rates should be returned, such as currency and whether tax should be included or not
    //
    // <h3>Output:</h3>
    // 
    // List of Rate Options, each with the following fields:
    // 
    // 1. Service - code and details of the service for this shipping option;
    //
    //  2. Delivery Logistics - logistics of how to make the delivery, such as instructions, pickup locations,   //logistics of how delivery will be
    //    DeliveryLogistics logistics = 2;
    //
    //    //cost information for the rates option
    //    ShippingPrice cost = 3;
    //
    // 2.
    //
    // 3.
    //
    // 4.
    //

## Fields That Allow Filtering
