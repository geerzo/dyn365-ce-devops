# Introduction
The purpose of this project is to provide reference examples of using [Visual Studio Team Service (VSTS)](https://www.visualstudio.com/team-services/) to build and deploy Dynamics 365 based applications.  The reference examples are based on various use cases, including:

* [Primary Release - Dynamics Only.json](https://github.com/devkeydet/dyn365-ce-devops/blob/master/Primary%20Release%20-%20Dynamics%20Only.json) - Deploying a Dynamics 365 solution package and initial data from source control.
* [Primary Release - Dynamics + Azure.json](https://github.com/devkeydet/dyn365-ce-devops/blob/master/Primary%20Release%20-%20Dynamics%20%2B%20Azure.json) - Builds on [Primary Release - Dynamics Only.json](https://github.com/devkeydet/dyn365-ce-devops/blob/master/Primary%20Release%20-%20Dynamics%20Only.json) and adds deployment of Azure components to the release to demonstrate deploying a release across both.

The intent of this project is to evolve (over time) to implement additional reference examples based on common and/or requested use cases.  Lessons learned from this project have resulted in contributions to https://github.com/waelHamze/xrm-ci-framework/.  Please feel free to suggest new scenarios / starter templates or other ideas via the [issues](https://github.com/devkeydet/dyn365-ce-devops/issues) section.  

These release templates point to a sample application that spans Dynamics 365 & Azure.  The source code that will be built and deployed by VSTS is available at:
https://github.com/devkeydet/CrmAsyncRequestResponseSampleV2

The source code in the repository above shows how to source control *both* code and configurations associated with a Dynamics 365 demo/poc/sample.

The reference build definition in this repository ([Primary Build.json](https://github.com/devkeydet/dyn365-ce-devops/blob/master/Primary%20Build.json)) shows how to use VSTS to:
* Build assets from a remote GitHub repository
    * This, of course, could be done from a VSTS repository as well depending on whether the source code should be public or private
* Build all the assets from source control into their deployable form
    * Solution package zip files
        * Packages Dynamics 365 configuration xml files using [Solution Packager](https://msdn.microsoft.com/en-us/library/jj602987.aspx)
    * Starter data import zip files
        * Zips up xml files produced by the [Configuration Migration](https://technet.microsoft.com/library/dn647421.aspx) tool to setup sample data
    * .NET dlls for both CRM and an [Azure Function App](https://azure.microsoft.com/en-us/services/functions/)
    * etc.

The reference release definitions show how to deploy assets from the build to the target environment(s).  Each does the following:
* Primary Release - Dynamics Only.json
    * Resets a Dynamics 365 instance by using the [Online Management API for Dynamics 365 Customer Engagement](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/online-management-api)
    * Deploys the Dynamics 365 package and sample/intiial data using [Package Deployer](https://msdn.microsoft.com/en-us/library/dn688182.aspx)
        * Demonstrates how to activate plugins *after* the sample/initial data is imported
    * Updates plugin [Secure Configuration](https://us.hitachi-solutions.com/blog/use-secure-vs-unsecure-configuration-plugins/) since it doesn't get stored in the solution package and can vary by environment    
* Primary Release - Dynamics + Azure.json
    * Registers and Azure AD application so the code hosted in Azure has permissions to make web api calls to Dynamics 365
    * Resets a Dynamics 365 instance by using the [Online Management API for Dynamics 365 Customer Engagement](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/online-management-api)
    * Deploys an [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) [template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#template-deployment) to provision necessary Azure resources
    * Gets configuration data from the Azure deployment for use in latter parts of the deployment process
    * Applies configuration settings to the Azure environment
    * Deploys the Dynamics 365 package and sample data using [Package Deployer](https://msdn.microsoft.com/en-us/library/dn688182.aspx)
    * Updates plugin [Secure Configuration](https://us.hitachi-solutions.com/blog/use-secure-vs-unsecure-configuration-plugins/) since it doesn't get stored in the solution package and can vary by environment

# Getting Started
TODO: Video walkthrough of importing the json files into VSTS plus additional manual steps to get the build and release working.
