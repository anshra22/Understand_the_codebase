# Business Settings - Data Dictionary

This document serves as a "Data Dictionary" for the Business Settings feature. It breaks down every configurable rule within the app, explaining its category, name, data type, and its exact purpose in the application.

| **Center Setting (Category)** | **Sub Setting Name (The Rule)** | **Data Type** | **Purpose (What it controls)** |
| :--- | :--- | :--- | :--- |
| **🧾 Invoice** | Invoice Header Text | Text Input | Custom text that prints at the very top of the invoice. |
| | Invoice Prefix | Text Input | Sets a custom starting text for invoice numbers (e.g., "INV-2024-"). |
| | Starting Invoice Number | Text Input | The number the app should start counting from (e.g., 100). |
| | Terms & Conditions | Text Input | Appends legal text to the bottom of all generated PDFs/Receipts. |
| | Show Discount Column | Radio (Yes/No) | Hides or unhides the discount input fields during the Sales flow. |
| | Show Tax Column | Radio (Yes/No) | Hides or unhides the GST/Tax calculation fields during the Sales flow. |
| | | | |
| **📦 Product** | Custom Serial Label | Text Input | Renames the word "Serial Number" globally (e.g., to "IMEI" or "Batch No"). |
| | Enable Serial Tracking | Radio (Yes/No) | Unlocks the ability to track and scan individual items rather than just bulk quantity. |
| | Want Bundles in Products | Radio (Yes/No) | Unlocks the "Composite Item" feature to group multiple products into one saleable item. |
| | Enable Product Variants | Radio (Yes/No) | Unlocks the ability to add colors, sizes, or types to a single master product. |
| | Quick Add to Cart | Radio (Yes/No) | Bypasses the confirmation dialog in POS, adding items instantly when clicked. |
| | | | |
| **🖨️ Printer** | Invoice Template Selection | Dropdown List | Chooses the visual design (Template 1, 2, 3, or 4) for the generated PDF. |
| | Thermal vs A4 Print Format | Radio (Yes/No) | Switches the PDF engine to generate tiny 58mm receipts instead of full-page documents. |
| | Print on Save | Radio (Yes/No) | Automatically triggers the Bluetooth printer dialog the moment an invoice is created. |
| | | | |
| **🏦 Payment & Banking**| Bank Details | Text Input | Stores bank account info to print on PDFs and invoices. |
| | Enable UPI / QR Code | Radio (Yes/No) | Toggles the visibility of a scannable payment QR code on the invoice. |
| | UPI ID | Text Input | The actual UPI ID used to generate the scanning QR code. |
| | | | |
| **💬 Integrations** | Enable WhatsApp API | Radio (Yes/No) | Checked before sending automated messages/PDFs via WhatsApp. |
| | | | |
| **⚙️ Advance** | POS Quick Actions | Radio (Yes/No) | Enables fast-checkout buttons (like exact cash amounts) on the Point of Sale screen. |
| | Auto Round-off | Radio (Yes/No) | Automatically adjusts final invoice totals to the nearest whole number (e.g., 99.99 becomes 100.00). |
| | | | |
| **🏢 Business Details** | Company Logo | Image Upload | Displays the uploaded image at the top header of all PDFs and Estimates. |
| | Authorized Signature | Image Upload | Displays a pre-saved signature image at the bottom of official documents. |
| | Company Name | Text Input | Injects the business name into headers, PDFs, and UPI payment links. |
| | Address | Text Input | Prints the physical location of the business on invoices. |
| | GSTIN / Tax ID | Text Input | Displays the legal tax registration number on invoices for compliance. |