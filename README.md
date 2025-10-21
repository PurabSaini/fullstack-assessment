# Stackline Full Stack Assignment

## 1. Bug: Incorrect Subcategory Filtering

   Description:
   When a category is selected, the website displays all subcategories instead of only those belonging to the selected category.

   Cause:
   The fetch request for subcategories does not include a category parameter specifying the selected category, resulting in an unfiltered response.

   Fix:
   Updated the fetch request to include the selected category as a query parameter. This ensures that getSubCategories retrieves only subcategories related to the selected category instead of receiving an unfiltered or undefined response.

## 2. Bug: Image Rendering Error

   Description: 
   When rendering images with the hostname 'images-na.ssl-images-amazon.com'
   the following error appears:

   Error: Invalid src prop (https://images-na.ssl-images-amazon.com/images/I/81ZSuzkKKHL._AC_SL1500_.jpg) on `next/image`, hostname "images-na.ssl-images-amazon.com" is not configured under images in your `next.config.js`.

   This error prevents product images with the above hostname from being displayed on the website.

   Cause:
   Next.js restricts external images to only those from hostnames explicitly allowed in the next.config.js file. The reason for this is so that only images from specific, trusted domains are loaded. The hostname images-na.ssl-images-amazon.com was not included in the images.remotePatterns configuration, causing the next/image component to reject those image sources as invalid/untrustworthy.

   Fix:
   Added the missing Amazon hostname to the remotePatterns array in next.config.js to allow Next.js to render these images. This ensures that images with the above hostname are recognized as valid sources and rendered correctly. 

## 3. Bug: Clear Filters Not Updating Dropdown Display for Categories
   
   Description: 
   When selecting a category and then clicking “Clear Filters”, the selected category resets internally but the dropdown still displays the previously selected category instead of reverting to the placeholder “All Categories.”

   Cause:
   The issue occurs because the Select component expects a controlled value. When selectedCategory is set to undefined, the Select component does not interpret this as an empty value and therefore just keeps the previously selected category.
   In other words, undefined did not correspond to any valid option, so the dropdown remained stuck showing the previously selected label.

   Fix:
   By setting the value of the Select component for the dropdown to an empty string ("") when no category is selected, the Select component correctly interprets this as the absence of a selection and reverts to displaying the placeholder “All Categories.” This ensures the dropdown visually resets when the page re-renders after the user clicks “Clear Filters.”

## 4. Enhancement: Redundant Subcategory Selection

   Description:
   When certain categories have only one subcategory, users can still open the subcategory dropdown and manually select it — even though it’s the only option available. This creates unnecessary steps for the user since the products displayed would not change at all.

   Cause:
   The logic that handles subcategory rendering does not check the number of available subcategories. Therefore, even when there’s only one subcategory, the UI still displays the dropdown as if multiple options exist.
   This leads to redundant interaction, since selecting the single available option has no functional impact — it’s already implicitly selected.

   Improvement:
   Added a condition to render the subcategory dropdown if the selected category contains more than one subcategory. As a result, users are not given redundant options
   when only one subcategory exists.

## 5. Bug: Search Bar Not Resetting When Category Changes

   Description:
   When a user searches for items within one category and then switches to another, the previous search term remains in the search bar.
   For example: Searching for a printer in “3D Printers & Supplies” and then selecting “Chocolate” still filters based on the printer keyword, resulting in unexpected or empty results.

   Cause:
   The search state variable is not reset when the selectedCategory changes. As a result, both the new category filter and the previous search query are active at the same time.
   Since the query is scoped to the new category but still contains the old search term, it may return irrelevant or no results.

   Fix:
   Set the search input state to an empty string ("") when the category is updated. This ensures consistent and predictable filtering behavior, as the previous search query is cleared when a new category is selected.

## 6. Bug: Singular vs. Plural Product Label

   Description:
   When only one product is displayed, the website incorrectly displayed 
   “Showing 1 products”

   Cause:
   The display text for the product count was hardcoded to always use the plural form “products”, without checking the count value. There was no conditional logic to adjust for singular vs. plural cases.

   Fix:
   Added conditional rendering logic to check the product count and display:

   “product” when products.length = 1

   “products” when product.lengths != 1

   This ensures proper grammar depending on the number of products displayed.
