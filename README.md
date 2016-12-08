# Summary

Running version 2.3.0 of @angular/compiler in PhantomJS results in a runtime error:

> Uncaught SyntaxError: In strict mode code, functions can only be declared at top level or immediately within another function.

The offending code can be found in `@angular/compiler/bundles/compiler.umd.js:26587`:
```js
  JitCompiler.prototype._createCompiledHostTemplate = function (compType, ngModule) {
      if (!ngModule) {
          throw new Error("Component " + stringify(compType) + " is not part of any NgModule or the module has not been imported into your module.");
      }
      var /** @type {?} */ compiledTemplate = this._compiledHostTemplateCache.get(compType);
      if (!compiledTemplate) {
          var /** @type {?} */ compMeta = this._metadataResolver.getDirectiveMetadata(compType);
          assertComponent(compMeta);
          var HostClass_1 = (function () {
              function HostClass_1() {
              }
              HostClass_1.overriddenName = identifierName(compMeta.type) + "_Host";
              return HostClass_1;
          }());
          function HostClass_tsickle_Closure_declarations() {
//        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
              /** @type {?} */
              HostClass_1.overriddenName;
          }
          var /** @type {?} */ hostMeta = createHostComponentMeta(HostClass_1, compMeta);
          compiledTemplate = new CompiledTemplate(true, compMeta.selector, compMeta.type, hostMeta, ngModule, [compMeta.type]);
          this._compiledHostTemplateCache.set(compType, compiledTemplate);
      }
      return compiledTemplate;
  };
```

## Steps to reproduce

1. `npm install`
1. `./node_modules/.bin/phantomjs script.js`

## Verification

Downgrading `@angular/compiler` to 2.2.x fixes this error.

