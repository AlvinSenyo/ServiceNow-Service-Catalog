# ServiceNow-Service-Catalog-With-Client-Script
**Product:** ServiceNow Service Catalog  
**Requirement:** Display a recommendation for Adobe Photoshop when a user orders a Developer Laptop (Mac) and selects Adobe Acrobat software.
# 🧩 End-to-End Data Analytics Workflow Using Excel, SQL, and Power BI

This project demonstrates a **complete, automated data analytics pipeline** — from raw data collection and cleaning to advanced visualization — using **Excel**, **PostgreSQL (SQL)**, and **Power BI**.

It showcases how to integrate these tools to deliver consistent, accurate, and interactive business intelligence across multiple regions.

---
---

## 📝 Project Overview
The goal of this project is to improve the User Experience (UX) within the Service Catalog by providing **contextual guidance**. By identifying specific combinations of hardware and software, the system proactively suggests additional tools that are commonly required for that specific user persona (e.g., Mac Developers), reducing the need for follow-up requests and improving productivity.

---

## 🏛️ System Architecture

1.  **Trigger:** An `onChange` event is fired when the user interacts with the `software_selection` variable.
2.  **Logic Engine:** A Catalog Client Script retrieves the current state of the form.


---
## 📂 File Inventory
* [Technical_Manual.txt](./Technical_Manual.txt) - Detailed, click-by-click build guide for manual replication.
* [DeveloperMacs.PNG](./DeveloperMac.PNG) - Catalog item creation.
* [Output.PNG](./Output.PNG) - Visual verification of the blue recommendation box appearing on the Service Portal.
* [RecommendAdobe.PNG](./RecommendAdobe.PNG) - Screenshot evidence of the Client Script.

## 📂 Process Flow
Components used:

| Object Type | Name | Description |
| :--- | :--- | :--- |
| **Catalog Item** | `Developer Laptop (Mac)` | The primary request form. |
| **Variable** | `device_type` | A Select Box defining the hardware platform. |
| **Variable** | `software_selection` | A Select Box defining the software package. |
| **Catalog Client Script** | `Recommend Photoshop Logic` | The JavaScript file containing the logic. |

---

## 💡 Key Functions
The implementation utilizes the following core ServiceNow JavaScript functions:

* `g_form.getValue(fieldName)`: Retrieves the internal value of a field to check logic conditions.
* `g_form.showFieldMsg(field, message, type)`: Injects a message box directly under a specific variable.
* `g_form.hideFieldMsg(field)`: Removes any existing messages to ensure the UI remains clean when conditions change.

---

## 🛠️ Implementation Script

```javascript
/**
 * Triggers a recommendation message when specific conditions are met.
 * Condition: Device = Mac AND Software = Adobe Acrobat.
 */
function onChange(control, oldValue, newValue, isLoading, isTemplate) {
    
    // 1. Exit the script early if the form is loading or the value is cleared.
    // This prevents errors on initial page load before the user interacts.
    if (isLoading || newValue === '') {
        return;
    }

    // 2. Fetch the current value of the device_type field.
    // We use the internal Name of the variable, not the Label.
    var device = g_form.getValue('device_type');

    // 3. Logic: Check if the user is on a Mac and selected Acrobat.
    // We compare against the 'Value' of the choices (mac, adobe_acrobat).
    var isMac = (device == 'mac');
    var isAcrobat = (newValue == 'adobe_acrobat');

    if (isMac && isAcrobat) {
        
        // 4. Recommendation Message
        var msg = "Recommendation: You may also need Adobe Photoshop.";
        
        // 5. Display the message in an 'info' (blue) box under the field.
        // Change 'info' to 'error' if you want the box to be Red.
        g_form.showFieldMsg('software_selection', msg, 'info');
        
    } else {
        
        // 6. Hide the message if the user changes the selection.
        // This keeps the form clean if they switch to a different device or software.
        g_form.hideFieldMsg('software_selection');
    }
}
