---
layout: "ovh"
page_title: "OVH: ovh_vrack"
sidebar_current: "docs-ovh-resource-vrack-x"
description: |-
  Orders a vrack.
---

# ovh_vrack

Orders a vrack.

## Important

~> __WARNING__ This resource is in beta state. Use with caution.

-> __NOTE__ Currently, the OVHcloud API doesn't support Vrack termination. You have to open a support ticket to ask for vrack termination. Otherwise, you may hit vrack quota issues.

## Example Usage

```hcl
data "ovh_order_cart" "mycart" {
  ovh_subsidiary = "fr"
  description    = "my vrack order cart"
}

data "ovh_order_cart_product_plan" "vrack" {
  cart_id        = data.ovh_order_cart.mycart.id
  price_capacity = "renew"
  product        = "vrack"
  plan_code      = "vrack"
}

resource "ovh_vrack" "vrack" {
  ovh_subsidiary = data.ovh_order_cart.mycart.ovh_subsidiary
  name           = "my vrack"
  payment_mean   = "fidelity"
  description    = "my vrack"

  plan {
    duration     = data.ovh_order_cart_product_plan.vrack.selected_price.0.duration
    plan_code    = data.ovh_order_cart_product_plan.vrack.plan_code
    pricing_mode = data.ovh_order_cart_product_plan.vrack.selected_price.0.pricing_mode
  }
}
```

## Argument Reference

The following arguments are supported:
* `description` - yourvrackdescription
* `name` - yourvrackname
* `ovh_subsidiary` - (Required) OVHcloud Subsidiary
* `payment_mean` - (Required) OVHcloud payment mode (One of "default-payment-mean", "fidelity", "ovh-account")
* `plan` - (Required) Product Plan to order
  * `duration` - (Required) duration
  * `plan_code` - (Required) Plan code
  * `pricing_mode` - (Required) Pricing model identifier
  * `catalog_name` - Catalog name
  * `configuration` - (Optional) Representation of a configuration item for personalizing product
    * `label` - (Required) Identifier of the resource
    * `value` - (Required) Path to the resource in API.OVH.COM
* `plan_option` - (Optional) Product Plan to order
  * `duration` - (Required) duration
  * `plan_code` - (Required) Plan code
  * `pricing_mode` - (Required) Pricing model identifier
  * `catalog_name` - Catalog name
  * `configuration` - (Optional) Representation of a configuration item for personalizing product
    * `label` - (Required) Identifier of the resource
    * `value` - (Required) Path to the resource in API.OVH.COM


## Attributes Reference

Id is set to the order Id. In addition, the following attributes are exported:

* `order` - Details about an Order
  * `date` - date
  * `order_id` - order id
  * `expiration_date` - expiration date
  * `details` - Information about a Bill entry
    * `description` - description
    * `order_detail_id` - order detail id
    * `domain` - expiration date
    * `quantity` - quantity
* `service_name` - The internal name of your vrack
