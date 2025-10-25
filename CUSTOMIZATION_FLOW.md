# Item Customization Flow Diagram

## 🔄 Complete User Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                     RESTAURANT OWNER FLOW                        │
└─────────────────────────────────────────────────────────────────┘

1. Login to Dashboard
   │
   ├─→ Click "Add Menu Item"
   │   │
   │   ├─→ Fill Basic Info (Name, Price, Description, Image)
   │   │
   │   ├─→ Customization Options Section
   │   │   │
   │   │   ├─→ [Sizes Tab]
   │   │   │   ├─→ Add "Small" (₹299) ✓ Default
   │   │   │   ├─→ Add "Medium" (₹499)
   │   │   │   └─→ Add "Large" (₹699)
   │   │   │
   │   │   ├─→ [Add-ons Tab]
   │   │   │   ├─→ Add "Extra Cheese" (+₹50) 🟢
   │   │   │   ├─→ Add "Olives" (+₹40) 🟢
   │   │   │   └─→ Add "Bacon" (+₹80) 🔴
   │   │   │
   │   │   ├─→ [Options Tab]
   │   │   │   ├─→ Add "Spice Level" (Required, Single)
   │   │   │   │   ├─→ "Mild" (₹0)
   │   │   │   │   ├─→ "Medium" (₹0)
   │   │   │   │   └─→ "Hot" (+₹20)
   │   │   │   │
   │   │   │   └─→ Add "Toppings" (Optional, Multiple)
   │   │   │       ├─→ "Mushrooms" (+₹30)
   │   │   │       └─→ "Peppers" (+₹20)
   │   │   │
   │   │   └─→ [Exclude Tab]
   │   │       ├─→ Add "Onions"
   │   │       ├─→ Add "Garlic"
   │   │       └─→ ✓ Allow special instructions
   │   │
   │   └─→ Click "Add Item" → ✅ Saved to Database
   │
   └─→ Item appears in menu with customization options


┌─────────────────────────────────────────────────────────────────┐
│                       CUSTOMER FLOW                              │
└─────────────────────────────────────────────────────────────────┘

1. Visit Menu Page (/m/{restaurantId})
   │
   ├─→ Enter Table Number
   │
   ├─→ Browse Menu Items
   │   │
   │   ├─→ Simple Item → [Add] → Directly to Cart
   │   │
   │   └─→ Customizable Item → [Customize] → Opens Modal
   │       │
   │       ├─→ 📏 Choose Size
   │       │   ┌─────────────────────────────────────┐
   │       │   │  ○ Small    ● Medium    ○ Large    │
   │       │   │    ₹299       ₹499        ₹699     │
   │       │   └─────────────────────────────────────┘
   │       │
   │       ├─→ 🎛️ Select Options (Required *)
   │       │   ┌─────────────────────────────────────┐
   │       │   │ Spice Level *                       │
   │       │   │  ○ Mild (₹0)                        │
   │       │   │  ● Medium (₹0)                      │
   │       │   │  ○ Hot (+₹20)                       │
   │       │   └─────────────────────────────────────┘
   │       │
   │       ├─→ ➕ Add-ons (Optional)
   │       │   ┌─────────────────────────────────────┐
   │       │   │ ☑ Extra Cheese      +₹50           │
   │       │   │ ☑ Olives            +₹40           │
   │       │   │ ☐ Bacon             +₹80           │
   │       │   └─────────────────────────────────────┘
   │       │
   │       ├─→ ❌ Exclude Ingredients
   │       │   ┌─────────────────────────────────────┐
   │       │   │ [Onions✓] [Garlic✓] [Peppers]     │
   │       │   └─────────────────────────────────────┘
   │       │
   │       ├─→ 📝 Special Instructions
   │       │   ┌─────────────────────────────────────┐
   │       │   │ "Please make it extra crispy"      │
   │       │   │                          (45/200)   │
   │       │   └─────────────────────────────────────┘
   │       │
   │       ├─→ 🔢 Quantity
   │       │   ┌─────────────────────────────────────┐
   │       │   │     [-]    2    [+]                 │
   │       │   └─────────────────────────────────────┘
   │       │
   │       ├─→ 💰 Live Price Calculation
   │       │   ┌─────────────────────────────────────┐
   │       │   │ Medium Pizza:        ₹499           │
   │       │   │ Extra Cheese:        +₹50           │
   │       │   │ Olives:              +₹40           │
   │       │   │ ─────────────────────────           │
   │       │   │ Subtotal:            ₹589           │
   │       │   │ Quantity: ×2                        │
   │       │   │ ═════════════════════════           │
   │       │   │ Total:               ₹1,178         │
   │       │   └─────────────────────────────────────┘
   │       │
   │       └─→ Click [Add to Cart] → ✅ Added
   │
   ├─→ View Cart
   │   ┌─────────────────────────────────────────────┐
   │   │ 🍕 Margherita Pizza                         │
   │   │    Size: Medium                             │
   │   │    Add-ons: Extra Cheese, Olives            │
   │   │    No: Onions, Garlic                       │
   │   │    "Please make it extra crispy"            │
   │   │    ₹589 × 2 = ₹1,178                        │
   │   │    [-] 2 [+] [🗑️]                           │
   │   └─────────────────────────────────────────────┘
   │
   └─→ Proceed to Checkout → Place Order


┌─────────────────────────────────────────────────────────────────┐
│                    PRICE CALCULATION LOGIC                       │
└─────────────────────────────────────────────────────────────────┘

Base Price Calculation:
┌────────────────────────────────────────────────────────────────┐
│                                                                 │
│  1. Start with Selected Size Price (or Item Price if no sizes) │
│     Example: Medium Pizza = ₹499                               │
│                                                                 │
│  2. Add each selected Add-on Price                             │
│     + Extra Cheese = +₹50                                      │
│     + Olives = +₹40                                            │
│                                                                 │
│  3. Apply Customization Option Price Modifiers                 │
│     + Hot Spice Level = +₹20                                   │
│     + Mushroom Topping = +₹30                                  │
│                                                                 │
│  4. Excluded Ingredients = No Price Change                     │
│     - Onions = ₹0                                              │
│     - Garlic = ₹0                                              │
│                                                                 │
│  5. Special Instructions = No Price Change                     │
│                                                                 │
│  ─────────────────────────────────────────────────────────────│
│  Subtotal per Item = ₹639                                      │
│                                                                 │
│  6. Multiply by Quantity                                       │
│     ₹639 × 2 = ₹1,278                                          │
│                                                                 │
│  ═════════════════════════════════════════════════════════════│
│  TOTAL = ₹1,278                                                │
│                                                                 │
└────────────────────────────────────────────────────────────────┘


┌─────────────────────────────────────────────────────────────────┐
│                    CART UNIQUENESS LOGIC                         │
└─────────────────────────────────────────────────────────────────┘

Same Item, Different Customizations = Separate Cart Items
┌────────────────────────────────────────────────────────────────┐
│                                                                 │
│  Cart Item 1:                                                   │
│  ├─ Pizza (Medium, Extra Cheese, No Onions)                    │
│  └─ Quantity: 2                                                 │
│                                                                 │
│  Cart Item 2:                                                   │
│  ├─ Pizza (Large, Olives, No Garlic)                           │
│  └─ Quantity: 1                                                 │
│                                                                 │
│  These are SEPARATE items in cart                              │
│                                                                 │
└────────────────────────────────────────────────────────────────┘

Same Item, Same Customizations = Increase Quantity
┌────────────────────────────────────────────────────────────────┐
│                                                                 │
│  Add: Pizza (Medium, Extra Cheese, No Onions) × 1              │
│  Add: Pizza (Medium, Extra Cheese, No Onions) × 1              │
│                                                                 │
│  Result:                                                        │
│  └─ Pizza (Medium, Extra Cheese, No Onions) × 2                │
│                                                                 │
└────────────────────────────────────────────────────────────────┘


┌─────────────────────────────────────────────────────────────────┐
│                    DATA STRUCTURE FLOW                           │
└─────────────────────────────────────────────────────────────────┘

Database (MongoDB)
    ↓
MenuItem Document
├─ name: "Margherita Pizza"
├─ price: 299
├─ sizes: [
│   {name: "Small", price: 299, isDefault: true},
│   {name: "Medium", price: 499},
│   {name: "Large", price: 699}
│  ]
├─ addOns: [
│   {name: "Extra Cheese", price: 50, isVeg: true},
│   {name: "Olives", price: 40, isVeg: true}
│  ]
├─ customizationOptions: [
│   {
│     name: "Spice Level",
│     type: "single",
│     required: true,
│     options: [
│       {label: "Mild", priceModifier: 0},
│       {label: "Hot", priceModifier: 20}
│     ]
│   }
│  ]
├─ excludableIngredients: ["Onions", "Garlic"]
└─ allowSpecialInstructions: true
    ↓
API Response
    ↓
Frontend State
    ↓
Customer Customization
    ↓
Cart Item
├─ ...menuItem (all fields)
├─ selectedSize: {name: "Medium", price: 499}
├─ selectedAddOns: [{name: "Extra Cheese", price: 50}]
├─ customizations: {"Spice Level": ["Hot"]}
├─ excludedIngredients: ["Onions"]
├─ specialInstructions: "Extra crispy"
├─ quantity: 2
├─ customizedPrice: 569
└─ customizationKey: "hash_of_customizations"
    ↓
Checkout
    ↓
Order Document


┌─────────────────────────────────────────────────────────────────┐
│                    COMPONENT HIERARCHY                           │
└─────────────────────────────────────────────────────────────────┘

App
└─ MenuPage
   ├─ Header (Restaurant name, Cart button)
   ├─ Search Bar
   ├─ Category Filter
   ├─ Menu Items Grid
   │  └─ MenuItem Card
   │     ├─ Image
   │     ├─ Name & Description
   │     ├─ Price
   │     └─ [Customize] or [Add] Button
   │        │
   │        └─→ Opens ItemCustomizationModal
   │            ├─ Header (Item name, Close button)
   │            ├─ Size Selection (if available)
   │            ├─ Customization Options (if available)
   │            ├─ Add-ons (if available)
   │            ├─ Exclude Ingredients (if available)
   │            ├─ Special Instructions (if enabled)
   │            ├─ Quantity Selector
   │            ├─ Price Display (live calculation)
   │            └─ [Add to Cart] Button
   │
   └─ Cart Sidebar
      ├─ Cart Items List
      │  └─ Cart Item
      │     ├─ Image
      │     ├─ Name
      │     ├─ Customization Details
      │     ├─ Price
      │     └─ Quantity Controls
      ├─ Total Price
      └─ [Checkout] Button

RestaurantDashboard
└─ Menu Management Section
   ├─ Add Item Button
   │  └─→ Opens Add Item Modal
   │      ├─ Basic Info Form
   │      ├─ ItemCustomizationManager
   │      │  ├─ [Sizes] Tab
   │      │  ├─ [Add-ons] Tab
   │      │  ├─ [Options] Tab
   │      │  └─ [Exclude] Tab
   │      └─ [Save] Button
   │
   └─ Menu Items List
      └─ Menu Item Card
         └─ [Edit] Button
            └─→ Opens Edit Item Modal
                (Same structure as Add Item Modal)


┌─────────────────────────────────────────────────────────────────┐
│                    STATE MANAGEMENT                              │
└─────────────────────────────────────────────────────────────────┘

MenuPage State:
├─ items: MenuItem[]
├─ cart: CartItem[]
├─ customizingItem: MenuItem | null
├─ searchQuery: string
├─ filters: {...}
└─ showCart: boolean

ItemCustomizationModal State:
├─ selectedSize: Size | null
├─ selectedAddOns: AddOn[]
├─ customizations: {[optionName]: string[]}
├─ excludedIngredients: string[]
├─ specialInstructions: string
└─ quantity: number

RestaurantDashboard State:
├─ items: MenuItem[]
├─ categories: Category[]
├─ itemFormData: {
│   ...basicFields,
│   sizes: Size[],
│   addOns: AddOn[],
│   customizationOptions: Option[],
│   excludableIngredients: string[],
│   allowSpecialInstructions: boolean
│  }
└─ showAddItemModal: boolean


┌─────────────────────────────────────────────────────────────────┐
│                    SUCCESS INDICATORS                            │
└─────────────────────────────────────────────────────────────────┘

✅ Restaurant can add customizations easily
✅ Customer sees "Customize" button on items
✅ Modal opens with smooth animation
✅ All options display correctly
✅ Price updates in real-time
✅ Validation works for required fields
✅ Cart shows full customization details
✅ Multiple customizations handled correctly
✅ Checkout includes all customization data
✅ Order placed successfully with customizations
```

## 🎯 Key Takeaways

1. **Flexible System**: Supports any combination of customizations
2. **Real-time Feedback**: Prices update instantly
3. **Smart Cart**: Handles complex customization scenarios
4. **User-Friendly**: Intuitive for both restaurant owners and customers
5. **Scalable**: Can handle simple to complex customizations
6. **Production-Ready**: Fully tested and documented
