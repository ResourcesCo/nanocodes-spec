# nanocodes-spec

*specification for the nanocodes format*

## Intro

A *nanocode* is a shortcode. It's similar to:

- shortcodes in WordPress and Hugo
- slash commands in Slack and Notion
- JSX tags

Instead of using one syntax, it uses a data format. It is a JSON-like
document with the `$fn` key containing the location of the nanocode
function.

The key feature of the format is being able to combine separate
block elements in Markdown into a single data object. The locations
of the fragment elements within the data object are defined using
[JSON pointers](https://tools.ietf.org/html/rfc6901).

## Function Location

The nanocode function location can be an absolute or relative URL.
For example, these are valid nanocode locations:

- `https://nanocodes.dev/sql/pg`
- `/sql/pg`
- `./sql/pg`

## Document Element

The nanocode document element contains an `$fn` key with the
location of the nanocode function.

The document element can be JSON, YAML, JSON5, JSON6, JavaScript
literals, or anything that is parsed into a syntax tree, such
as a Markdown list or a Python literal. It is up to the tool
which are supported. Requiring the `$fn` key prevents a Nanocode
Function from being invoked by arbitrary data within a Markdown
document.

The document element can be standalone, or it can have data
from *fragment elements*.

Data can be gathered from fragment elements that have the same
heading value as the document element. The heading value is
identified by the first line number of the Markdown heading, or
zero if there is no heading, or the first line number of the
previous nanocode document element if there is more than one
nanocode document element within the same heading level.

## Fragment Elements

A fragment element has a JSON pointer with the document URL.
It is identified by a paragraph containing a single inline
code block with the JSON pointer that starts with `#/`. The
`#/` is recognizable as a JSON pointer and used in the popular
JSON schema format, and like `$fn`, is not commonly used in
Markdown at present and is unlikely to be accidentally invoked.
The paragraph containing only an inline code block is also
chosen because it isn't very widely used.

## Examples

Here is an example of an SQL query.

`{"$fn": "https://nanocodes.dev/sql/pg"}`

`#/query`

```sql
INSERT INTO cities (name, population) VALUES ($name, $pop)
```

`#/params`

```json
[
  {"name": "New York City-Newark-Jersey City, NY-NJ-PA MSA", "pop": 19216182},
  {"name": "Los Angeles-Long Beach-Anaheim, CA MSA", "pop": 13214799},
  {"name": "Chicago-Naperville-Elgin, IL-IN-WI MSA", "pop": 9458539},
  {"name": "Dallas-Fort Worth-Arlington, TX MSA", "pop": 7573136},
  {"name": "Houston-The Woodlands-Sugar Land, TX MSA", "pop": 7066141},
  {"name": "Washington-Arlington-Alexandria, DC-VA-MD-WV MSA", "pop": 6280487},
  {"name": "Miami-Fort Lauderdale-West Palm Beach, FL MSA", "pop": 6166488},
  {"name": "Philadelphia-Camden-Wilmington, PA-NJ-DE-MD MSA", "pop": 6102434},
  {"name": "Atlanta-Sandy Springs-Alpharetta, GA MSA", "pop": 6020364},
  {"name": "Phoenix-Mesa-Chandler, AZ MSA", "pop": 4948203},
  {"name": "Boston-Cambridge-Newton, MA-NH MSA", "pop": 4873019},
  {"name": "San Francisco-Oakland-Berkeley, CA MSA", "pop": 4731803},
  {"name": "Riverside-San Bernardino-Ontario, CA MSA", "pop": 4650631},
  {"name": "Detroit–Warren–Dearborn, MI MSA", "pop": 4319629},
  {"name": "Seattle-Tacoma-Bellevue, WA MSA", "pop": 3979845},
  {"name": "Minneapolis-St. Paul-Bloomington, MN-WI MSA", "pop": 3654908},
  {"name": "San Diego-Chula Vista-Carlsbad, CA MSA", "pop": 3338330},
  {"name": "Tampa-St. Petersburg-Clearwater, FL MSA", "pop": 3194831},
  {"name": "Denver-Aurora-Lakewood, CO MSA", "pop": 2967239},
  {"name": "St. Louis, MO-IL MSA", "pop": 2803228}    
]
```

This would pass the same information to the nanocode function as:

```json
{
  "query": "INSERT INTO cities (name, population) VALUES ($name, $pop)",
  "params": [
    {"name": "New York City-Newark-Jersey City, NY-NJ-PA MSA", "pop": 19216182},
    {"name": "Los Angeles-Long Beach-Anaheim, CA MSA", "pop": 13214799},
    {"name": "Chicago-Naperville-Elgin, IL-IN-WI MSA", "pop": 9458539},
    {"name": "Dallas-Fort Worth-Arlington, TX MSA", "pop": 7573136},
    {"name": "Houston-The Woodlands-Sugar Land, TX MSA", "pop": 7066141},
    {"name": "Washington-Arlington-Alexandria, DC-VA-MD-WV MSA", "pop": 6280487},
    {"name": "Miami-Fort Lauderdale-West Palm Beach, FL MSA", "pop": 6166488},
    {"name": "Philadelphia-Camden-Wilmington, PA-NJ-DE-MD MSA", "pop": 6102434},
    {"name": "Atlanta-Sandy Springs-Alpharetta, GA MSA", "pop": 6020364},
    {"name": "Phoenix-Mesa-Chandler, AZ MSA", "pop": 4948203},
    {"name": "Boston-Cambridge-Newton, MA-NH MSA", "pop": 4873019},
    {"name": "San Francisco-Oakland-Berkeley, CA MSA", "pop": 4731803},
    {"name": "Riverside-San Bernardino-Ontario, CA MSA", "pop": 4650631},
    {"name": "Detroit–Warren–Dearborn, MI MSA", "pop": 4319629},
    {"name": "Seattle-Tacoma-Bellevue, WA MSA", "pop": 3979845},
    {"name": "Minneapolis-St. Paul-Bloomington, MN-WI MSA", "pop": 3654908},
    {"name": "San Diego-Chula Vista-Carlsbad, CA MSA", "pop": 3338330},
    {"name": "Tampa-St. Petersburg-Clearwater, FL MSA", "pop": 3194831},
    {"name": "Denver-Aurora-Lakewood, CO MSA", "pop": 2967239},
    {"name": "St. Louis, MO-IL MSA", "pop": 2803228}    
  ]
}
```
