# Fluent Tools

Little helpers in addition to the built-in tooling to help you work with the new [Fluent](https://docs.servicenow.com/bundle/xanadu-api-reference/page/build/servicenow-sdk/concept/servicenow-fluent.html) (since Xanadu) and [ES Modules](https://docs.servicenow.com/bundle/washingtondc-api-reference/page/script/sdk/concept/servicenow-sdk.html) (since the Washington DC release) in ServiceNow for JavaScript developers using ServiceNow SDK (Now-SDK) with traditional assets such as business rules and script includes.

## ES Module Relationship

Applies to: Washington DC, Xanadu

The JS Modules Relationship creates a new relationship that you can add to the form view of any record with a script type field on it (e.g. script includes, business rules). It will parse the "require()" calls in the script and show you the referenced JS Modules in the related list. Requires you to modify the view to reference the relationship from any table you wish to use this from.

![Adding JS Module related list to form](./images/add_relationship.png)
