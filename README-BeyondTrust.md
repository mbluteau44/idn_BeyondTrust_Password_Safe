<!DOCTYPE html>
<html>
<body>

<h1>SailPoint IdentityNow SaaS Connectivity  :: BeyondTrust Password Safe</h1>

<h2>PAM and SCIM PAM working example</h2>
  
  The BeyondTrust Password Safe SaaS Connector has been created using <a href="https://developer.sailpoint.com/idn/docs/saas-connectivity/">SailPoint IdentityNow SaaS Connectivity</a>.
  
  Password Safe was added to IdentityNow as the first default PAM Source in March 2022.
  However, a SaaS Connector allows direct Cloud to Cloud communication without requiring a VA to be deployed on-premises, to Password Safe Cloud.

  This SaaS Connector is an example for how to implement a fully working example based on SCIM and SCIM PAM extension.

  For more information on SCIM, see <a href="http://www.simplecloud.info">SCIM</a> and <a href="http://www.simplecloud.info">SCIM PAM</a>

<h3>Supported Use Cases</h3>

- Account Create
- Account Delete
- Account Enable
- Account Disable
- Account List
- Account Read
- Account Update
- Entitlement List
- Entitlement Read
- Test Connection

<h3>Requirements</h3>

IdentityNow v8.3+
SailPoint SaaS Connectivity
BeyondTrust Password Safe Cloud v23.3+

<h3>Configuration Guide</h3>

Once the Password Safe SaaS Connector has been added to our IdentityNow instance, we can use it to create a new Source.

   <img src="assets/images/CreateSource.png" alt="Create Source: BeyondTrust-Password_Safe">
  
  Click Configure Source, provide a Name and Description, then click Continue.

   <img src="assets/images/CreateSource-Name.png" alt="Create Source: BeyondTrust-Password_Safe">
  
  Provide the Base URL and Authentication URL for your Password Safe Cloud instance. Also provide Client ID and Secret for the SCIM service account.
  For more information on creating the IdentityNow SCIM service account, refer to <a href="https://www.beyondtrust.com/docs/beyondinsight-password-safe/bi/integrations/third-party/identity-now.htm">Create Group and Account</a>

   <img src="assets/images/CreateSource-Configuration.png" alt="Create Source: Configuration">
  
  At this point, you should be able to successfully test the connection.
  
   <img src="assets/images/CreateSource-TestConnection.png" alt="Create Source: Test Connection">

 Before we import data, we want to configure a Correlation rule.

   <img src="assets/images/Correlation.png" alt="Correlation">

  
  <h3>IdentityNow API Step: Parent-Child Entitlements</h3>

Note: This step is optional.
  
In order to take advantage of new UI elements in IdentityNow to show Parent-Child relationships between entitlements(Group = Parent, Container = Child) we need to modify the Source Schemas.  This is necessary with the current version of SaaS Connectivity, which does not support configuring Parent-Child relationships via connector-spec.json.

  Step 1: In IdentityNow, click your account name in the upper right corner, then select Preferences.
  Step 2: Select Personal Access Token, and create a new Access Token. Add scopes/permissions including idn:connector-config:manage.
  Step 3: Use Postman to authenticate to the IdentityNow v3 API, and use /v3/sources to find your Source id.
  Step 4: Access your source id with /v3/source/{id} and find the id for the group CONNECTOR_SCHEMA.
  Step 5: Use GET /v3/sources/{source_id}/schemas/{group_id} and copy the response in body.

   <img src="assets/images/Hierarchy-GetGroupSchema.png" alt="GET Group Schema with CONNECTOR_SCHEMA id">
  
  Step 6: Use PUT /v3/sources/{source_id}/schemas/{group_id} with the following 2 modifications to body(replace {group_id}:

   <img src="assets/images/HierarchyAttribute-1.png" alt="PUT Group Schema with CONNECTOR_SCHEMA id">

   <img src="assets/images/HierarchyAttribute-2.png" alt="PUT Group Schema with CONNECTOR_SCHEMA id">

  Now we are ready to Import data. From the Source in IdentityNow, navigate to Account Aggregation and Entitlement Aggregation and Start the Manual Aggregations.
  
   <img src="assets/images/Aggregation-Accounts.png" alt="Aggregation - Accounts">

   <img src="assets/images/Aggregation-Accounts.png" alt="Aggregation - Accounts">

And with the optional Parent-Child Entitlements steps, we should be able to see the relationships between Groups and Containers:

   <img src="assets/images/Child-Entitlements.png" alt="Child Entitlements">

To be continued...


  </body>
  </html>
