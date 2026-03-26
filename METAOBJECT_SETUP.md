# Product Testimonial Metaobject Setup Guide

This guide shows how to set up metaobjects for product testimonials in Shopify admin.

## Step 1: Create Metaobject Definition

Navigate to: **Shopify Admin > Settings > Custom data > Metaobjects**

### Metaobject Definition: `product_testimonial`

```json
{
  "name": "Product Testimonial",
  "type": "product_testimonial",
  "fields": [
    {
      "name": "Customer Image",
      "key": "customer_image",
      "type": "file_reference",
      "validations": {
        "file_type_options": ["image"]
      }
    },
    {
      "name": "Customer Name",
      "key": "customer_name",
      "type": "single_line_text_field",
      "required": true
    },
    {
      "name": "Testimonial Text",
      "key": "testimonial_text",
      "type": "multi_line_text_field",
      "required": true
    },
    {
      "name": "Star Rating",
      "key": "star_rating",
      "type": "number_integer",
      "validations": {
        "min": 1,
        "max": 5
      },
      "required": true
    }
  ],
  "fieldsets": []
}
```

## Step 2: Create Product Metafield Definition

Navigate to: **Shopify Admin > Settings > Custom data > Products**

### Product Metafield: `custom.product_testimonial`

```json
{
  "namespace": "custom",
  "key": "product_testimonial",
  "name": "product_testimonial",
  "description": "Customer testimonials for this product",
  "type": "list.metaobject_reference",
  "validations": {
    "metaobject_definition_id": "product_testimonial"
  }
}
```

## Step 3: Sample Testimonial Entries

Here are examples of how to fill in testimonials:

### Example 1: 5-Star Review

```
Customer Image: customer-1.jpg (uploaded file)
Customer Name: Sarah Johnson
Testimonial Text: This product exceeded my expectations! The quality is outstanding and it arrived faster than expected. Highly recommend to anyone looking for a reliable solution.
Star Rating: 5
```

### Example 2: 4-Star Review

```
Customer Image: customer-2.jpg
Customer Name: Michael Chen
Testimonial Text: Great product overall. Does exactly what it promises. Only reason for 4 stars instead of 5 is the price point, but you get what you pay for in terms of quality.
Star Rating: 4
```

### Example 3: 5-Star Review (No Image)

```
Customer Image: (leave blank)
Customer Name: Emma Rodriguez
Testimonial Text: Absolutely love this! I've been using it for 3 months now and it's been life-changing. Customer service was also incredibly helpful when I had questions.
Star Rating: 5
```

## Step 4: Assign Testimonials to Products

1. Go to **Products > [Select a Product]**
2. Scroll to **Metafields** section
3. Find **product_testimonial** field
4. Click **Add product_testimonial**
5. Select or create testimonials from the list
6. Save product

## Sample Product with Testimonials

### Product: "Premium Yoga Mat"

**Assigned Testimonials (3 total):**

**Testimonial 1:**
- Customer Image: `files/testimonials/lisa-m.jpg`
- Customer Name: Lisa Martinez
- Testimonial Text: "Best yoga mat I've ever owned! The grip is amazing even during hot yoga sessions, and it's so comfortable on my knees. Worth every penny!"
- Star Rating: 5

**Testimonial 2:**
- Customer Image: `files/testimonials/james-k.jpg`
- Customer Name: James Kim
- Testimonial Text: "I bought this mat 6 months ago and it still looks brand new. The durability is impressive, and I love that it's eco-friendly too."
- Star Rating: 5

**Testimonial 3:**
- Customer Image: `files/testimonials/amanda-s.jpg`
- Customer Name: Amanda Smith
- Testimonial Text: "Great mat for the price. Very comfortable and easy to clean. My only minor complaint is that it's a bit heavier than I expected, but that also makes it feel premium."
- Star Rating: 4

## How It Appears on Product Page

When you add the **Product Testimonial** block to a product page:

1. If product has 0 testimonials: Block won't render
2. If product has 1 testimonial: Shows the testimonial (no navigation arrows)
3. If product has 2-10 testimonials: Shows carousel with prev/next arrows and dot indicators

### Visual Layout:

```
┌─────────────────────────────────────────┐
│          Customer Reviews               │ ← Heading (optional)
├─────────────────────────────────────────┤
│                                         │
│  ←  ⭐⭐⭐⭐⭐                         →  │ ← Nav arrows
│                                         │
│     "This product exceeded my           │
│      expectations! The quality is       │
│      outstanding..."                    │ ← Testimonial text
│                                         │
│      👤  Sarah Johnson                  │ ← Customer info
│                                         │
├─────────────────────────────────────────┤
│          ● ○ ○                          │ ← Dot indicators
└─────────────────────────────────────────┘
```

## Tips

1. **Image Dimensions**: Upload customer images at least 160x160px for best quality (displayed at 50px on desktop, 40px on mobile)
2. **Testimonial Length**: Keep testimonials between 50-200 characters for optimal display
3. **Star Ratings**: Use 4-5 stars for featured testimonials, can include 3-star reviews for authenticity
4. **Customer Names**: Use first name + last initial for privacy (e.g., "Sarah J.") or full names with permission
5. **Sorting**: Testimonials appear in the order you add them to the product
6. **Mobile**: Users can swipe left/right to navigate testimonials on mobile devices

## Accessing Metafields Programmatically

If you need to access testimonials in Liquid:

```liquid
{% assign testimonials = product.metafields.custom.product_testimonial.value %}

{% for testimonial in testimonials %}
  Name: {{ testimonial.customer_name }}
  Text: {{ testimonial.testimonial_text }}
  Rating: {{ testimonial.star_rating }}
  Image: {{ testimonial.customer_image | image_url: width: 100 }}
{% endfor %}
```

## Troubleshooting

**Block not showing:**
- Verify product has testimonials assigned
- Check metafield namespace is `custom` and key is `product_testimonial`
- Ensure metaobject type is exactly `product_testimonial`

**Images not loading:**
- Confirm images are uploaded to Files section
- Check file reference field is linked correctly
- Images are optional - block will still render without them

**Stars not displaying:**
- Ensure Lucide icons are loaded in theme
- Check star_rating is between 1-5
- Verify JavaScript is not blocked
