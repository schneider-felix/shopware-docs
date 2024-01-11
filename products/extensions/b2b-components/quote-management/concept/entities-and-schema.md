---
nav:
  title: Entities & Schema
  position: 10

---

# Entities and schema

## Entities

### Quote

The quote entity stores fundamental information about each quote such as unique identifiers, state, pricing, discount, associated users, customer, and order details.

### Quote Delivery

The quote delivery represents the delivery information of a quote. It has a shipping method, earliest and latest shipping date, and shipping costs.

### Quote Delivery Position

The quote delivery position represents the line items of a quote delivery. It has a quote line item, price, total price, unit price, quantity, and custom fields.

### Quote Line item

The quote line item represents the line items of a quote. Only product type is supported currently

### Quote Transaction

The quote payment capture, it has an amount, allows saving an external reference.

## Schema

```mermaid
erDiagram
    // Entities
    entity "quote" {
        + id [PK]
        version_id [PK]
        auto_increment
        state_id [FK]
        user_id
        currency_id [FK]
        language_id [FK]
        sales_channel_id [FK]
        created_by_id
        updated_by_id
        customer_id [FK]
        order_id
        order_version_id
        quote_number
        expiration_date
        price
        shipping_costs
        discount
        item_rounding
        total_rounding
        tax_status
        amount_total
        amount_net
        subtotal_net
        total_discount
        original_price
    }

    entity "quote_delivery" {
        + id [PK]
        version_id [PK]
        quote_id [FK]
        quote_version_id [FK]
        shipping_method_id [FK]
        shipping_date_earliest
        shipping_date_latest
        shipping_costs
        custom_fields
    }

    entity "quote_delivery_position" {
        + id [PK]
        version_id [PK]
        quote_delivery_id [FK]
        quote_delivery_version_id [FK]
        quote_line_item_id [FK]
        quote_line_item_version_id [FK]
        price
        total_price
        unit_price
        quantity
        custom_fields
        created_at
        updated_at
    }

    entity "quote_line_item" {
        + id [PK]
        version_id [PK]
        quote_id [FK]
        quote_version_id [FK]
        parent_id [FK]
        parent_version_id
        product_id
        product_version_id [FK]
        promotion_id
        cover_id [FK]
        states
        label
        referenced_id
        identifier
        description
        quantity
        type
        payload
        unit_price
        total_price
        price_definition
        price
        discount
        stackable
        removable
        good
        position
    }

    entity "quote_transaction" {
        + id [PK]
        version_id [PK]
        quote_id [FK]
        quote_version_id [FK]
        payment_method_id [FK]
        amount
        custom_fields
    }

    // Relationships
    quote_delivery ||--o{ quote : "quote_id, quote_version_id"
    quote_delivery_position ||--o{ quote_delivery : "quote_delivery_id, quote_delivery_version_id"
    quote_line_item ||--o{ quote : "quote_id, quote_version_id"
    quote_line_item ||--o{ quote_line_item : "parent_id, parent_version_id"
    quote_transaction ||--o{ quote : "quote_id, quote_version_id"
```
