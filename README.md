# Fluent Tools

Little helpers in addition to the built-in tooling to help you work with the new [Fluent](https://docs.servicenow.com/bundle/xanadu-api-reference/page/build/servicenow-sdk/concept/servicenow-fluent.html) (since Xanadu) and [ES Modules](https://docs.servicenow.com/bundle/washingtondc-api-reference/page/script/sdk/concept/servicenow-sdk.html) (since the Washington DC release) in ServiceNow for JavaScript developers using ServiceNow SDK (Now-SDK) with traditional assets such as business rules and script includes.

## ES Module Relationship

Applies to: Washington DC, Xanadu

The ES Modules Relationship creates a new relationship that you can add to the Core UI form view of any record with a script type field on it (e.g. script includes, business rules). It will parse the "require()" calls in the script and show you the referenced ES Modules (from the sys_module table) in a related list. Requires you to modify the view to reference the relationship from any table you wish to use this from.

![Adding ES Module related list to form](./images/add_relationship.png)

## X_Require Script Include

NOTE: This **was broken** before Xanadu Patch 2. You will get a "require not defined" error when trying to call modules from other scopes. A patch is on the way, and when it's released the following will work for you.

### How it works

In order to call a module from ES5 or cross scope it needs a wrapper in the same scope as the module. Luckily it's not necessary to map every export to the new wrapper object. Add this script include to your scope and name it `x_require` (I have also added a UI Action to do this automatically--see the next section if you have installed this scope in your instance).

```javascript
var x_require = function (path) {
  return require(path);
};
```

If you're in another scope and need to call a module in Scope A it may look something like this. To get the path string just open the sys_module record and copy the `path` value.

```javascript
const { methodA } = x_snc_scope_a.x_require(
  "x_snc_scope_a/x-snc-jon-scoped-app/0.0.1/src/server/script.js"
);
const z = methodA(paramB);
gs.info(`Result of methodA: ${z}`);
```

In old-fashioned ES5 it's done like this:

```javascript
var myModule = x_snc_scope_a.x_require(
  "x_snc_scope_a/x-snc-jon-scoped-app/0.0.1/src/server/script.js"
);

var z = myModule.methodA(paramB);
gs.info("Result of methodA: " + z);
```

### Helper UI Action

I created a UI Action to help with this. To generate the `x_require` method if it does not exist already, or to get boiler plate code to use once it does exist, just go to the the module record you wish to call and use the X Require Script Include link.

![X Require Script Include UI Action](./images/x_require_ui_action.png)
