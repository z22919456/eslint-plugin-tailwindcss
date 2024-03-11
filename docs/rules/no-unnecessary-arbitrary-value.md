# Avoid unjustified arbitrary classnames (no-unnecessary-arbitrary-value)

Arbitrary values are handy but you should stick to regular classnames defined in the Tailwind CSS config file as much as you can.

## Rule Details

Given the default configuration in which `h-auto` exists... There is no need to use an arbitrary classname.

Examples of **incorrect** code for this rule:

```html
<div class="h-[auto]">height</div>
```

Examples of **correct** code for this rule:

```html
<div class="h-auto">height</div>
```

### The rule can handle `0` values with or without their units

Given the default configuration in which `h-0` exists... There is no need to use an arbitrary classname.

Examples of **incorrect** code with `0` based value:

```html
<div class="h-[0%]">Use `h-0` (`0px`) instead</div>
```

Examples of **correct** code with `0` based value:

```html
<div class="h-0">Use `h-0` (`0px`) instead</div>
```

### The rule can handle negative & double negative

Given the default configuration... There is no need to use an arbitrary classname.

Examples of **incorrect** code for negative arbitrary values:

```html
<div class="m-[-1.25rem] -z-[-10]">[Double] negative values</div>
```

Examples of **correct** code for negative arbitrary values:

```html
<div class="-m-5 z-10">[Double] negative values</div>
```

### Options

```js
...
"tailwindcss/no-unnecessary-arbitrary-value": [<enabled>, {
  "callees": Array<string>,
  "config": <string>|<object>,
  "skipClassAttribute": <boolean>,
  "tags": Array<string>,
}]
...
```

### `callees` (default: `["classnames", "clsx", "ctl", "cva", "tv"]`)

If you use some utility library like [@netlify/classnames-template-literals](https://github.com/netlify/classnames-template-literals), you can add its name to the list to make sure it gets parsed by this rule.

For best results, gather the declarative classnames together, avoid mixing conditional classnames in between, move them at the end.

### `ignoredKeys` (default: `["compoundVariants", "defaultVariants"]`)

Using libraries like `cva`, some of its object keys are not meant to contain classnames in its value(s).
You can specify which key(s) won't be parsed by the plugin using this setting.
For example, `cva` has `compoundVariants` and `defaultVariants`.
NB: As `compoundVariants` can have classnames inside its `class` property, you can also use a callee to make sure this inner part gets parsed while its parent is ignored.

### `config` (default: generated by `tailwindcss/lib/lib/load-config`)

By default the plugin will try to load the file returned by the official `loadConfig()` utility.

This allows the plugin to use your customized `colors`, `spacing`, `screens`...

You can provide another path or filename for your Tailwind CSS config file like `"config/tailwind.js"`.

If the external file cannot be loaded (e.g. incorrect path or deleted file), an empty object `{}` will be used instead.

It is also possible to directly inject a configuration as plain `object` like `{ prefix: "tw-", theme: { ... } }`.

Finally, the plugin will [merge the provided configuration](https://tailwindcss.com/docs/configuration#referencing-in-java-script) with [Tailwind CSS's default configuration](https://github.com/tailwindlabs/tailwindcss/blob/master/stubs/defaultConfig.stub.js).

### `skipClassAttribute` (default: `false`)

Set `skipClassAttribute` to `true` if you only want to lint the classnames inside one of the `callees`.
While, this will avoid linting the `class` and `className` attributes, it will still lint matching `callees` inside of these attributes.

### `tags` (default: `[]`)

Optional, if you are using tagged templates, you should provide the tags in this array.

### `classRegex` (default: `"^class(Name)?$"`)

Optional, can be used to support custom attributes

## Further Reading

If there is exactly one equivalent regular classname, this rule will fix the issue for you by replacing the arbitrary classnames by their unique substitutes.

But if there are several possible substitutes for an arbitrary classname, then you can manually perform the replacement.