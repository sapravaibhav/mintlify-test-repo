# OAS Example Single Project API Documentation

## Modifying Documentation

1. Create a branch with a name that starts with *docci*.

   Naming the branch in this manner ensures that a preview build gets generated whenever you commit to the branch.

2. Make necessary changes and commit. A preview build is automatically generated.

   The link to the preview build is posted to : [#devx-alerts](https://autodesk.slack.com/archives/CTVC3AS9F)

   ![preview](https://git.autodesk.com/cloudplatform-apim/api_documentation/blob/dev/common_content/_static/screen_slack-01.png)

   To check progress and see build log files: https://o01-clc.jenkins.autodesk.com/job/devx/job/docCI/job/release/

   ![jenkins](https://git.autodesk.com/cloudplatform-apim/api_documentation/blob/dev/common_content/_static/screen_jenkins.png)


3. Once changes are in, raise a Pull Request into the Main branch. When the Pull Request is merged, it will automatically update the dev portal. (https://dev.forge.autodesk.com/en/docs/oas_example_single_project/v1/developers_guide/overview/)

## To Promote Docs to Staging and Production

When you merge a Pull Request to the Main Branch, a build gets triggered and a Spinnaker job is created.

1. In [#devx-alerts](https://autodesk.slack.com/archives/CTVC3AS9F), click the link to the Spinnaker job from the build notification.

   ![preview](https://git.autodesk.com/cloudplatform-apim/api_documentation/blob/dev/common_content/_static/screen_slack-02.png)

2. In Spinnaker, use Manual Judgement to first promote to Staging, and then to Production.

   **Note:** You need permission to promote docs to Staging and Production. If you don't have permission Spinnaker will pretend to promote the docs, but will do nothing.

   ![spinnaker](https://git.autodesk.com/cloudplatform-apim/api_documentation/blob/dev/common_content/_static/screen_spinnaker.png)

## To Obtain Permission to Promote to Staging and Production

1. Go to https://myaccess.microsoft.com/@autodesk.onmicrosoft.com#/access-packages/active and check if the `DOC - Development - Commercial - Owner` package is listed under the packages you have access to.

   ![access](https://git.autodesk.com/cloudplatform-apim/api_documentation/blob/dev/common_content/_static/screen_access.png)

2. If `DOC - Development - Commercial - Owner` is not listed, locate the package in the **Available** tab, and click the **Request** link in that row.


**Permission is revoked every 90 days. So, you have to request permission again every 90 days.**

For questions, comments and feature request #aps-docpipeline-support
