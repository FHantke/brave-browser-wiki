The `gn` template `brave_xml_preprocessor` can be used to process XML resources files before it is compiled.

A target `brave_java_xml_preprocess_resources` has already been set up. To add a new XML resource file for pre-processing,

1. Add the XML file path to `brave_java_preprocess_xml_sources` in `android/brave_java_resources.gni`.
2. Add a python file to where you want, and add the path to the python file to `brave_java_preprocess_module_sources` in `android/brave_java_resources.gni`.
3. In the newly added python file, define a function `_ProcessXML(root)` which takes one single argument `root` as the root element of the XML root [etree Element](https://docs.python.org/3/library/xml.etree.elementtree.html#xml.etree.ElementTree.Element). The function shall return the processed root `Element`.
   1. You can do whatever processing you want with the Element. For example, insert a new node by parsing a string using [fromstring(text)](https://docs.python.org/3/library/xml.etree.elementtree.html#xml.etree.ElementTree.fromstring) and insert it at a given position with [insert(index, subelement)](https://docs.python.org/3/library/xml.etree.elementtree.html#xml.etree.ElementTree.Element.insert).
   2. Remember to register namespaces. You will need to at least register the `android` ns by doing
      ```
      ns = { 'android' : 'http://schemas.android.com/apk/res/android' }
      for prefix, uri in ns.items():
        ET.register_namespace(prefix, uri)
      ```
      as well as passing `namespaces=ns` to all [find(match)](https://docs.python.org/3/library/xml.etree.elementtree.html#xml.etree.ElementTree.Element.find) calls.
4. You can then use the processed resource with a `brave_` prefix. For example, `R.main_menu` will now become `R.brave_main_menu`.

See [this branch](https://github.com/brave/brave-core/compare/android_res_xml_parser_test) for a simple example for what you can do.