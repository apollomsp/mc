# Microsoft Message Center Collector

An automated GitHub Actions workflow that collects Microsoft 365 service announcements and health messages from the Message Center and stores them in a public repository for consumption by *DeltaPulse*.

## Overview

This repository automatically fetches Microsoft 365 Message Center data every 8 hours using the Microsoft Graph API and stores it as a JSON file. The collected data includes service announcements, maintenance notifications, and other important Microsoft 365 service communications.

## Features

- 🔄 **Automated Collection**: Runs every 8 hours via GitHub Actions
- 📄 **Pagination Support**: Handles large datasets by fetching all pages of results
- 🔐 **Secure Authentication**: Uses Azure AD app registration with client credentials
- 📊 **Clean Data Format**: Stores only the message array for easy consumption

## Data Format

The collected data is stored in `service-messages.json` as a JSON array containing Microsoft 365 Message Center items. Each message includes:

- Message ID and classification
- Service affected (Exchange, SharePoint, Teams, etc.)
- Message title and body
- Start and end times
- Severity level
- Tags and additional metadata

## Usage

### For DeltaPulse Integration

This repository serves as a data source for DeltaPulse. The JSON file can be consumed directly:

```
https://raw.githubusercontent.com/[username]/mc/main/service-messages.json
```

### Manual Triggering

You can manually trigger the workflow from the GitHub Actions tab if needed.

## Setup

### Prerequisites

1. Azure AD app registration with the following permissions:
   - `ServiceMessage.Read.All` (Application permission)
2. GitHub repository with the following secrets configured

### Required Secrets

Configure these secrets in your GitHub repository settings:

| Secret | Description |
|--------|-------------|
| `TENANT_ID` | Your Microsoft 365 tenant ID |
| `CLIENT_ID` | Azure AD app registration client ID |
| `CLIENT_SECRET` | Azure AD app registration client secret |

### Azure AD App Registration Setup

1. Go to the [Azure Portal](https://portal.azure.com)
2. Navigate to **Azure Active Directory** → **App registrations**
3. Create a new app registration
4. Add the following API permissions:
   - Microsoft Graph → Application permissions → `ServiceMessage.Read.All`
5. Grant admin consent for the permissions
6. Create a client secret and note the values

## Workflow Details

The GitHub Actions workflow (`CollectItems.yml`) performs the following steps:

1. **Authentication**: Obtains an access token using client credentials flow
2. **Data Collection**: Fetches all Message Center items with pagination support
3. **Data Processing**: Extracts and consolidates message arrays from all pages
4. **Storage**: Saves the data as `service-messages.json` in the repository root
5. **Version Control**: Commits changes only when new data is available

## Schedule

The workflow runs automatically:
- Every 8 hours (00:00, 08:00, 16:00 UTC)
- Can be manually triggered via GitHub Actions interface

## Contributing

This is an automated data collection repository. For issues or feature requests related to DeltaPulse integration, please refer to the main DeltaPulse project.

## License

This project is open source and available under the [MIT License](LICENSE).

## Related Projects

- [DeltaPulse](https://github.com/deltapulse) - The main project that consumes this data

---

**Note**: This repository is designed for automated data collection. The `service-messages.json` file is automatically updated and should not be manually edited.