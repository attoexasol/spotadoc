# Fixing Bulk Import Issues for Extra Fields in wp-all-import Plugin


## Problem Description
When using wp-all-import to bulk import data from CSV files, extra fields were not being stored correctly in the database. This documentation outlines the modifications made to resolve this issue.

## Changes Made
### 1. Adjusting Extra Fields Addition
<b>File Location: </b> wp-content\plugins\wp-all-import-pro\views\admin\import\template\_custom_fields_template.php

<b>Description: </b> Modified the way extra fields are sent or added during the import process.


### Changes Made (added these code at the end of this file):
  ```
    <script>
    document.addEventListener('DOMContentLoaded', function() {
        document.querySelectorAll(".wpallimport-custom-fields .wp_all_import_autocomplete")[0].value = 'lp_listingpro_options_fields';
        document.querySelectorAll(".wpallimport-custom-fields textarea")[0].value = 'Company: \nAddress: \nP.O. BOX: \nIsland/State/Province: \nCountry: \nTelephone Number(s):\nBusiness Email Address:\nServices I Offer:\nFeatures: \n';
    });
</script>
  ```

![27ac40c3-01eb-4e79-b434-101c81560aa7](https://github.com/user-attachments/assets/a7d07d21-7694-469d-ad8a-5ada6ceb94ac)

### 2. Handling Data Storage in Database: 
<b>File Location: </b>  wp-content\plugins\wp-all-import-pro\models\import\record.php

<b>Description: </b>  Manipulated data types to ensure extra fields are stored correctly in the database and displayed in the admin panel.

### Line number: 3688 to 3802

### Changes Made:

  ```
    // Extract and process input data for extra fields
$inputs = $add_meta_data[0][0];
$start = strpos($inputs, "Company");
$end = strpos($inputs, "')");

$result = substr($inputs, $start, $end - $start);

$lines = explode("\\r\\n", $result);

// Initialize variables for each extra field
$company = '';
$address = '';
$pobox = '';
$island_state_province = '';
$country = '';
$telephone_numbers = '';
$email = '';
$services_i_offer = '';
$features = '';

// Iterate through lines to extract key-value pairs
foreach ($lines as $line) {
    $line = trim($line);
    if (!empty($line)) {
        $parts = explode(":", $line, 2);

        if (count($parts) == 2) {
            $key = trim($parts[0]);
            $value = trim($parts[1]);

            // Assign values based on key
            switch ($key) {
                case "Company":
                    $company = $value;
                    break;
                case "Address":
                    $address = $value;
                    break;
                case "P.O. BOX":
                    $pobox = $value;
                    break;
                case "Island/State/Province":
                    $island_state_province = $value;
                    break;
                case "Country":
                    $country = $value;
                    break;
                case "Telephone Number(s)":
                    $telephone_numbers = $value;
                    break;
                case "Business Email Address":
                    $email = $value;
                    break;
                case "Services I Offer":
                    $services_i_offer = $value;
                    break;
                case "Features":
                    $features = $value;
                    break;
                default:
                    break;
            }
        }
    }
}

// Build the output array with serialized data
$output = [
    'lp_feature' => ['5', '444', '445', '446', '447', '450', '452', '83', '451'],
    'company' => $company,
    'address' => $address,
    'p-o-box' => $pobox,
    'island-state-province' => $island_state_province,
    'country' => $country,
    'telephone-numbers' => $telephone_numbers,
    'email' => $email,
    'services-i-offer' => $services_i_offer,
    'features' => [
        'Speaks English',
        'Specialist',
        'Accepts Health Insurance',
        'Accepts Credit Cards',
        'Available After Hours',
        'Available Weekends',
        'Available Holidays',
        'Home Visitation Services',
        'Telemedicine',
        'Primary Care'
    ],
    'company-mfilter' => "company-$company",
    'address-mfilter' => "address-$address",
    'p-o-box-mfilter' => "p-o-box-$pobox",
    'island-state-province-mfilter' => "island-state-province-$island_state_province",
    'country-mfilter' => "country-$country",
    'telephone-numbers-mfilter' => "telephone-numbers-$telephone_numbers",
    'email-mfilter' => "email-$email",
    'services-i-offer-mfilter' => "services-i-offer-$services_i_offer",
    'features-mfilter' => [
        "features-Speaks English",
        "features-Specialist",
        "features-Accepts Health Insurance",
        "features-Accepts Credit Cards",
        "features-Available After Hours",
        "features-Available Weekends",
        "features-Available Holidays",
        "features-Home Visitation Services",
        "features-Telemedicine",
        "features-Primary Care"
    ]
];

// Serialize the output array
$serialized_output = serialize($output);

// Replace the original data with serialized output
if ($start !== false) {
    $prefix = substr($inputs, 0, $start);
    $add_meta_data[0][0] = $prefix . $serialized_output . '\')';
}

  ```

![Screenshot_2](https://github.com/user-attachments/assets/8c58bd50-e807-4b3f-b774-aee238a1e3ad)


<b>Purpose: </b> Ensures that extra fields data is correctly stored and structured for display in the admin panel.




