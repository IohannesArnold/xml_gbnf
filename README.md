# XML GBNF

This repo contains GBNF files that can be used in llama.cpp (and possibly
elsewhere) to constrain LLM output to be in the XML format.

## Description

By using these grammar files, you can ensure that the output of llama.cpp
conforms to XML syntax. Note that this does not mean that the output is
*valid* XML, or even *well-formed* XML. In other words, the grammar file
alone does not prevent llama.cpp from outputting something like the
following:
```html
<body>
  <html>Here is an example of a web page:
    <html>
    <h1>Welcome to Pandas!</h1></body>
    </html>
  </p>
<br/>
```
Note that not all tags match; the body element is closed early; the LLM
doesn't notice that it's using HTML for content (and thus the tags should be
escaped with character-references), etc. These types of constraints are beyond
the capacity of the llama.cpp grammar engine at present. Knowledge about these
aspects of XML structure has to come from the model itself. But if you have a
model that understands XML, these grammar files can help ensure it has
predictable output.

### Differences between the grammars

The full XML specification contains a whole bunch of stuff that people rarely
use, like CDATA sections, Processing Instructions, Comments, or Entity
Specifications. Therefore this repo will provide three grammars:

 * `xml_simple.gbnf` -- Not the full XML spec, but just enough to get element
 tags, attributes, and character references, which is 90+% of the time all
 anyone is looking for anyway.
 * `xml_1_0.gbnf` -- The full XML 1.0 specification
 * `xml_1_1.gbnf` -- The full XML 1.1 specification

At the moment only `xml_simple.gbnf` is provided in the master branch; the
others are still in development.

## Usage

Invoke llama.cpp with the `--grammar-file <PATH_TO_FILE.gbnf>` flag to use
one of these grammars. If llama.cpp immediately returns a `[end of text]`
response, that is probably because the EOS token has been set to `<|endoftext|>`
, which the model immediately predicts. You can use `--ignore-eos` to prevent
this.

## Contributions and Project Scope

The goal of this repo is simply to maintain the GBNF files. PRs to fix any
errors in the grammar files or to update them with changes in llama.cpp would
be very welcome. Anyone who wants to work on the full XML spec files is also
welcome to contribute.

However, this project intentionally has a small scope of just maintaining the
GBNF grammars. I will not add any additional scripts or other tooling relating
to XML output and LLMs in this repo. I am happy to add a link in this README
to your project though.
