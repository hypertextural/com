# RecycleBin / Pending categories

We'll have to continue using these, there's no way to delete the productions through CSV imports

# Running a CSV import without having to access the web interface

There's no way within BigCommerce to do this.

You could use their REST API to batch upload products, but you've said before you don't want to use their APIs. You could also use some kind of service that takes your CSV and translates it into an API call, but then you're just reliant on an intermediary which you had wanted to avoid.

https://developer.bigcommerce.com/docs/rest-catalog/products#update-products-batch

# Verify Custom Fields as Filters

Yes, you add the custom field to some products, and then you can find it as an option in [Product Filters](https://store-lcsnodqbnd.mybigcommerce.com/manage/products/filters) and make it visible--can be used as filter on listing pages after that as long as there are relevant products on the page.

# CSS Truncation of 'Description' on Grid Listings

We can truncate, we'll use the weird old -webkit- flexbox way. ^sdf

```css
.custom-fields > div {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

This can be added through WebDAV in `content/css/custom.css`.
