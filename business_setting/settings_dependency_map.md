# Business Settings - Complete Dependency Map

This document is the result of a global codebase audit. It provides a definitive map of every Business Setting ID, its purpose, and a complete list of every file in the application that reads that setting to change its behavior.

| **Setting ID** | **Purpose** | **Dependent Features & Files** |
| :--- | :--- | :--- |
| **1010** | Enable KOT (Kitchen Order Ticket) printing in POS | `feature/sales/views/pos_screen.dart` |
| **1020** | Enable "Assign Sales Person" during a sale | `feature/sales/views/sale_invoice_create_screen.dart`<br>`feature/sales/views/pos_screen.dart`<br>`feature/reports/view/report_screen.dart` |
| **1021** | Enable "Hold Invoice" functionality in Sales/POS | `feature/sales/views/sale_invoice_create_screen.dart`<br>`feature/sales/views/pos_screen.dart` |
| **1022** | **Rename "Serial Number" globally** | `feature/products/view/product_add.dart`<br>`feature/products/view/product_details.dart`<br>`feature/sales/views/sale_invoice_create_screen.dart`<br>`feature/sales/views/sales_product_screen.dart`<br>`feature/sales/views/pos_screen.dart`<br>`feature/purchase/view/purchase_product_screen.dart`<br>`feature/purchase_return/views/purchase_return_product_screen.dart`<br>`feature/sales_return/view/sales_return_product_list_screen.dart`<br>`feature/sales_return/view/dummy.dart`<br>`app/widgets/serial_number_widget.dart` |
| **1023** | Enable Invoice Note on PDF | `feature/sales/views/sales_invoice_screen.dart` |
| **1024** | **Enable Serial Number Tracking** | `feature/products/view/product_add.dart` |
| **1025** | **Enable Bundles / Composite Products** | `feature/products/view/product_add.dart` |
| **1026** | Set the POS Invoice Template | `feature/sales/views/pos_screen.dart` |
| **1027** | Show Paid/Due Amount on PDF | `widgets/pdf_template_two.dart`<br>`widgets/pdf_template_three.dart` |
| **1028** | Show MRP (Max Retail Price) on PDF | `widgets/pdf_template_one.dart`<br>`widgets/pdf_template_two.dart`<br>`widgets/pdf_template_three.dart`<br>`feature/sales_return/view/sales_return_invoice_screen.dart`<br>`app/widgets/estimate_template.dart` |
| **1029** | **Enable Product Variants** (Colors/Sizes) | `feature/products/view/product_add.dart` |
| **1032** | Enable Batched Products (Expiry Dates) | `feature/products/view/product_add.dart` |
| **1033** | Enable Multiple Pricing Levels for Products | `feature/sales/views/sales_product_screen.dart`<br>`feature/sales_return/view/sales_return_product_list_screen.dart`<br>`feature/estimate/views/estimate_product_screen.dart` |
