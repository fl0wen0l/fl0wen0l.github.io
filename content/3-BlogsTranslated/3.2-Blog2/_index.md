---
title: "Blog 2"
date: "2025-12-01"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---
# AWS Transform announces full‑stack Windows modernization capability

**Author:** Prasad Rao | **Date:** 01 DEC 2025

Earlier this year in May, we announced the general availability of [AWS Transform for .NET](https://aws.amazon.com/transform/net), the first agentic AI service to modernize .NET applications at scale. During the early access period, we received valuable feedback that, beyond modernizing .NET applications, you also want to modernize SQL Server and legacy UI frameworks. Your applications often follow a three-tier architecture—presentation, application, and database—and you need an end‑to‑end solution that can transform all these tiers in a coordinated way.

Today, based on that feedback, we’re excited to announce [AWS Transform for Windows full‑stack modernization](https://aws.amazon.com/transform/windows), which offloads complex, repetitive modernization work across the Windows application stack. You can now identify application and database dependencies and modernize them in a coordinated fashion through a centralized experience.

AWS Transform accelerates full‑stack Windows modernization up to five times across the application, UI, database, and deployment tiers. In addition to migrating .NET Framework applications to cross‑platform .NET, it moves SQL Server databases to [Amazon Aurora PostgreSQL‑Compatible Edition](https://aws.amazon.com/rds/aurora/features/) with intelligent stored procedure conversion and restructures application code that depends on database specifics. For validation and testing, AWS Transform deploys the application to [Amazon Elastic Compute Cloud (Amazon EC2)](https://aws.amazon.com/ec2) on Linux or [Amazon Elastic Container Service (Amazon ECS)](https://aws.amazon.com/ecs/), and provides customizable [AWS CloudFormation](https://aws.amazon.com/cloudformation/) templates and deployment configurations for production environments. AWS Transform also added UI modernization capability from ASP.NET Web Forms to Blazor.

There’s a lot to explore, so in this post I’ll give a first look at AWS Transform’s Windows full‑stack modernization capabilities across all tiers.

## Creating a Windows full‑stack modernization job

AWS Transform connects to your source code repositories and database servers, analyzes application and database dependencies, generates modernization waves, and coordinates full‑stack transformations for each wave.

To get started with AWS Transform, I first completed the onboarding steps in the [Getting started with AWS Transform guide](https://docs.aws.amazon.com/transform/latest/userguide/getting-started.html). After onboarding, I sign in to the AWS Transform console and create a job for Windows full‑stack modernization.

![Create new job for Windows modernization](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/29/1.-createjob-1-2.png)

![Create job by selecting SQL Server database modernization](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/29/1.-createjob-2-2.png)

After creating the job, I completed the [prerequisites](https://docs.aws.amazon.com/transform/latest/userguide/win-full-stack/sql-server-setup.html). Then I configured a [database connector](https://docs.aws.amazon.com/transform/latest/userguide/win-full-stack/sql-server-create-job.html) so AWS Transform can securely access SQL Server databases running on Amazon EC2 and [Amazon Relational Database Service (Amazon RDS)](https://aws.amazon.com/rds/). A connector can access multiple databases on the same SQL Server instance.

![Create new database connector](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/2.-DB-Connector.png)

Next, I set up a [source connector](https://docs.aws.amazon.com/transform/latest/userguide/dotnet-creating-repo-connector.html) to connect to my source code repositories.

![Add source code connector](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/3.-source-code-connector.png)

I also have the option to choose whether I want AWS Transform to deploy transformed applications. I selected **Yes** and provided a target AWS account ID and AWS Region for deployment. The deployment option can be configured later as well.

![Choose whether to deploy transformed apps](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/4.-deploy-apps.png)

After connectors are configured, AWS Transform connects to resources and runs validations to verify IAM roles, network setup, and related AWS resources.

Once validation succeeds, AWS Transform discovers databases and their associated repositories. It determines dependencies between databases and applications to create waves that transform related components together. Based on this analysis, AWS Transform generates a wave‑based transformation plan.

![Start assessment for discovered databases and repositories](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/5.-start-assessment.png)

## Assessing databases and dependent applications

For assessment, I review the discovered databases and repositories and pick appropriate branches for each repo. AWS Transform scans the databases and repositories, then presents a list of databases with their dependent .NET applications and the transformation complexity.

![Start wave planning for assessed databases and repositories](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/6.-start-wave-planning.png)

I select the target databases and repositories to modernize. AWS Transform analyzes those selections and generates a comprehensive **SQL modernization assessment report** with a detailed wave plan. I download the report to review the proposed modernization plan. The report includes an executive summary, wave plan, database‑to‑repo dependency mapping, and complexity analysis.

![View SQL modernization assessment report](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/7.-SQL-Summary-report.png)

## Converting waves at scale

The wave plan generated by AWS Transform includes four steps for each wave. First, it converts the SQL Server schema to PostgreSQL. Second, it migrates data. Third, it transforms dependent .NET application code to be compatible with PostgreSQL. Finally, it deploys the application for testing.

Before converting the SQL Server schema, I can create a new PostgreSQL database or select an existing target database.

![Select or create target database](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/8.-choose-DB-1.png)

After selecting source and target databases, AWS Transform generates a conversion report for my review. AWS Transform converts the SQL Server schema to a PostgreSQL‑compatible structure, including tables, indexes, constraints, and stored procedures.

![Download schema conversion report](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/9.-download-conversion-report.png)

For any schema items that AWS Transform cannot convert automatically, I can handle them manually in the [AWS Database Migration Service (AWS DMS)](https://aws.amazon.com/dms/) console. Alternatively, I can edit them in my preferred SQL editor and update the target database.

After schema conversion completes, I have the option to migrate data—this is an optional step. AWS Transform leverages AWS DMS to move data from my SQL Server instance to the target PostgreSQL instance. I can choose to perform data migration later, after all code conversions complete, or use sample data for testing by loading it into the target database.

![Choose whether to migrate data](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/10.-migrate-data.png)

The next step is code transformation. I specify a target branch where AWS Transform uploads transformed code artifacts. AWS Transform updates the codebase to make the application compatible with the converted PostgreSQL database.

![Specify target branch for transformed codebase](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/11.-target-branch.png)

With this release, Windows full‑stack modernization in AWS Transform supports codebases targeting .NET 6 and above. For codebases still on legacy .NET Framework, I first use [AWS Transform for .NET to migrate them to cross‑platform .NET](https://aws.amazon.com/blogs/aws/aws-transform-for-net-the-first-agentic-ai-service-for-modernizing-net-applications-at-scale/). I’ll expand on that workflow later.

After transformations complete, I can view source and target branches with their transformation status. I can also download and review conversion reports.

![Download conversion report](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/12-download-code-report.png)

## Modernizing .NET Framework applications including the UI tier

A major feature we’re releasing today is UI modernization from ASP.NET Web Forms to Blazor. This complements existing support for converting Razor MVC views to ASP.NET Core Razor views.

As noted earlier, if I have a .NET application on legacy .NET Framework, I first use [AWS Transform for .NET to migrate it to cross‑platform .NET](https://aws.amazon.com/blogs/aws/aws-transform-for-net-the-first-agentic-ai-service-for-modernizing-net-applications-at-scale/). For legacy applications whose UI is built on ASP.NET Web Forms, AWS Transform now modernizes the UI tier to Blazor alongside backend code conversion.

AWS Transform for .NET converts ASP.NET Web Forms projects to Blazor on ASP.NET Core, enabling websites to be migrated to Linux. The UI modernization is enabled by default in AWS Transform for .NET on both the AWS Transform web console and the Visual Studio extension.

During modernization, AWS Transform converts .aspx pages, custom ASCX controls, and code‑behind files into server‑side Blazor components instead of WebAssembly. The following project and file changes are made during conversion:

| From           | To               | Description                                               |
| -------------- | ---------------- | --------------------------------------------------------- |
| *.aspx, *.ascx | *.razor          | .aspx pages and custom .ascx controls become .razor files |
| Web.config     | appsettings.json | Web.config settings become appsettings.json               |
| Global.asax    | Program.cs       | Global.asax logic becomes Program.cs                      |
| *.master       | *layout.razor    | Master pages become layout.razor files                    |

![Illustration of specific project file changes](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/24/web-forms-to-blazor-project-changes.png)

## Other new features in AWS Transform for .NET

Along with UI conversion, AWS Transform for .NET added support for multiple conversion capabilities and improved the developer experience. New features include:

* **Targeting .NET 10 and .NET Standard** – AWS Transform now supports conversions to .NET 10, the latest Long‑Term Support (LTS) release published on 11 Nov 2025. It also supports converting class libraries to .NET Standard. Additionally, AWS Transform is available in the AWS Toolkit for Visual Studio 2026.
* **Editable conversion reports** – After assessment completes, you can now view and customize the conversion plan based on your requirements and preferences. For example, you can update package replacement details.
* **Real‑time conversion updates with estimated time remaining** – Depending on codebase size and complexity, conversions can take some time. You can now follow real‑time status updates with estimated time remaining.
* **Markdown next‑steps** – After conversion completes, AWS Transform now generates a markdown next‑steps file listing remaining tasks to finish the migration. You can use this as an actionable plan to iterate on conversions with AWS Transform or use AI code assistants to complete remaining tasks.

## Things to know

A few additional notes:

* **AWS Regions** – AWS Transform for Windows full‑stack modernization is generally available today in US East (N. Virginia). For region availability and roadmap, see [AWS capabilities by region](https://builder.aws.com/capabilities/).
* **Pricing** – There is currently [no additional charge](https://aws.amazon.com/transform/pricing/) for AWS Transform Windows modernization features. Any AWS resources you create or continue to use in your account as a result of AWS Transform outputs will be billed at standard rates. For limits and quotas, consult the [AWS Transform documentation](https://docs.aws.amazon.com/transform/latest/userguide/transform-limits.html).
* **Supported SQL Server versions** – AWS Transform supports converting SQL Server versions from 2008 R2 through 2022, including Express, Standard, and Enterprise editions. SQL Server must be hosted on Amazon RDS or Amazon EC2 in the same Region as AWS Transform.
* **Supported Entity Framework versions** – AWS Transform supports modernizing Entity Framework 6.3 through 6.5 and Entity Framework Core 1.0 through 8.0.
* **Getting started** – To get started, visit the [AWS Transform guide for Windows full‑stack modernization](https://docs.aws.amazon.com/transform/latest/userguide/win-full-stack/windows-full-stack.html).

— [Prasad](https://www.linkedin.com/in/kprasadrao/)

---

### About the author

**Prasad Rao** is a Principal Partner Solutions Architect at AWS based in the UK. His focus area is .NET application modernization and Windows workloads on AWS. He leverages his experience to assist AWS Partners across EMEA with long‑term technical enablement to build scalable architectures on AWS. He also mentors diverse folks who are new to the cloud and want to get started on AWS.
