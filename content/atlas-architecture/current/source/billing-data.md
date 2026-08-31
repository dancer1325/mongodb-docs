# Features for Atlas Billing Data

Tracking and managing cloud spending can be difficult in a large organization. While cost-saving configurations help you proactively manage spending, Atlas also provides tools for you to view and analyze spending. You can:

- Categorize Atlas resources based on your organization's billing needs.
- Leverage billing data to visualize and understand your Atlas spending.
- Pull billing data programmatically to integrate with your FinOps tools for charge-back and accounting purposes within each department and application.

On this page, learn how to use built-in Atlas tools and Atlas billing data to track your cloud spending.

## Apply Resource Tags

You can apply resource tags in Atlas for precise cost allocation by categorizing resources according to departments, projects, or cost centers. You can also group and analyze tagged resources in financial reports, providing a clear and organized view of how various teams or projects contribute to overall cloud spending. To learn more, see configure-resource-tags.

## Access Billing Data Programmatically

The Atlas Administration API provides a REST (Representational State Transfer)\ful interface that allows you to programmatically access your billing data for use with external tools and reports. Combine this feature with resource tags to easily categorize your Atlas spending. You can retrieve all your pending invoices with the Return All Pending Invoices for One Organization API (Application Programming Interface) endpoint. The endpoints response body contains the `results.lineItems.tags` field, which reflect the tags you applied to your resources (such as organizations and clusters). You can then feed this line-item-level billing data into external FinOps tools and reports track your Atlas spending by environment, team, or other tag values.

## Enable Cross-Organization Billing

Atlas allows you to share a billing subscription across many organizations and to pay a single invoice for them. Enable cross-organization billing for easy visibility into Atlas spending for all of your organizations. After you configure a paying organization, you pay invoices for the paying organization that include a list of charges incurred for all linked organizations. To learn more, see cross-org-billing. After you enable cross-organization billing, you can view linked invoices if you have the Organization Billing Admin role or the Organization Owner role for the paying organization.

## Review Your Monthly Cost Visualization

To view your billing data monthly, navigate to the Cost Explorer page. The Cost Explorer provides a granular view of cloud spending in chart and table form, allowing users to analyze costs by clusters, projects, or teams. Historical spending data and customizable filters help identify trends and inefficiencies, enabling better financial decision-making and resource optimization. You can view your usage over the past six months, and access your billing data up to the past 18 months. If your organization uses cross-organizational billing, you can view billing data across all linked organizations. The Billing Cost Explorer filters and groups usage data by organization, project, cluster, and service. Each filter contains a Usage chart with stacked columns representing the total cost incurred each month. Underneath, there is a Usage By Month table that displays the billing data shown in the chart. To learn more, see cost-explorer.

## Review Your Annual Cost Visualization

To view your billing data annually, navigate to the Billing Overview page. This page helps you to understand the costs incurred by your organization's Atlas usage by service, deployment, and project. Each category contains a Usage chart with stacked columns representing the total cost incurred each month. To learm more, see Year-to-Date Usage Chart.

## Review Your Invoice Cost Visualization

To view your billing data as an invoice, click on the Invoice Date or Invoice Period you want to view. This page shows you the costs incurred by your Atlas usage over the invoice period through the Total Usage and By Deployment charts. For the  Total Usage chart, you can filter your usage by service to view charges incurred by a particular Atlas service. For the By Deployment chart, you can view the proportion of your usage incurred by each of your clusters across all your projects. To view line-item charges, see view-past-invoices.

## Create Billing Dashboards

You can visualize your billing data in a MongoDB Charts billing dashboard to help you optimize your Atlas spending. Billing dashboards contain prebuilt charts that help you monitor your Atlas usage in an organization across different categories and periods of time, and MongoDB Charts integrates with Atlas to seamlessly ingest billing data.

![Atlas Billing Dashboard example.](/includes/images/atlas-billing-dashboard.png)

By default, billing dashboards include the following metrics and charts:

- Total spending across the organization
- Biggest spenders in the organization
- Total spending by instance size, project, cluster, product category, or SKU
- Total cost by product category

You can also customize your billing dashboard by applying dashboard filters and adding new charts, including any charts that use tags that you've applied to your billing data. To create or manage a billing dashboard, see billing-dashboards.
