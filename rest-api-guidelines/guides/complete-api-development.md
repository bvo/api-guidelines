# Complete API Development

1. **Design the API** 1. Analyze business requirements 2. Identify affordances

   > e.g.: Create user, Submit order, Search for an article

   1. Identify resources

      > e.g.: User, Order, Article

   2. Identify relations

      > e.g.: User has many Orders via order relation, all of the required affordances should be mapped to relations.

   3. Formalize the design in Akwatype
   4. Akwatype will generate the [Open API Specification](http://swagger.io/specification/) \(OAS, formerly known as "Swagger"\) format
   5. Follow the [API guidelines](https://adidas.gitbook.io/api-guidelines/introduction/readme)
   6. Review the API Design
   7. Put the OAS file in a version control system \(VCS\) repository

2. **Develop the API** 1. Check out the VCS repository with the OAS file 2. Set up the [Dredd API testing tool](https://github.com/apiaryio/dredd) locally 3. Configure the Dredd for your project

   ```text
       $ dredd init
   ```

   1. Run the Dredd test locally

      > Against locally running API implementation, Every test should fail.

   2. Implement the API

      > Keep running the Dredd locally to see the progress.

   3. Set up a [CI/CD pipeline](https://adidas-group.gitbooks.io/api-guidelines/content/guides/api-testing-ci-environment.html) to execute the Dredd tests automatically

      > NOTE: Both TeamCity and Jenkins environments are available, contact API evangelist for details.

3. **Deploy the API** 1. Deploy the service 2. Update the OAS file to add the deployment HOST

   > ```text
   > host: api.mashery.com
   > basePath: /demo-approval-api
   > ```

   1. Verify the deployment with Dredd

      > Use Dredd pointed towards the deployment host, be careful NOT to run it against the production OR using real production credentials.

   2. Monitor the API usage "From performance and technical standpoint."

4. **Expose the API using Mashery** 1. **API** 1. Create new API Definition in Mashery 2. Create a new API Endpoint the API Definition

   > Set the "Your Endpoint Address" to the internal deployment HOST.

   1. Create a new API Package in Mashery
   2. Create a new API Plan within the API Package
   3. Use Mashery API Designer to add the newly created API Definitions' API Endpoint to the API Plan
   4. Revisit the API Plan's API key default settings
   5. Revisit the API Plan's API default rate limits
   6. Revisit the API Plan's access policy/authorization
   7. **API Documentation** 1. Create new API developer's portal page in the Mashery

      > Manage &gt; Content &gt; Documentation &gt; APIs

      1. [Embed Apiary documentation](https://help.apiary.io/tools/embed/#apiary-embed-api-reference) on the newly created API Page
      2. Revisit the API documentation access policy/authorization

5. **Use the API**

   > This step can be done at the same time as "Develop the API" thank Apiary hosted Mock, Inspector, and Documentation.

   1. Read API documentation at Apiary
   2. Use API mock service provided by Apiary
   3. Use API call inspector provided by Apiary
   4. Obtain your API key

      > The key is part of the API Plan created in Mashery and can be requested from your dashboard in the API developer's portal.

   5. When available use API implementation via Apiary proxy to debug the API calls
   6. Use production deployment

6. **Analyze the API** 1. Examine the use of production API Using Mashery 2. Analyze the technical performance metrics 3. Collect the feedback from users
7. **Update API Design**

   > Based on the analysis, new or changing business requirements

   1. Create a new branch in the VCS repository with OAS file
   2. Create a new project \(alternative\) in Apiary
   3. Make sure the CI/CD pipeline is:
      1. Set to push the OAS file to Apiary but make sure to modify the Apiary project name
      2. Set to run Dredd test in the CI/CD
   4. Modify the design \(OAS file\) accordingly, follow the "Design API" step
   5. Follow the [**rules for extending**](https://adidas-group.gitbooks.io/api-guidelines/content/core-principles/rules-for-extending.html) and [**API guidelines versioning policies**](../evolution/versioning.md)
   6. Use VCS pull request \(PR\) to propose the change to review
   7. After the API Design change is verified, reviewed and approved, continue with the "Develop the API" phase
   8. Eventually, when the updated design is ready to be deployed for production, merge the branch into the production branch
   9. Follow this guide from "Expose the API using Mashery" step

