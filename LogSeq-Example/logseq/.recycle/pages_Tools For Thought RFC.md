- Goals of the Tools for Thought OPML Extension
	- Add additional elements to be used in an OPML outline that represent links and block references.
	- OPML already has the link and include types. (See [Inclusion](http://opml.org/spec2.opml#1629042832000))
	- The new elements are intended for functionality seen in LogSeq or Roam Research where there are potentially multiple tokens within the text of a headline that need to be replaced with a link or the text of another headline.
- The "tft" Namespace
	- OPML supports extensions as long as they are defined in their own namespace. I'm proposing the prefix "tft" (Tools for Thought) and the URI "https://tft.opmlns.org/" which will be the home for the final spec.
- What is a `<tft:link>` element?
	- An `<outline>` can have zero or more `<tft:link>` elements that represent links within the outline text. This does not function like OPML inclusion, it should work like a web browser and open the linked document.
	- There are two required attributes `text` and `href` and an optional `alt` attribute.
	- The `text` attribute represents the text in the outline text that needs to be turned into a link.
	- The `href` attribute represents what document it's linking to. Typically this would be another OPML file.
	- The `alt` attribute represents the displayed text of the link if different from the `text` attribute.
	- Example:
		- ```
		  <outline text="Today I'm meeting with [[Andrew]] to talk about [[OPML]].">
		  <tft:link text="[[Andrew]]" href="Andrew.opml" alt="Andrew" />
		  <tft:link text="[[OPML]]" href="OPML.opml" alt="OPML" />
		  </outline>
		  ```
- What is a `<tft:ref>` element?
	- An `<outline>` can have zero or more `<tft:ref>` elements that represent text replacement within the outline.
	- There are two required attributes `text` and `href`.
	- The `text` attribute represents the text in the outline text that needs to be replaced.
	- The `href` attribute represents what the source of the new text.
		- If `href` points to an OPML or HTML file with no fragment (text after a hashmark), then we use the contents of the page title in the head element. If no title is present, ignore this element.
		- If `href` points to an OPML file with a fragment, then we look for the first `<outline>` element with `tft:id` attribute that matches the fragment and use the contents of the `text` attribute.
		- If there is no matching `tft:id` check for an `id` attribute (without namespace) because in practice OPML isn't strict about extra attributes being namespaced.
		- If `href` points to an HTML file with a fragment, then look for the first element in the document with a matching `id` attribute and use the contents of the element.
	- Example:
		- ```
		  <outline text="08:30 ((61b212cb))">
		  <tft:ref text="((61b212cb))" href="Prompts.opml#61b212cb" />
		  </outline>
		  ```
- What is a `tft:id` attribute?
	- An `<outline>` can have zero or one `tft:id` attribute that is a string that uniquely represents this headline in this document.
	- Example:
		- ```
		  <outline text="What is my top priority today?" tft:id="61b212cb" />
		  ```