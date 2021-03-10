# nanocodes-spec

*specification for the nanocodes format*

A *nanocode* is a shortcode. It's similar to:

- shortcodes in WordPress and Hugo
- slash commands in Slack and Notion
- JSX tags

Instead of using one syntax, it uses a data format. It can be either:

- an array containing:
  - the location of the nanocode
  - an object containing keyword arguments as the second parameter
  - positional arguments at the end
- an object containing:
  - the location of the nanocode in a `tag` key
  - positional arguments in the `children` key
  - keyword arguments as the rest of the keys

The data can come from JSX, JSON, JSON5, JSON6, JavaScript literals,
YAML, or anything that is parsed into a syntax tree, such as a
Markdown list or a Python literal.

The tag starts with `https://` or `/`. This avoids something being
run accidentally. It is also expected that an allowlist of nanocode
locations is used. This can be defined in a `.nanocodes.md` for a
project. With JSX, the tag can be embedded into a component.

Here is an example:

```json
[
  "/request/post",
  {"headers": {"authorization": "Bearer abc123"}},
  "https://api.example.com/messages",
  {
    "title": "Test post",
    "body": "Test"
  }
]
```

Here it is as an object:

```json
{
  "tag": "/request/post",
  "headers": {"authorization": "Bearer abc123"},
  "children": [
    "https://api.example.com/messages",
    {
      "title": "Test post",
      "body": "Test"
    }
  ]
}
```

This can also be expressed as the following Markdown list. Note that when
a colon appears in the top level key, it gets pulled into the keyword
arguments, no matter where it occurs:

- /request/post
- `https://api.example.com/messages`
- {}
  - title: Test post
  - body: Test
- headers: {}
  - authorization: Bearer abc123

The trimmed text before and after the colon are used. If you need it to
start or end with a space, use backquotes. If you need something that looks
like a number to be a string, use backquotes with quotes around it. Ditto
if you need an escape sequence inside a string.
