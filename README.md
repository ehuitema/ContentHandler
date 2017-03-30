# ContentHandler
 
This module creates a content handler to be able to dynamically "host" content files from Mendix, e.g. css and js files in a secured context (i.e. logged in user). 

### Typical usage scenario
Enables custom content on the client after login, e.g. custom css. Ideal in multitenant projects, where specific branding/ styling applies per tenant. Customized styling/ theming per user is possible as well. Content can be anything textual: JSON, css, javascript, html, etc.

### Features and limitations
* Does not support binary content, e.g. images, etc.
* Uses a Microflow to generate the content.
* The Microflow can have parameters, a list of KeyValuePairs, which are processed from the GET url params.
* the url consists of a base path, followed by an alias that maps to a microflow

# Dependencies
* Mendix 5.21.1
* CommunityCommons module - though not strictly necessary, the sample flow that generates css uses a method from CommunityCommons

# Installation
### Prerequisities

### Installation steps
* Import the module **CommunityCommons** in your project
* Import the module **ContentHandler** in your project

# Getting Started

### configuration / how to use
* Add startup flow from \_USE\_ME folder _AS\_StartContentHandler_ to your project's startup. This installs a request handler with the url path "/content". This path name may be altered.
* Add the _ContentAlias\_Overview_ page to your administration menu.
* Implement a Microflow that returns a _ContentDocument_ entity, containing the actual content that will be returned to the client. See _SUB\_GetCSS_ and _SUB\_TestPlainTextContent_ microflows as example.
* Create a _ContentAlias_ configuration record (via admin pages) for each Microflow that delivers content. The ContentAlias links an alias as used in the url path to the created Microflow. The Microflow must be fully qualified, i.e including the module name. The alias is used in the url path, following the handler url path name (e.g. /content/\<alias\>) which in turn triggers the configured Microflow.
* The content from the Microflow can now be returned to the client with a call to the url: (example) **\<your base url\>/content/\<alias\>**. The Microflow is executed in the user's context. Use "Apply entity access" on your Microflow to enforce user's security.
* If you want to use parameters to your Microflow, use request parameters: (example) **\<your base url\>/content/\<alias\>?name=mycontent&lang=nl** etc. These parameters are passed to the Microflow as a list of _KVP_ entities. Each parameter pair results in 1 KVP entity. Note that in the _ContentAlias_ config record, you need to set _ParseRequestParams_ to true to parse the request parameters.
* To enable custom css in your browser, add an HTMLSnippet widget on your page/ layout, set its type to JavaScript and use e.g.
\
    require(["dojo/dom-construct"], function(domConstruct){\
        domConstruct.create('link', { rel: "stylesheet", type: "text/css", href: mx.appUrl + 'content/css'}, dojo.body(), "last");\
    });

# Remarks

# Known bugs

# Links

# License
Licensed under the Apache license.

# Developers notes
* `git clone https://github.com/ehuitema/ContentHandler`

# Version history
