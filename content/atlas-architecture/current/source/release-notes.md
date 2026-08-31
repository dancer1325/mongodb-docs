# Atlas Architecture Center Release Notes

This page lists the changes introduced in each new version of the Atlas Architecture Center.

## v20251125

**Released 25 November, 2025**

- Updates all Terraform examples to use MongoDB Atlas Terraform Provider version 2.x (`~> 2.2`). If you're upgrading from provider version 1.x, see the [MongoDB Atlas Terraform Provider 2.0.0 Upgrade Guide](https://registry.terraform.io/providers/mongodb/mongodbatlas/latest/docs/guides/2.0.0-upgrade-guide) for breaking changes and migration steps. Examples now use the `mongodbatlas_advanced_cluster` resource with v2.x syntax.
- Adds the arch-center-compliance-gdpr page.

## v20250829

**Released 29 August, 2025**

- Adds opinionated Multi-Region guidance.
- Adds a new Reliability section.
- Enhances the Getting Started section with a new Operational Readiness Checklist.
- Enhances Security guidance.

Specific page and section updates:

- arch-center-landing-zone page: Content reorganization for clarity, and updates to add that the primary is mapped *randomly* to a zone, and link to new guidance pages.
- arch-center-hierarchy page: Adds organization creation example for Terraform.
- arch-center-network-security page: Minor update to mention limitation for changing the number of service attachments and recommendation to use the DNS seedlist connection string.
- arch-center-compliance page: Minor update to introduce [Atlas for Government](https://www.mongodb.com/products/platform/atlas-for-government) as an Atlas compliance option.
- arch-center-high-availability page: Adds guidance for 3- and 5-node replica set topologies.
- arch-center-dr page: Minor update to recommend multi region clusters and electable nodes for high availability, and link to other pages with existing guidance.
- arch-center-cost-saving-config page: Minor update to link to cost-analysis guidance for different deployment topologies.
- Adds the following new pages:

   - arch-center-migration: Describes how to migrate data from your on-premises MongoDB deployments to Atlas.
   - arch-center-checklist: Provides a checklist to help you prepare your environment and team for a successful Atlas deployment.
   - arch-center-paradigms: Introduces and compares different Atlas deployment paradigms, which are described with use-cases in the following new pages:

      - arch-center-paradigms-single
      - arch-center-paradigms-multi-region
      - arch-center-paradigms-multi-cloud
      - arch-center-paradigms-hybrid
- Adds the arch-center-compliance-dora page.

## v20250317

**Released 17 March, 2025**

- arch-center-hierarchy page: Minor update to add guidance for when to consider multiple clusters per project and managing local Atlas deployments using containers.
- arch-center-high-availability page: Minor update to add more guidance for high availability and link to other pages with existing guidance.
- arch-center-automation page: Minor update to explain the advantages of using Atlas's IaC (Infrastructure as Code) tools.
- arch-center-automation page: Minor update to add info about dashboards.

## v20250228

**Released 28 Feb, 2025**

- Releases the first version of the Atlas Architecture Center, including guidance to:

   - Design a landing zone.
   - Set up a single-region, single-cloud Atlas architecture in alignment with the [Well-Architected Framework](https://www.mongodb.com/resources/products/capabilities/well-architected-framework), including example commands for the Atlas CLI and Terraform. To get started, see arch-center-hierarchy.
