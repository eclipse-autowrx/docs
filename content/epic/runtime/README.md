

This directory contains the documentation for the SDV-Runtime project.

## Documentation Structure

- **Overview** (_index.md) - Introduction and main concepts
- **Architecture** - Details about the system architecture
  - Architecture overview (_index.md)
  - Component details (components.md)
  - Data flows (data-flows.md)
- **Getting Started** - Instructions for using SDV-Runtime
  - Quick start (_index.md)
  - Installation details (installation.md)
  - First application (first-application.md)
- **Reference** - Technical reference material
  - API documentation (api-documentation.md)
  - Configuration options (configuration.md)
  - Troubleshooting (troubleshooting.md)
  - Advanced deployment (deployment.md)

## Building the Documentation

This documentation uses Hugo to generate a static site. To build and preview the documentation:

1. Install Hugo: https://gohugo.io/installation/
2. Run the Hugo server:
   ```bash
   hugo server -D -s ./docs
   ```
3. Open a browser and navigate to http://localhost:1313/

## Contributing to the Documentation

When adding new content:

1. Maintain the existing structure and formatting
2. Include reading guides at the top of each page
3. Add navigation links at the bottom of each page
4. Ensure all pages have complete front matter
5. Run the test script to check for issues before submitting changes
