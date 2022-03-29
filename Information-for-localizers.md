## Escaped tags

You may encounter encoded entities (tags). For example, `&lt;b&gt;test&lt;/b&gt;` is the encoding of `<b>test</b>`. Note, that encoded entities start with an ampersand and end with a semicolon. There are no spaces in between any of that. Sometimes, you may encounter incorrectly double-escaped encodings like `&amp;lt;` (instead of `&lt;`). Please, **leave those as they are**.

## Placeholders

Sometimes strings have variables (placeholders) which look like the pictures below.<br/>
<table>
    <tr>
      <td><img width="400" alt="string with placeholders example 1" src="https://user-images.githubusercontent.com/41635752/144468340-2dea8b7c-2174-4d3e-a1bd-58ba694d33ef.png"></td>
      <td><b>or</b></td>
      <td><img width="400" alt="string with placeholders example 2" src="https://user-images.githubusercontent.com/41635752/160643250-0cc70caf-44d3-49d3-a667-64d117951308.png"></td>
    </tr>
</table>

In these cases you should:
  - Use your mouse and click on the placeholder to insert it into the cursor position of your translation. You do not need to modify anything within the placeholder. 
  - The text inside the `ex` tags of the placeholder is an example for you, the translator, of values that would be used in this placeholder.
  - When placeholders contain URLs, you may receive a warning from Transifex about a missing URL:<br/>
    <img width="400" alt="Missing URL warning example" src="https://user-images.githubusercontent.com/41635752/160645779-8c51f18a-d53c-43cb-9242-95825ca2f016.png"><br/>
    You can ignore such warnings.
  - If you do not use Transifex edit for translating, please, see the [Manual handling of placeholders](https://github.com/brave/brave-browser/wiki/Strings-and-Localization#manual-handling-of-placeholders) section below.<br/>

Sometimes you may encounter other types of placeholders such as double square or double squiggly brackets. Do not translate terms inside these placeholders (for example, `[[user]]` or `{{user}}` should be left as is).


## Plural strings

Sometimes strings support single/plural versions. Such strings support [ICU plural rules](http://cldr.unicode.org/index/cldr-spec/plural-rules) and look similar to this:
  <table>
    <tr>
      <td>{COUNT, plural,<br/>
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=0 {<b>Open all in &amp;amp;private window</b>}<br/>
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=1 {<b>Open in &amp;amp;private window</b>}<br/>
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;other {<b>Open all (</b>{COUNT}<b>) in &amp;amp;private window</b>}}<br/>
      </td>
    </tr>
  <table>
Only the bolded text in the above example should be translated. Terms <i>COUNT</i> (or any other term after the initial <i>'{'</i>), <i>plural</i>, <i>other</i> should <b>NOT</b> be translated.

## Escaped characters 

If you see `&#8217;` in the text, that is an encoded apostrophe(`’`). For example, `you&#8217;re` is just `you're` in which case you don't need to preserve this encoded sequence in the translation.
  <br/>Other common codes:
  - `&#8211;` - en dash(`–`)
  - `&#8212;` - em dash(`—`)
  - `&#8216;` - opening single quote(`‘`)
  - `&#8230;` - ellipsis(`…`)
  - **Note:** standalone `&amp;` (or `&amp;amp;`), `&lt;` (or `&amp;lt;`), or `&gt;` (or `&amp;gt;`), if needed in the localized string, should be left as they are. For example, `Bob &amp; Jim`, or `System &gt; Settings` should stay that way.

## Branding

In branded feature names, such as "Brave", "Brave Rewards", and "Brave Ads", do not translate Brave, but **do translate** the feature name ("Rewards", "Ads", etc.).

## Manual handling of placeholders

If you are translating strings containing placeholders outside the Transifex editor the source string would look similar to this:

<img width="300" alt="string with placeholders example" src="https://user-images.githubusercontent.com/831718/50179178-59893f80-02d4-11e9-86e4-585d55fa37ac.png">

When translating:
* Use `<ph` to replace `&lt;ph`
* Leave `name="EXTENSION_NAME"` part as is, then
* Use `>` to replace `&gt;` 
* Leave `$1` (`$2`, etc.) as is
* If present, replace `&lt;ex&gt;` with `<ex>` and `&lt;/ex&gt;` with `</ex>`
* Replace `&lt;/ph&gt;` with `</ph>` 
* **DO NOT replace `&lt;` and `&gt;` when they surround other elements, such as `a` (`&lt;a...` and `&lt;a/&gt;` should not be modified).**

The text inside the resulting `<ex>...</ex>` tags doesn't need to be translated - it's an example for you, the translator, of values that would be used in this variable.<br/>

In the example in the graphic above, the translation should look like this:
      <table>
        <tr>
          <td><b>Source:</b></td>
          <td>&amp;lt;ph name="EXTENSION_NAME"&amp;gt;$1&amp;lt;/ph&amp;gt; (extension ID "&amp;lt;ph name="EXTENSION_ID"&amp;gt;$2&amp;lt;ex&amp;lt;abacabadabacabaeabacabadabacabaf&amp;lt;/ex&amp;lt;&amp;lt;/ph&amp;gt;") is not allowed in Brave.</td>
        </tr>
        <tr>
          <td><b>Translation:</b></td>
          <td>&lt;ph name="EXTENSION_NAME"&gt;$1&lt;/ph&gt;<b> (translate this "</b>&lt;ph name="EXTENSION_ID"&gt;$2&lt;ex&gt;<b>do not translate this</b>&lt;/ex&gt;&lt;/ph&gt;<b>") translate this too.</b></td>
        </tr>
      </table>

As noted above, you may encounter other encoded tags inside the `<ph>` tags: for example, `&lt;ph name="BEGIN_BOLD1"&gt;&amp;lt;b1&amp;gt;&lt;/ph&gt;`. These encoded tags inside `<ph>` tags should be left as they are, in this case resulting in:
     <table>
       <tr>
         <td><b>Source:</b></td>
         <td>&amp;lt;ph name="BEGIN_BOLD1"&amp;gt;&amp;amp;lt;b1&amp;amp;gt;&amp;lt;/ph&amp;gt;</td>
       </tr>
       <tr>
         <td><b>Translation:</b></td>
         <td>&lt;ph name="BEGIN_BOLD1"&gt;&amp;amp;lt;b1&amp;amp;gt;&lt;/ph&gt;</td>
       </tr>
     </table> 
