# GitHub Workflows

## Sync Integration to Stage

This workflow synchronizes container images from the integration environment (`zoocasa-stage-two`) to the stage environment (`zoocasa-stage`). It's useful for promoting tested images from integration to stage.

### Prerequisites

Before using this workflow, ensure you have:

1. Access to the GitHub repository
2. AWS credentials configured in the repository secrets:
   - `AWS_ACCESS_KEY_ID`
   - `AWS_SECRET_ACCESS_KEY`

### Usage

1. Navigate to the "Actions" tab in your GitHub repository
2. Select "Sync Integration to Stage" from the workflow list
3. Click "Run workflow"
4. Provide the following inputs:
   - `service_list`: Comma-separated list of services to sync (e.g., `importer-listings-address,importer-listings-election`)
   - `dry_run`: Set to `true` to preview changes without applying them (optional, defaults to `false`)

### Example

To sync multiple services with a dry run:

```sh
service_list: client-app,client-app-worker,go-search,go-search-indexer,go-search-insights,go-search-sitemaps,importer-listings-address,importer-listings-controller,importer-listings-election,importer-listings-image-observaility,importer-listings-populator,zoocasa-next,zoocasa-next-images
dry_run: true
```

### What the Workflow Does

1. Connects to the integration cluster (`zoocasa-stage-two`)
2. Retrieves current images for specified services
3. Connects to the stage cluster (`zoocasa-stage`)
4. In dry-run mode:
   - Shows current images in stage
   - Shows what would be updated
5. In normal mode:
   - Updates the images in stage to match integration

### Notes

- All services must exist in both clusters
- Services must be in the `stage` namespace in both clusters
- The workflow runs in the `us-east-1` region
- Changes are applied to all specified services in a single run
