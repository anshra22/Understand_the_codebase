# Architecture Diagram Rules (Level 1)

These are the strict rules and guidelines established for creating architectural component diagrams for the Bookz App codebase. Adhering to these rules ensures that the diagrams serve as accurate, living technical documentation that perfectly maps the theoretical architecture to the physical codebase.

### 1. The Core Structure (The 3-Tier Vertical Stack)
*   **Three Main Boxes:** The diagram must always be divided into three strict parent containers (subgraphs):
    1.  `UI Layer` (Presentation / Views)
    2.  `GetX Layer` (Business Logic / Controllers)
    3.  `Hive Layer` (Data Access / Database Operations)
*   **Vertical Alignment:** Data flow must always read top-to-bottom (`Views` ➔ `GetX` ➔ `Hive`).

### 2. UI Layer Rules (The Trigger)
*   **Specificity:** Instead of generic descriptions (like "User taps save"), the UI box must state the exact Flutter widget being used (e.g., `Element: ElevatedButton('Save')`).
*   **Purpose:** The UI box must briefly explain *why* the element exists (e.g., `Purpose: Submit new category details`).
*   **Action Labels:** The arrows connecting the UI to GetX must be labeled with the exact logical action trigger (e.g., **`save`**, **`load`**, **`delete`**).

### 3. GetX Layer Rules (The Logic & Orchestration)
*   **Sub-boxes for Files:** Inside the GetX parent box, there must be separated sub-boxes (subgraphs) representing the actual physical `.dart` files (e.g., `product_categories_controller.dart`).
*   **Method Rectangles:** Inside the file sub-boxes, individual GetX operations must be drawn as standard **Rectangle shapes**.
*   **Technical Signatures (Input/Output):** Inside the rectangle, every executed method must be written exactly as it appears in Dart code, showing what it returns (Output), the method name, and what it gets as arguments (Input) (e.g., `Future<void> saveCategory(BuildContext context)`).
*   **Purpose:** Each method must include a brief `Purpose:` description immediately below its signature to explain what it does.
*   **Orchestration (Many-to-Many):** A single GetX method acting as an orchestrator (like `saveProduct`) can and should point to *multiple* different Hive files/methods if the business logic requires it (e.g., saving core data to one file, and serial numbers to another file simultaneously).

### 4. Hive / Data Access Layer Rules (The Bottleneck)
*   **Strict Boundaries:** The database layer must **never** communicate directly with the UI layer. Data must always travel back up through the GetX Controller first.
*   **One Box = One Physical File:** For every physical file in the `lib/app/hive/controller/` folder (like `hive_product.dart`), there can only be **one single file container (subgraph)**.
*   **Method Rectangles:** Inside the file container, individual database operations must be drawn as standard **Rectangle shapes** (not database cylinders).
*   **Technical Signatures (Input/Output):** Just like the GetX layer, every database method must be written as a precise Dart signature showing what it gets and returns (e.g., `Future<void> saveProductCategory(ProductCategory category)`).
*   **Purpose:** Each method must include a brief `Purpose:` description immediately below its signature.
*   **The Funnel Effect:** Multiple methods from various GetX Controllers must consolidate and point their arrows precisely into the specific Hive method rectangles, proving exactly which files and functions handle specific pieces of data.

### 5. Styling Rules
*   **Visual Contrast:** The diagram must use high-contrast styling with white text (`color:#ffffff`) against distinct, dark background colors to clearly separate the three tiers visually.
    *   UI Layer: Blue shades (`#1565c0`)
    *   GetX Layer: Green shades (`#2e7d32`)
    *   Hive Layer: Orange/Red shades (`#e65100`)


    --------------------------------

    graph TD
    %% ==========================================
    %% TOP LAYER: USER INTERFACE (VIEWS)
    %% ==========================================
    subgraph UI_Layer ["📱 User Interface (Views)"]
        direction LR
        UI_List["<b>Product List Screen</b><br/>Elements: InitState, SearchBar, Scroll, Tabs<br/>Purpose: View & filter inventory"]
        UI_Add["<b>Add/Edit Product Screen</b><br/>Elements: InitState, Buttons, Icons<br/>Purpose: Create or edit product details"]
        UI_Cat["<b>Category Screen</b><br/>Elements: InitState, SaveBtn, DeleteIcon<br/>Purpose: Manage categories"]
        UI_Brand["<b>Brand Screen</b><br/>Elements: InitState, SaveBtn, DeleteIcon<br/>Purpose: Manage brands"]
        UI_Det["<b>Product Details Screen</b><br/>Elements: InitState, UI Builders<br/>Purpose: View deep product details & taxes"]
    end

    %% ==========================================
    %% MIDDLE LAYER: BUSINESS LOGIC (GETX)
    %% ==========================================
    subgraph GetX_Layer ["🧠 Business Logic (GetX Controllers)"]
        direction LR
        
        subgraph Ctrl_List ["products_list_controller.dart"]
            direction TB
            GL_Get["Future&lt;void&gt; getProductList()<br/>Purpose: Fetch product list"]
            GL_Filt["void filterProductss(String query)<br/>Purpose: Search products"]
            GL_More["void loadMoreProducts()<br/>Purpose: Paginate products"]
            GL_UpdF["void updateFilter(ProductFilter filter)<br/>Purpose: Switch tabs"]
        end
        
        subgraph Ctrl_Add ["products_add_controller.dart"]
            direction TB
            GA_Init["void onInit() / Future&lt;void&gt; initSku()<br/>Purpose: Setup state & SKU"]
            GA_Sel["void brandSelect() / void categorySelect()<br/>Purpose: Open selection dialogs"]
            GA_Bar["void generateBarCode()<br/>Purpose: Create barcode string"]
            GA_Save["Future&lt;void&gt; saveProduct(BuildContext context)<br/>Purpose: Insert new product"]
            GA_Upd["Future&lt;void&gt; updateProduct(BuildContext context)<br/>Purpose: Modify product"]
        end
        
        subgraph Ctrl_Cat ["product_categories_controller.dart"]
            direction TB
            GC_Get["Future&lt;void&gt; getProductCategory()<br/>Purpose: Load categories"]
            GC_Save["Future&lt;void&gt; saveCategory()<br/>Purpose: Insert category"]
            GC_Edit["Future&lt;void&gt; editCategory()<br/>Purpose: Modify category"]
            GC_Del["Future&lt;void&gt; deleteCategory(String catId)<br/>Purpose: Remove category"]
        end

        subgraph Ctrl_Brand ["product_brand_controller.dart"]
            direction TB
            GB_Get["Future&lt;void&gt; getProductBrand()<br/>Purpose: Load brands"]
            GB_Save["Future&lt;void&gt; saveBrand()<br/>Purpose: Insert brand"]
            GB_Edit["Future&lt;void&gt; editBrand()<br/>Purpose: Modify brand"]
            GB_Del["Future&lt;void&gt; deleteBrand(String brandId)<br/>Purpose: Remove brand"]
        end
        
        subgraph Ctrl_Det ["product_details_controller.dart"]
            direction TB
            GD_Fetch["Future&lt;void&gt; fetchProductDetails(String productID)<br/>Purpose: Load full details"]
            GD_Tax["Future&lt;String&gt; calculateTax(double price, String taxId)<br/>Purpose: Compute dynamic pricing"]
        end
    end

    %% ==========================================
    %% BOTTOM LAYER: DATA ACCESS (HIVE FILES)
    %% ==========================================
    subgraph Hive_Layer ["💾 Data Access Layer (Hive Operations)"]
        direction LR
        
        subgraph DB_Prod ["hive_product.dart (HiveProductController)"]
            direction TB
            HP_GetP["Future&lt;List&lt;Product&gt;&gt; getLocalProductList()<br/>Purpose: Read products from DB"]
            HP_SavP["Future&lt;void&gt; saveProduct(Product product)<br/>Purpose: Write/Update product in DB"]
            
            HP_GetC["Future&lt;List&lt;ProductCategory&gt;&gt; getLocalProductCategories()<br/>Purpose: Read categories from DB"]
            HP_SavC["Future&lt;void&gt; saveProductCategory(ProductCategory category)<br/>Purpose: Write/Update category in DB"]
            HP_DelC["Future&lt;void&gt; deleteProductCategory(String id)<br/>Purpose: Remove category from DB"]
            
            HP_GetB["Future&lt;List&lt;ProductBrand&gt;&gt; getLocalProductBrands()<br/>Purpose: Read brands from DB"]
            HP_SavB["Future&lt;void&gt; saveProductBrand(ProductBrand brand)<br/>Purpose: Write/Update brand in DB"]
            HP_DelB["Future&lt;void&gt; deleteProductBrand(String id)<br/>Purpose: Remove brand from DB"]
        end
        
        subgraph DB_Serial ["hive_product_serial.dart (HiveProductSerialController)"]
            direction TB
            HS_Get["Future&lt;List&lt;ProductSerial&gt;&gt; getProductSerials(String productId)<br/>Purpose: Read product serials from DB"]
            HS_Sav["Future&lt;void&gt; saveProductSerials(List&lt;ProductSerial&gt; serials)<br/>Purpose: Write product serials to DB"]
        end
    end

    %% ==========================================
    %% ⬇️ DOWNWARD FLOW: UI -> GETX
    %% ==========================================
    UI_List -->|Load/Scroll| GL_Get & GL_More
    UI_List -->|Search/Filter| GL_Filt & GL_UpdF
    
    UI_Add -->|Init/Helpers| GA_Init & GA_Sel & GA_Bar
    UI_Add -->|Save/Update| GA_Save & GA_Upd
    
    UI_Cat -->|Load| GC_Get
    UI_Cat -->|Save/Edit| GC_Save & GC_Edit
    UI_Cat -->|Delete| GC_Del
    
    UI_Brand -->|Load| GB_Get
    UI_Brand -->|Save/Edit| GB_Save & GB_Edit
    UI_Brand -->|Delete| GB_Del
    
    UI_Det -->|Load Details| GD_Fetch
    UI_Det -->|View Tax| GD_Tax

    %% ==========================================
    %% ⬇️ DOWNWARD FLOW: GETX -> HIVE
    %% ==========================================
    GL_Get & GL_More ==>|Reads Products| HP_GetP
    
    GA_Save & GA_Upd ==>|Writes Core Data| HP_SavP
    GA_Save & GA_Upd ==>|Writes Serial Data| HS_Sav
    
    GC_Get ==>|Reads Categories| HP_GetC
    GC_Save & GC_Edit ==>|Writes Category| HP_SavC
    GC_Del ==>|Deletes Category| HP_DelC
    
    GB_Get ==>|Reads Brands| HP_GetB
    GB_Save & GB_Edit ==>|Writes Brand| HP_SavB
    GB_Del ==>|Deletes Brand| HP_DelB
    
    GD_Fetch ==>|Reads Core Data| HP_GetP
    GD_Fetch ==>|Reads Serial Data| HS_Get

    %% Styling
    classDef layerBox fill:none,stroke:#ffffff,stroke-width:2px,stroke-dasharray: 5 5,color:#ffffff;
    classDef ui fill:#1565c0,stroke:#0d47a1,stroke-width:2px,color:#ffffff;
    classDef logic fill:#2e7d32,stroke:#1b5e20,stroke-width:2px,color:#ffffff;
    classDef data fill:#e65100,stroke:#bf360c,stroke-width:2px,color:#ffffff;
    
    class UI_Layer,GetX_Layer,Hive_Layer layerBox;
    class UI_List,UI_Add,UI_Cat,UI_Brand,UI_Det ui;
    class GL_Get,GL_Filt,GL_More,GL_UpdF,GA_Init,GA_Sel,GA_Bar,GA_Save,GA_Upd,GC_Get,GC_Save,GC_Edit,GC_Del,GB_Get,GB_Save,GB_Edit,GB_Del,GD_Fetch,GD_Tax logic;
    class HP_GetP,HP_SavP,HP_GetC,HP_SavC,HP_DelC,HP_GetB,HP_SavB,HP_DelB,HS_Get,HS_Sav data;