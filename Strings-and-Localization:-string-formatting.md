First, please refer to the parent document [Strings and Localization](https://github.com/brave/brave-browser/wiki/Strings-and-Localization) for how to add your strings to the repo and get them translateable.

### Links, Text formatting, Value replacement
We cannot assume that all languages have the same syntax. For example we can have the following sentence in which we want the verb to be the link:

> The cat will [jump](#nothing) on the table

Other languages will split the sentence up roughly the same:

> Le chat va [sauter](#nothing) sur la table

In order to assemble the code for this string whilst including the click action on the verb (`jump`), it may be tempting to split the string up in to 3 (hint: don't do this):

> 1. The cat will
> 2. jump
> 3. on the table

In such a scenario, we could then create 3 text UI elements, and simply have the second one styled as a link and attach a click handler to it.

**DO NOT DO THAT**

Here's an example why, in Irish: 

> [Léimfidh](#nothing) an cat ar an mbord

In this language, the verb ("jump") comes _before_ the subject ("The cat"). If we assembled the separately-translated strings according to the English syntax order, the sentence would not make sense. The same and more severe issues will happen in many other languages.

Another issue with separately-translated string parts is that language changes depending on the full context of the sentence of paragraph - sometimes just spelling, and sometimes the entire word where there may be multiple translations for the same English source word depending on the context.

## Preserving language syntax
Instead, a good way is to use string tokens and replacement to allow the full string to be translated:

> The cat will `<ph name="LINK_BEFORE" desc="Placed before the action word">$1</ph>`[jump](#nothing)`<ph name="LINK_AFTER" desc="Placed after the action word">$2</ph>` on the table

The Placeholder Tag (`<ph`) can be moved around by translators to the appropriate place where the verb will be translated.
We can now split the string up to multiple strings in order to style and wire-up the strings in the correct parts:
1. Everything before the action start
2. The action
3. Everything after the action end

In chromium this could be done in c++ with the Localization Utilities, or in JS. Here's an example:
```
    const text = getLocale('stringKey')
    const actionIndex: number = text.indexOf('$1')
    const actionEndIndex: number = text.indexOf('$2')
    const beforeActionText = text.substring(0, actionIndex)
    const actionText = text.substring(actionIndex + 2, actionEndIndex)
    const afterActionText = text.substring(actionEndIndex + 2)
```

We could do anything we like with these string fragments, including putting them in separate components:

```
const TranslatedText = (
  <>
    {beforeActionText}
    <button>{actionText}</button>
    {afterActionText}
  </>
)
```

### Placeholder values
Any text inside a `<ph></ph>` tag will be preserved and not translated. In the above example, we used `$1` and `$2` for easy replacement. It can be tempting to use actual markup values, especially if the string is for HTML-based UI (and not, for example, native UI controls). For example: `<ph>%lt;a href="https://somewhere"%gt;</ph>`.
There are at least three reasons to avoid this temptation:
1. Consider that we may want to change this constant value, which would unfortunately invalidate all translations of that entire string unneccessarily. 
2. The string would now be specifically tied to a single UI framework implementation. If it needs to be re-used for native and web UI, it becomes complicated to strip out the HTML.
3. These strings would have to be injected as HTML into the UI, which opens up a large security risk.
Instead, using simple tokens (`$1`, `$n`, `|`) ensures re-usability and simple code.

There are further documentation surrounding more advanced topics such as numerical values, pluralization, which can be found in the chromium source. Documentation link not found yet!