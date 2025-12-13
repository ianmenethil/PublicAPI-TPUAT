# PublicAPI-TPUAT

This repository automatically downloads the Swagger 2.0 JSON specification from [https://apiuat.travelpay.com.au/v2.0/help](https://apiuat.travelpay.com.au/v2.0/help) on a daily basis, converts it to OpenAPI 3.1 format, and splits the OpenAPI spec into multiple files for easier access.

## Files

- `swagger.json`: The original downloaded Swagger 2.0 specification.
- `openapi.json`: The converted OpenAPI 3.1 specification.
- `openapi/`: A folder containing the split OpenAPI 3.1 specification files.

## Automation

The update process runs daily at midnight UTC via a GitHub Actions workflow. It includes error handling for download failures and only commits changes if the content has actually been updated.

## Manual Trigger

To run the update manually (e.g., for testing), navigate to the Actions tab in this repository, select the "Daily API Update" workflow, and click "Run workflow".

## Tools Used

- [Scalar CLI](https://github.com/scalar/scalar) for OpenAPI conversion and splitting.
- GitHub Actions for automation.

