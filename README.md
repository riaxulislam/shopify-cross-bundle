# Shopify Bundle Selector Section README

This is a custom Liquid section for Shopify themes that creates an interactive product bundle builder. It allows customers to select bundle sizes (e.g., 3-pack +1 free, 5-pack +2 free, or full pack), choose individual products from a grid, track progress via a progress bar, and add the bundle to the cart with optional subscription support via app widgets (e.g., Recharge or similar subscription apps). 

The section supports dynamic pricing (one-time or subscription), free product marking in cart properties, and customization via the theme editor. It integrates with Shopify's cart API and handles subscriptions without duplicate adds.

**Key Features:**
- Bundle tabs for different sizes.
- Product grid (3 per row) with quantity controls and flavor info popups.
- Progress bar (now radio-style circles with dynamic connector fill).
- Sticky bottom bar (mobile-friendly).
- Subscription widget isolation to prevent conflicts.
- Add-to-cart with line item properties (e.g., "Item 1: Product Title (Free)").
- Full color customization in theme editor.
- Responsive design (desktop, tablet, mobile).
- Auto-selection on bundle switch and progress-based auto-bundle switching.

**Limitations:**
- Assumes the main product has variants matching bundle names (e.g., "3 Pack", "5 Pack", "All Pack"). If not found, falls back to first variants.
- Supports up to 12 products (configurable via blocks).
- Subscription relies on app widgets (e.g., `@app` blocks) – ensure a subscription app is installed.
- Tested on Shopify Online Store 2.0 themes.

## Installation and Setup

### Step 1: Add the Section File
1. Log in to your Shopify admin: **Online Store > Themes > Actions (on your theme) > Edit code**.
2. In the **Sections** folder, click **Add a new section**.
3. Name it `bundle-selector.liquid` (or similar).
4. Paste the entire provided code (HTML, CSS, JS, Schema) into the file.
5. Save.

### Step 2: Prepare Your Product
- This section is designed for a **bundle product page** (e.g., a "Mix & Match Bundle" product).
- Ensure the product has **variants**:
  - Variant 1 title: Match `bundle_name_1` (default: "3 Pack").
  - Variant 2: "5 Pack".
  - Variant 3: "All Pack".
- If using auto-prices (`use_auto_prices` enabled):
  - Set prices on variants.
  - For subscriptions: Use metafields (e.g., `metafields.subscriptions.discount_price`) or app-specific setup.
- Add **12 or fewer related products** as blocks (see Step 4).
- Optional: Install a subscription app (e.g., Recharge) and add its widget via an `@app` block.

### Step 3: Add to a Product Template
1. Go to **Online Store > Themes > Customize**.
2. Select a product page (or create a custom template: **Products > Default product > Create template** named e.g., "Bundle").
3. In the template JSON (Edit code > Sections > product-template.liquid or similar), add:
   ```
   {% section 'bundle-selector' %}
   ```
   Place it where you want the bundle UI (e.g., below add-to-cart or replace default form).
4. If using a custom template:
   - In **Products > Your Product > Template**, select the new "Bundle" template.

### Step 4: Configure in Theme Editor
1. Open the theme customizer on a bundle product page.
2. Click the new "Bundle Selector" section.
3. **Product Details Section**:
   - Toggle show/hide.
   - Custom title, reviews HTML (e.g., embed Judge.me or Yotpo), bullet points (pipe-separated).
4. **Bundle Configuration**:
   - Set titles, limits (paid products), tab text.
   - Toggle bundles and "Best Deal" badges.
   - Manual prices if disabling auto-prices.
5. **Add Products**:
   - In section blocks, add "Product" blocks (up to 12).
   - Select a product, optional custom image, flavor info (for popup).
6. **Add Subscription Widget**:
   - Add an `@app` block from your subscription app (e.g., Recharge widget).
   - It will render isolated in the section.
7. **Colors**:
   - Customize all via headers (e.g., Progress Bar Colors, Tabs, etc.).
   - Changes apply live.
8. **Progress Bar Customization** (Updated Feature):
   - Uses radio-style circles (no numbers, filled dots).
   - Connector line fills dynamically.
   - New settings added: `progress_circle_border_color`, etc. (if not in schema, add them – see code updates in previous response).
8. Save and preview on a product page.

### Step 5: Testing
1. Visit the bundle product page.
2. **Select Bundle**: Click tabs – auto-selects products, updates price.
3. **Add/Remove Products**: Click "Select", adjust qty – progress updates, messages show (e.g., "Add 1 more for free").
4. **Subscription**: Select via widget – price syncs, adds selling_plan to cart.
5. **Add to Cart**:
   - Enables when limit met.
   - Adds single bundle variant with properties (check cart: properties show items/free).
   - Redirects to /cart.
6. **Cart Validation**:
   - In cart, bundle shows as one line item with properties.
   - Subscription: Price/discount applies if plan selected.
7. **Edge Cases**:
   - Max 12 products.
   - Mobile: Sticky bar, 2-column grid.
   - Errors: Console log for debug (e.g., variant not found).

### Troubleshooting
- **Variant Not Found**: Ensure variant titles match settings. Check fallback logic in code.
- **Subscription Not Working**: 
  - App widget must use selectors like `input[name^="purchaseOption_"]`, `data-radio-type="selling_plan"`.
  - Check console: "Selected Selling Plan ID".
  - Disable/re-enable app if conflicts.
- **Progress Bar Issues**: Ensure CSS/JS updates applied (radio style). Clear cache.
- **Prices Wrong**: Toggle `use_auto_prices`. Use manual if metafields missing.
- **Duplicate Add/Error on Cart Add**: Widget isolation prevents – if issues, inspect app embeds.
- **Styles Not Applying**: Theme conflicts – add `!important` or scope CSS.
- **Schema Errors**: Fixed "deepened" to "header" – ensure JSON valid.
- **Button Text**: No price hidden as per code (text is "Add Bundle to Cart").
- **Update Progress Bar**: If using old number style, replace with provided radio CSS/JS (from previous response). Add new color settings to schema under "Progress Bar Colors".

### Customization Tips
- **Extend Bundles**: Edit JS `maxTotalForBundle` for more logic.
- **Free Products**: Adjust `freeProducts` in JS.
- **Analytics**: Add GA events in `handleAddToCart`.
- **Multilingual**: Use Shopify translations or settings for text.
- **Performance**: Images optimize with `img_url: '200x'`.

For support: Check Shopify docs on sections/apps. If bugs, console errors or share product URL/setup.

Version: 1.2 (Progress bar radio update, subscription fix). Updated: October 2023.
