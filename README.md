# MIN Tracker - Google Tag Manager Template

A comprehensive tracking solution for Media Intelligence Network (MIN) affiliate marketing campaigns, supporting counter tracking, page view tracking, standard conversion tracking, and shopping cart tracking.

## Features

- **Counter Tracking (Pre-Consent)**: Load counter scripts before user consent for touchpoint counting
- **Advertiser Tracking**: Full tracking container with page view and conversion tracking
- **Standard Conversion Tracking**: Track individual transactions with order details
- **Shopping Cart Tracking**: Track detailed basket information with multiple items
- **Flexible Configuration**: Easy-to-use interface with conditional field display

## Installation

1. Download the `template.tpl` file from this repository
2. Open your Google Tag Manager container
3. Go to **Templates** in the left sidebar
4. Click **New** in the Tag Templates section
5. Click the **⋮** menu in the top right and select **Import**
6. Select the downloaded `template.tpl` file
7. Click **Save**

## Configuration

### Tag Type Selection

Choose between two tag types:

#### 1. Counter (Pre-Consent)
Use this option to load the MIN counter script before user consent. This is typically used for simple touchpoint counting.

**Required Fields:**
- **Counter ID**: Your unique counter identifier

**Example:**
```
Counter ID: 12345
```

This will load: `https://data.min-cdn.net/counter/12345.js`

#### 2. Advertiser Tracking
Complete tracking solution with page view and optional conversion tracking.

**Required Fields:**
- **Advertiser ID**: Your unique advertiser identifier

**Optional Conversion Tracking Types:**
- **No Purchase Tracking**: Only loads the general tracking container (page views)
- **Standard Tracking**: Track single transactions with order details
- **Shopping Cart Tracking**: Track detailed basket information with multiple items
- **Both**: Enable both standard and shopping cart tracking

### Standard Tracking Configuration

When "Standard Tracking" or "Both" is selected:

**Required Fields:**
- **Order Number (token)**: Transaction identifier (e.g., `{{DLV - transaction_id}}`)
- **Revenue (turnover)**: Total transaction value (e.g., `{{DLV - value}}`)

**Optional Fields:**
- **Trigger ID**: Trigger identifier (default: "1")
- **Currency**: Currency code (default: "EUR", e.g., `{{DLV - currency}}`)
- **Description**: Optional transaction description

### Shopping Cart Tracking Configuration

When "Shopping Cart Tracking" or "Both" is selected:

**Required Fields:**
- **Order Token**: Transaction identifier (e.g., `{{DLV - transaction_id}}`)
- **Turnover**: Total transaction value (e.g., `{{DLV - value}}`)

**Cart Items Table:**
Add each product in the cart with the following fields:
- **Article Number**: Product SKU or identifier
- **Quantity**: Number of items
- **Price**: Unit price
- **Product Name**: Product title
- **Category**: Product category
- **Action ID**: Optional action identifier
- **Voucher**: Coupon or voucher code (optional)
- **Currency**: Currency code (default: "EUR")

**Tip:** You can use Data Layer variables to populate the cart items table, e.g., `{{DLV - items}}`

## Usage Examples

### Example 1: Counter Tag (Pre-Consent)
```
Tag Type: Counter (Pre-Consent)
Counter ID: 98765
```

### Example 2: Page View Tracking Only
```
Tag Type: Advertiser Tracking
Advertiser ID: 12345
Type of Purchase Tracking: No Purchase Tracking
```

### Example 3: Standard Conversion Tracking
```
Tag Type: Advertiser Tracking
Advertiser ID: 12345
Type of Purchase Tracking: Standard Tracking
Trigger ID: 1
Order Number: {{DLV - transaction_id}}
Revenue: {{DLV - value}}
Currency: {{DLV - currency}}
```

### Example 4: Shopping Cart Tracking
```
Tag Type: Advertiser Tracking
Advertiser ID: 12345
Type of Purchase Tracking: Shopping Cart Tracking
Order Token: {{DLV - transaction_id}}
Turnover: {{DLV - value}}
Cart Items: {{DLV - items}}
```

## Triggers

### Recommended Trigger Setup

**For Counter Tag:**
- Trigger: All Pages (or Page View - Consent Initialization)
- Should fire before consent is given

**For Advertiser Tracking (Page Views):**
- Trigger: All Pages (after consent)

**For Conversion Tracking:**
- Trigger: Custom Event - Purchase
- Or: Page View on confirmation page (e.g., `/thank-you` or `/order-confirmation`)

## Data Layer Variables

Create the following Data Layer Variables in GTM to make configuration easier:

- `transaction_id` → DLV - transaction_id
- `value` → DLV - value
- `currency` → DLV - currency
- `items` → DLV - items (for shopping cart tracking)

## Permissions

The template requires the following permissions:

- **Inject Scripts**: 
  - `https://mediaintelligence.de/*`
  - `https://min.easystg.de/*`
  - `https://data.min-cdn.net/*`
- **Send Pixels**: 
  - `https://mediaintelligence.de/*`
  - `https://min.easystg.de/*`
- **Access Local Storage**: Read access to `emid` key
- **Access Global Variables**: Execute permissions for:
  - `eamTrckClearBasket`
  - `eamTrckAddBasketItemByAdvertiserId`
  - `eamTrckSubmitBasket`
- **Logging**: All environments (for debugging)

## Validation

The template includes built-in validation:

- **Counter ID**: Must not be empty when Counter tag type is selected
- **Advertiser ID**: Must not be empty when Advertiser Tracking is selected
- **Order Number/Token**: Must not be empty when Standard or Cart tracking is enabled
- **Revenue/Turnover**: Must not be empty when Standard or Cart tracking is enabled

All validation errors are logged to the browser console.

## Troubleshooting

### Tag not firing
1. Check that the trigger is correctly configured
2. Use GTM Preview mode to verify the tag fires
3. Check browser console for error messages

### Data not being sent
1. Verify all required fields are populated
2. Check that Data Layer variables are correctly set
3. Ensure user has given necessary consent
4. Check browser console for validation errors

### Counter not loading
1. Verify Counter ID is correct
2. Check that the counter tag fires before consent
3. Verify network requests in browser developer tools

## Support

For questions or issues with this template, please:
1. Check the troubleshooting section above
2. Review the browser console for error messages
3. Open an issue on this GitHub repository

## License

This template is provided as-is for use with Media Intelligence Network tracking.

## Changelog

### Version 1.0.0
- Initial release
- Counter tracking support
- Advertiser tracking container (eatms.js)
- Standard conversion tracking (etrack)
- Shopping cart tracking (ebasket)
- Field validation
- Conditional field display
