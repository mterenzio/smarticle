Smarticles are data-driven, personalized articles. The goal is to create an easy interface to create templates and queries for using a structured journalism repository within narratives.

All that really means is that it's a tool to personalize articles toward individual readers.

The first step when creating a Smarticle is to define a schema for the data that will be used. This will be a simple text file with some JSON in it. For example, if your Smarticle was using a data type for a quote it might look like this.
```
{
quote: "string",
name: "string",
date: Date
}
```
The name of the field is on the left and the schema type of that field to the right of the colon. 

The permitted SchemaTypes are

    String
    Number
    Date
    Boolean

Additionally, there is a special type called Location to store geo coordinates.

The name of the text file holding your schema is important. It will become the name of this data-type. So, the example above, if you wanted to store quotes with the schema above you would save the above file as quote.js and upload it by dragging it on top the file upload form.

Next, you'll want to add some data to the system. The easiest way to do this is by uploading a CSV file with data that corresponds to a schema you have already created.

Continuing with the quote example above, you might have a CSV file named quote.csv conatining the following:

quote, name, date
"Smarticle is a really cool tool from 45 years in the future, dude!", "Matt Terenzio", 12/16/1968


The first line will always list out the fields that correspond to the schema and the filename will always be the same name as the schema being used but with a ".csv" instead of ".js". Strings should be quoted, especially if they have commas and the system will try to figure out dates as best it can. 

( These will be stored as a BSON Date or UTC datetime so you may have to do some wrangling with dates depending on your use case, but let's not get distracted here. :) )

So, you'll upload this file in the data upload form and if everything worked out, we have some data to play with in our Smarticle.

Next we create the Smarticle particles. Silly, I know.

This includes a handlebars style template and a query for the the data expressed in JSON.

The template might look something like this:
```
<script id="particle1" type="text/particle">
<p>On {{date}}, {{name}} said "{{quote}}".</p>
</script>
```
Hopefully it's obvious what is happening here. Insert the field name from your schema into a typical paragraph or wherever you'd like. When the system processes the template it will insert values into those placeholders based on a query you make. Note that HTML is allowed inside the templates.

The query itself might look like this:
```
<script src="http://smarticle.io/js/particle.js" id="particlesource" type="text/javascript">

		[
			{
			    "id": "particle1",
			    "data": {
			        "type": "quote",
			        "query": {
			            "name": "Matt Terenzio"
			        }
			    }
			}
		]
</script>
```
Knida scary perhaps but it's really just saying this query if for the template with the id of "particle1". The schema type is a "quote" and the query will return quotes with the name field that equals "Matt Terenzio".

When rendered on to an article page it might look like this (inside a paragraph tag):

On December 16, 1968, Matt Terenzio said "Smarticle is a really cool tool from 45 years in the future, dude!".

You might be wondering how the date gets rendered in a nice readable format like that. For that we need a little handlebars block helper. Helpers can handle lots of things and can be rather simple or pretty complicated. We'll likely build some common use cases into the Smarticle framework. For now you can Google "handlebars block helper" for info on it, or just relax in comfort knowing it will be possible.


At the moment, that's the basics of creating a Smarticle. There are a few libararies that need to be present. Namely, JQuery and Handlebars, and beyond that you'd just need to get these tags onto a web page and it should work.

Fission, the backend app, is open source and available on Github by the generosity of the Knight Prototype Fund. So you can also run your own structured journalism repository, if that's your thing.
