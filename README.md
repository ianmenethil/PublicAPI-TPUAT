# TravelPay APIs - UAT

This repository automatically downloads the Swagger 2.0 JSON specification from [https://apiuat.travelpay.com.au/v2.0/help](https://apiuat.travelpay.com.au/v2.0/help) weekly, adds missing Basic Auth security scheme, converts it to OpenAPI 3.1 format, and splits the OpenAPI spec into multiple files for easier access.

## Files

- `swagger.json`: The original downloaded Swagger 2.0 specification (with Basic Auth added post-download).
- `openapi.json`: The converted OpenAPI 3.1 specification.
- `openapi/`: A folder containing the split OpenAPI 3.1 specification files (including security schemes like Api-Key and BasicAuth).

## Automation

The update process runs weekly on Mondays at midnight UTC via a GitHub Actions workflow. It includes error handling for download failures, adds Basic Auth to the Swagger spec, and only commits changes if the content has actually been updated.

## Manual Trigger

To run the update manually (e.g., for testing), navigate to the Actions tab in this repository, select the "Weekly API Update" workflow, and click "Run workflow".

## Tools Used

- [Scalar CLI](https://github.com/scalar/scalar) for OpenAPI conversion and splitting.
- `jq` for JSON post-processing (adding Basic Auth).
- GitHub Actions for automation.

If you have any questions or need modifications, feel free to open an issue or pull request.
