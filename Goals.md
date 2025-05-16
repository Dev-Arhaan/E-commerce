# Advanced Product Catalog: Project Breakdown

Here's a structured approach to building the Advanced Product Catalog:

---

## Phase 1: Project Setup & Core Product Display

* **Goal 1.1: Initialize Project Environment**
    * [ ] Create a new React application (e.g., using Vite or Create React App).
    * [ ] Initialize a Git repository for version control.
    * [ ] Create a basic project structure (folders for components, pages, services, assets, etc.).
* **Goal 1.2: Install Core Dependencies**
    * [ ] Install `tailwindcss` and its dependencies (`postcss`, `autoprefixer`).
        * [ ] Configure `tailwind.config.js` and `postcss.config.js`.
        * [ ] Import Tailwind styles into the main CSS file.
    * [ ] Install `react-router-dom` for client-side routing.
    * [ ] Install `axios` (or use native `Workspace`) for API calls.
* **Goal 1.3: Basic Product Data Fetching & Display**
    * [ ] Create a service/utility function to fetch products from `https://dummyjson.com/products`.
    * [ ] Create a `ProductCard` component:
        * [ ] Props: `product` object.
        * [ ] Display:
            * [ ] Product Name (`product.title`).
            * [ ] Short Description (`product.description`).
            * [ ] Price (`product.price`).
            * [ ] Thumbnail Image (`product.thumbnail`).
            * [ ] Stock Status (e.g., from `product.stock` > 0 ? "In Stock" : "Out of Stock").
        * [ ] Style with TailwindCSS for a basic card layout.
    * [ ] Create a `ProductListPage` component:
        * [ ] Use `useState` to store the list of products and loading/error states.
        * [ ] Use `useEffect` to fetch products on component mount.
        * [ ] Render a list/grid of `ProductCard` components.
        * [ ] Implement basic loading and error message display.
* **Goal 1.4: Basic Routing Setup**
    * [ ] Configure `App.js` (or your main router file) with `react-router-dom`.
    * [ ] Set up a route for the `ProductListPage` (e.g., `/`).
* **Goal 1.5: Initial UI Styling with TailwindCSS**
    * [ ] Create a basic layout structure (e.g., Header, Main Content, Footer).
    * [ ] Apply Tailwind utility classes for overall page structure and `ProductCard` styling.
    * [ ] Ensure basic responsiveness for the product list on desktop and mobile.

---

## Phase 2: Product Detail Page & Enhanced Product Display

* **Goal 2.1: Implement Product Detail Page**
    * [ ] Create a `ProductDetailPage` component.
    * [ ] Set up a dynamic route (e.g., `/products/:productId`) in `react-router-dom`.
    * [ ] Fetch individual product data using `https://dummyjson.com/products/{id}` based on the `productId` from URL parameters.
        * [ ] Handle loading and error states.
    * [ ] Display detailed product information:
        * [ ] Larger Product Image (e.g., from `product.images[0]` or cycle through `product.images`).
        * [ ] Detailed Product Description (`product.description`).
        * [ ] Price (`product.price`).
        * [ ] Rating (`product.rating`) and Reviews (display count or placeholder for reviews if API doesn't provide full reviews).
        * [ ] Stock Status (`product.stock`).
        * [ ] Brand (`product.brand`), Category (`product.category`).
    * [ ] Style the `ProductDetailPage` with TailwindCSS for clear presentation.
* **Goal 2.2: Navigation to Product Detail Page**
    * [ ] Make each `ProductCard` in `ProductListPage` clickable.
    * [ ] Use `Link` from `react-router-dom` to navigate to the corresponding `ProductDetailPage`.
* **Goal 2.3: "Related Products" Section**
    * [ ] In `ProductDetailPage`, fetch related products.
        * [ ] Strategy: Use the current product's category to fetch other products in the same category (e.g., `https://dummyjson.com/products/category/{categoryName}`).
        * [ ] Ensure the current product is not listed in related products.
        * [ ] Limit the number of related products displayed.
    * [ ] Create a `RelatedProducts` component (can reuse `ProductCard` or a variation).
    * [ ] Display related products below the main product details.
    * [ ] Style the "Related Products" section using TailwindCSS.

---

## Phase 3: Shopping Cart Implementation

* **Goal 3.1: Setup Cart State Management**
    * [ ] Choose state management: React Context API or Redux.
    * [ ] **If React Context API:**
        * [ ] Create `CartContext`.
        * [ ] Define initial state: `cartItems` (array), `itemCount`, `totalPrice`.
        * [ ] Implement `CartProvider` with functions (reducers or direct state updates) for:
            * [ ] Add item to cart.
            * [ ] Remove item from cart.
            * [ ] Update item quantity (optional, or handle by remove/add).
            * [ ] Clear cart.
    * [ ] **If Redux:**
        * [ ] Set up Redux store.
        * [ ] Create cart slice/reducer with actions for add, remove, update, clear.
* **Goal 3.2: "Add to Cart" Functionality**
    * [ ] Add an "Add to Cart" button to `ProductCard` and `ProductDetailPage`.
    * [ ] On click:
        * [ ] Dispatch action/call context function to add the selected product (with quantity 1, or increment if exists).
        * [ ] Update `itemCount` and `totalPrice` in the global state.
        * [ ] (Optional: Provide user feedback, e.g., toast "Item added to cart").
        * *Note: The `https://dummyjson.com/carts/add` POST endpoint is for server-side cart simulation. For this frontend task, focus on client-side state management first. API integration for cart can be a later step or mocked.*
* **Goal 3.3: Shopping Cart Display (Page or Overlay)**
    * [ ] Create a `CartPage` component (routed, e.g., `/cart`) or a `CartOverlay` component (modal/sidebar).
    * [ ] Display cart items:
        * [ ] Iterate through `cartItems` from global state.
        * [ ] For each item: Product Name, Image, Price, Quantity, Subtotal.
        * [ ] Add controls to remove item or adjust quantity.
    * [ ] Display Cart Summary:
        * [ ] Total number of items (`itemCount`).
        * [ ] Total price of items (`totalPrice`).
    * [ ] Style with TailwindCSS.
* **Goal 3.4: Remove Products from Cart**
    * [ ] Implement remove button functionality in the cart display.
    * [ ] Dispatch action/call context function to remove the item or decrement quantity.
    * [ ] Update `itemCount` and `totalPrice`.
* **Goal 3.5: Cart Persistence**
    * [ ] Use `localStorage` to save the cart state.
    * [ ] `useEffect` in `CartProvider` (or Redux store subscription) to update `localStorage` whenever cart state changes.
    * [ ] Load initial cart state from `localStorage` when the application starts.
* **Goal 3.6: Cart Indicator in Navigation**
    * [ ] Add a cart icon (e.g., in the Header component).
    * [ ] Display the `itemCount` from the global cart state next to the icon.
    * [ ] Link the icon to the `CartPage` or to toggle the `CartOverlay`.

---

## Phase 4: Dynamic Filtering

* **Goal 4.1: Setup Filter State Management**
    * [ ] Use React Context API or integrate with existing Redux store for filter state.
    * [ ] Define filter state:
        * [ ] `selectedCategory` (string, default: 'all' or '').
        * [ ] `priceRange` (object: `{ min: number, max: number }`).
        * [ ] `minRating` (number, default: 0).
        * [ ] `searchTerm` (string, for text search using the API).
    * [ ] Create actions/context functions to update these filter states.
* **Goal 4.2: Create Filter UI**
    * [ ] Create a `Filters` component (e.g., sidebar).
    * [ ] **Category Filter:**
        * [ ] Fetch categories from `https://dummyjson.com/products/categories`.
        * [ ] Display as a list of links, buttons, or a dropdown.
        * [ ] On selection, update `selectedCategory` state.
    * [ ] **Price Range Filter:**
        * [ ] Input fields for min and max price (e.g., `<input type="number">`).
        * [ ] (Optional: A range slider component).
        * [ ] On change, update `priceRange` state.
    * [ ] **Rating Filter:**
        * [ ] Buttons or radio inputs for star ratings (e.g., "4 stars & up", "3 stars & up").
        * [ ] On selection, update `minRating` state.
    * [ ] **Search Bar (for text search):**
        * [ ] Input field for `searchTerm`.
    * [ ] Style the filter UI using TailwindCSS for desktop and mobile views (e.g., collapsible on mobile).
    * [ ] Ensure appropriate spacing and usability.
* **Goal 4.3: Implement Filtering Logic & Real-Time Updates**
    * [ ] In `ProductListPage`, use `useEffect` to listen for changes in filter states.
    * [ ] **Client-Side Filtering (Initial Approach/Fallback):**
        * [ ] If all products are fetched initially, filter the array on the client based on active filters.
        * [ ] Apply category, price range, and rating filters sequentially.
    * [ ] **Server-Side Filtering (Using Mock API):**
        * [ ] Construct the API URL based on filters: `https://dummyjson.com/products/search?q={searchTerm}`.
        * [ ] For category: `https://dummyjson.com/products/category/{selectedCategory}`.
        * [ ] *Note: DummyJSON's search API might not directly support combined price and rating filters with category or search query. You may need to fetch by search/category and then apply price/rating filters client-side, or fetch all and filter client-side if complex server-side filtering is not feasible.*
        * [ ] Re-fetch products when filters change.
    * [ ] The product list should update immediately without a page reload.
* **Goal 4.4: Clear Filters Functionality**
    * [ ] Add a "Clear All Filters" button.
    * [ ] On click, reset all filter states to their default values.
    * [ ] The product list should revert to showing all (or default) products.

---

## Phase 5: Pagination / Infinite Scrolling & Real-Time Product Availability

* **Goal 5.1: Implement Pagination or Infinite Scrolling**
    * [ ] Choose one method. DummyJSON supports `limit` and `skip` for pagination.
    * [ ] **If Pagination:**
        * [ ] Add state for `currentPage`, `productsPerPage` (e.g., 10 or 20).
        * [ ] Fetch `total` products count from the API response.
        * [ ] Calculate `skip = (currentPage - 1) * productsPerPage`.
        * [ ] Fetch products with `limit` and `skip` parameters.
        * [ ] Create pagination controls (Next, Previous, Page numbers).
        * [ ] Update product list when page changes.
    * [ ] **If Infinite Scrolling:**
        * [ ] Add state for `currentPage` (or `skip` offset), `hasMore` items.
        * [ ] Use `Intersection Observer API` or scroll event listener to detect when user is near the bottom.
        * [ ] When triggered and `hasMore` is true:
            * [ ] Fetch next set of products (increment `skip`).
            * [ ] Append new products to the existing list.
            * [ ] Update `hasMore` based on API response (if fewer products than `limit` are returned, or if `skip + fetched_count >= total`).
            * [ ] Show a loading indicator at the bottom while fetching more.
* **Goal 5.2: Real-Time Product Availability (Conceptual)**
    * [ ] *Challenge: DummyJSON is a mock API and doesn't offer real WebSockets or dynamic stock updates.*
    * [ ] **Simulate/Conceptualize:**
        * [ ] For the purpose of the exercise, discuss how this *would* be implemented.
        * [ ] **WebSockets:**
            * [ ] Client connects to a WebSocket server.
            * [ ] Server emits stock update events (e.g., `{ productId: 'X', newStock: Y }`).
            * [ ] Client listens and updates local product state/UI.
        * [ ] **Polling:**
            * [ ] Periodically re-fetch stock for visible products or products in cart (less efficient).
            * [ ] `useEffect` with a timer to call an API endpoint (if one existed for stock only).
    * [ ] **Implement "Out of Stock" Message on Add to Cart Attempt:**
        * [ ] When "Add to Cart" is clicked, use the current (most recently fetched) stock information for that product.
        * [ ] If `product.stock <= 0` (or less than requested quantity):
            * [ ] Display an "Product Out of Stock" message (e.g., toast or inline message).
            * [ ] Prevent adding to cart.
    * [ ] **UI Indicators for Availability Checks:**
        * [ ] Add a loading spinner or visual cue if a specific stock check were to be implemented (e.g., a button "Check availability" or before adding to cart if a delay is expected). Since we're using existing data, this might be minimal.

---

## Phase 6: State Management, UI/UX Refinements & TailwindCSS

* **Goal 6.1: Solidify State Management**
    * [ ] Review `useEffect` and `useState` usage for efficiency and correctness.
    * [ ] Ensure global state (cart, filters) is well-managed and updates propagate correctly.
    * [ ] Ensure side effects are handled properly.
* **Goal 6.2: Responsive Design Polish with TailwindCSS**
    * [ ] Thoroughly test on mobile, tablet, and desktop views.
    * [ ] Adjust Tailwind classes for:
        * [ ] Navigation (e.g., collapsing to a hamburger menu).
        * [ ] Filter UI (e.g., drawer/modal on mobile).
        * [ ] Product grids/lists (column counts, spacing).
        * [ ] Product Detail Page layout.
        * [ ] Cart display.
* **Goal 6.3: TailwindCSS UI Components Styling**
    * [ ] Ensure consistent and clean styling for:
        * [ ] Buttons (primary, secondary, disabled states).
        * [ ] Forms (inputs, selects, sliders) - focus states, validation (if any).
        * [ ] Product Cards (hover effects, clear information hierarchy).
        * [ ] Modals or overlays used.
* **Goal 6.4: General UI/UX Improvements**
    * [ ] Add subtle transitions and animations for a smoother feel.
    * [ ] Implement clear loading states (e.g., skeleton loaders for product cards/details).
    * [ ] Provide user feedback for actions (e.g., "Filters applied", "Item removed").
    * [ ] Ensure accessibility basics (e.g., ARIA attributes for interactive elements, sufficient color contrast).

---

## Phase 7: Bonus Features (Optional)

* **Goal 7.1: Search Autocomplete**
    * [ ] Enhance the search bar in the `Filters` component.
    * [ ] On user input (with debouncing to limit API calls):
        * [ ] Fetch suggestions from `https://dummyjson.com/products/search?q={typedValue}&limit=5&select=title`.
        * [ ] Display suggestions in a dropdown below the search bar.
        * [ ] On selecting a suggestion, update `searchTerm` and trigger filtering, or navigate to the product if it's a direct match.
* **Goal 7.2: Product Ratings Display (Focus on Display)**
    * [ ] Ensure the average rating and number of reviews (if available from API, `product.rating` is usually just the average) are clearly displayed on `ProductCard` and `ProductDetailPage`.
    * [ ] Style star ratings visually (e.g., using star icons).
    * *Note: Implementing a system for users to *submit* ratings is beyond the scope of a mock API but could be simulated on the frontend.*
* **Goal 7.3: Dark/Light Mode Toggle**
    * [ ] Add a toggle button (e.g., in the Header).
    * [ ] Use TailwindCSS's `dark:` variant:
        * [ ] Add `darkMode: 'class'` to `tailwind.config.js`.
        * [ ] Toggle a `dark` class on the `<html>` or `<body>` element.
    * [ ] Define dark mode styles for components using `dark:...` utility classes.
    * [ ] (Optional) Persist theme preference in `localStorage`.

---

## Phase 8: Finalization & Deliverables

* **Goal 8.1: Code Review and Refactoring**
    * [ ] Clean up code: remove unused variables, comments, console logs.
    * [ ] Ensure consistent coding style and formatting.
    * [ ] Break down large components if necessary.
    * [ ] Optimize for performance where possible (e.g., memoization, efficient state updates).
* **Goal 8.2: Testing**
    * [ ] Perform thorough manual testing of all features.
        * [ ] Test different filter combinations.
        * [ ] Test cart operations.
        * [ ] Test pagination/infinite scrolling.
        * [ ] Test on different browsers (Chrome, Firefox, Safari).
        * [ ] Test responsiveness on various device sizes.
    * [ ] Check for console errors or warnings.
* **Goal 8.3: Create README.md**
    * [ ] Project Title and brief Overview.
    * [ ] Features Implemented (list them out).
    * [ ] Technologies Used.
    * [ ] Installation Instructions:
        * [ ] `git clone ...`
        * [ ] `npm install` (or `yarn install`)
        * [ ] `npm run dev` (or `yarn dev`)
    * [ ] How to Use / Demo Instructions.
    * [ ] Known Issues or Limitations (especially regarding mock API constraints).
    * [ ] (Optional) Screenshots or GIFs.
* **Goal 8.4: Prepare GitHub Repository**
    * [ ] Ensure all code is pushed to the GitHub repository.
    * [ ] Create a clean commit history (squash/rebase if necessary, though not critical for this type of project).
    * [ ] Ensure the `README.md` is complete and well-formatted.
* **Goal 8.5: Deploy Live Demo**
    * [ ] Choose a hosting platform (Vercel or Netlify are excellent for React apps).
    * [ ] Connect the GitHub repository to the platform.
    * [ ] Configure build settings (usually auto-detected for Vite/CRA).
    * [ ] Deploy the project.
    * [ ] Test the live demo link thoroughly.
* **Goal 8.6: Share Deliverables**
    * [ ] Compose an email to `mahesh.takey@cadandcart.com`.
    * [ ] Include the link to the GitHub repository.
    * [ ] Include the link to the live demo.
    * [ ] Briefly mention the completion of the task.

