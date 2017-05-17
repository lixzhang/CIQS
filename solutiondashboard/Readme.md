# Embedded Power BI Solution Dashboard

Embedded Power BI Solution Dashboard feature enables CIQS solution authors to quickly and seamlessly display deployed solution dashboard on a website provisioned in customer's subscription. Previously, end-users would need to download Power BI Desktop file for the solution, and follow multi-step directions to connect the dashboard to the SQL Server database deployed as part of the solution. This friction point is eliminated by hosting the dashboard in Embedded Power BI workspace collection, displaying the dashboard in a website deployed in end-user's subscription, and automatically connecting Embedded Power BI Dashboard with SQL Server database provisioned during the solution deployment.

# Prerequisites

To integrate with Embedded Power BI Dashboard feature, your solution needs to have the following.

1. Power BI Desktop file (file extension .pbix) with SQL Server **Direct Query** data source.

2. Azure SQL Server and database.

# Sample Pattern

A [sample pattern](/Samples/008-solutiondashboard) is available which demonstrates how to integrate a CIQS solution with Embedded Power BI feature.

# How to Integrate

The first step is to add your Power BI Desktop to your solution's assets directory. We recommend that you create `\assets\dashboards\solution` directory within your solution's folder and place your PBIX file there.

Next, open your solution's Manifest file. Add the snippet below to your solution's provisioning steps. Note that the SQL Server must be deployed and the tables must be created in the steps preceding the "SolutionDashboard" step.

```xml
<ProvisioningSteps>
  ...
  <SolutionDashboard>
    <Parameters>
      <Parameter hidden="true" name="pbixFileUrl" defaultValue="{PatternAssetBaseUrl}/dashboards/solution/dashboard.pbix" />
      <Parameter hidden="true" name="sqlServer" defaultValue="{Outputs.sqlServer}" />
      <Parameter hidden="true" name="sqlDatabase" defaultValue="{Outputs.sqlDatabase}" />
      <Parameter hidden="true" name="sqlServerUsername" defaultValue="{Outputs.sqlServerUsername}" />
      <Parameter hidden="true" name="sqlServerPassword" defaultValue="{Outputs.sqlServerPassword}" />
    </Parameters>
  </SolutionDashboard>
  ...
</ProvisioningSteps>
```

Be sure to set `defaultValue` properties appropriately.

| Parameter     | Description   |
| ------------- |---------------| 
| pbixFileUrl         | Pbix file location (see above). If dashboard.pbix file is placed under `\assets\dashboards\solution`, the value will be {PatternAssetBaseUrl}/dashboards/solution/dashboard.pbix                           |
| sqlServer           | SQL Server URL, ie. sqlserver001.database.windows.net      |
| sqlDatabase         | SQL database name                                          |
| sqlServerUsername   | SQL server username                                        |
| sqlServerPassword   | SQL database password                                      |

Finally, be sure to display a link to the dashboard on the "Deployment Summary" page. Include the snippet below in your solution's `Summary.md` file.

```markdown
You can see your solutions dashboard [here]({Outputs.solutionDashboardUrl}).
```

Opionally, you can also include a preview of the dashboard in the Deployment Summary page by adding the snippet below to your `Summary.md` file.

```html
<iframe width="780" height="480" src="{Outputs.solutionDashboardUrl}" frameborder="0" allowfullscreen></iframe>
```

# References
[Embedded Power BI](https://docs.microsoft.com/en-us/azure/power-bi-embedded/power-bi-embedded-what-is-power-bi-embedded)
