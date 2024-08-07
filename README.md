# Fixing Bulk Import Issues for Extra Fields in wp-all-import Plugin


## Problem Description
When using wp-all-import to bulk import data from CSV files, extra fields were not being stored correctly in the database. This documentation outlines the modifications made to resolve this issue.

## Changes Made
### 1. Adjusting Extra Fields Addition
<b>File Location: </b> C:\Users\User\Local Sites\doc\app\public\wp-content\plugins\wp-all-import-pro\views\admin\import\template\_custom_fields_template.php

<b>Description: </b> Modified the way extra fields are sent or added during the import process.


### Changes Made:
  ```
    <script>
    document.addEventListener('DOMContentLoaded', function() {
        document.querySelectorAll(".wpallimport-custom-fields .wp_all_import_autocomplete")[0].value = 'lp_listingpro_options_fields';
        document.querySelectorAll(".wpallimport-custom-fields textarea")[0].value = 'Company: \nAddress: \nP.O. BOX: \nIsland/State/Province: \nCountry: \nTelephone Number(s):\nBusiness Email Address:\nServices I Offer:\nFeatures: \n';
    });
</script>
  ```


