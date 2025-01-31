# Fluent Tools

Little helpers in addition to the built-in tooling to help you work with the new [Fluent](https://docs.servicenow.com/bundle/xanadu-api-reference/page/build/servicenow-sdk/concept/servicenow-fluent.html) (since Xanadu) and [ES Modules](https://docs.servicenow.com/bundle/washingtondc-api-reference/page/script/sdk/concept/servicenow-sdk.html) (since the Washington DC release) in ServiceNow for JavaScript developers using ServiceNow SDK (Now-SDK) with traditional assets such as business rules and script includes.

## ES Module Relationship

Applies to: Washington DC, Xanadu

The ES Modules Relationship creates a new relationship that you can add to the Core UI form view of any record with a script type field on it (e.g. script includes, business rules). It will parse the "require()" calls in the script and show you the referenced ES Modules (from the sys_module table) in a related list. Requires you to modify the view to reference the relationship from any table you wish to use this from.

![Adding ES Module related list to form](./images/add_relationship.png)

## X_Require Script Include

### How it works

In order to call a module from ES5 or cross scope it needs a wrapper in the same scope as the module. Luckily it's not necessary to map every export to the new wrapper object. Add this script include to your scope and name it `x_require`.  If you have installed this application into your instance you'll find the sample code in a script include called `global.x_require`.  Just copy and paste it into your scope and update the API_NAME constant.

```javascript
const x_require = (function() { 
    if (typeof require == "undefined") {
        return function(path) {
	    // **Use the API Name value of this script**
	    const API_NAME = "global.x_require"; 
            var nowGr = new GlideRecord("sys_script_include");
            nowGr.get("api_name", API_NAME);
            nowGr.script = "require('" + path + "')";

            var evaluator = new GlideScopedEvaluator();
            var obj = evaluator.evaluateScript(nowGr, "script");

            return obj;
        };
    } else {
        return function(path) {
            return require(path);
        };
    }
})();
```

If you're in another scope and need to call a module in "Scope A" it may look something like this. To get the path string just open the sys_module record and copy the `path` value.

```javascript
const { methodA } = x_snc_scope_a.x_require("x_snc_scope_a/x-snc-jon-scoped-app/0.0.1/src/server/script.js");
const z = methodA(paramB);
gs.info(`Result of methodA: ${z}`);
```

In old-fashioned ES5 it's done like this:

```javascript
var myModule = x_snc_scope_a.x_require("x_snc_scope_a/x-snc-jon-scoped-app/0.0.1/src/server/script.js");

var z = myModule.methodA("Hello world!");
gs.info("Result of methodA: " + z);
```
