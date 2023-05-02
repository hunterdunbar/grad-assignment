# Eleventy Edge Function Tutorial

## Intro
This tutorial will provide an overview of building with Edge functions in Eleventy. Edge functions are javascript functions which are deployed along with the static site and provide the fastest possible method of executing a serverless function in a static site. This allows developers to deliver the benefits of SSGs while still providing dynamic content.  
There are many different applications for using edge functions but a few examples would be:
-	Adding personalized content to a site
-	Adding authentication capability to a site
-	Forms
-	Providing search capability

## Configuration of Files
Edge functions can be added to Eleventy websites using the Eleventy Edge Plugin (EleventyEdgePlugin), which get initialized in the Eleventy config file. 
It is then necessary to define the paths in your project which should call the function. This is done in a netlify.toml file. Toml is another type of markdown language. 
After these steps are complete, the Edge content can be added to templates. 
As long as the edge is define in a template that falls within the routes defined in the toml, the edge function will fire when the page loads. This function can return dynamic data to the page when the page is loaded. 

## Tutorial
### Setup
Initialize Node project
`
   Npm init –y
`
Install Eleventy
`
   Npm install @11ty/eleventy --save dev
`
Create a basic eleventy project setup with src and dist directories. 

### Function setup
Install the Netlify CLI
`
   npm install netlify-cli
`
Create a simple eleventy config file
`
   const { EleventyEdgePlugin } = require("@11ty/eleventy");

   module.exports = function(eleventyConfig) {

    eleventyConfig.addPlugin(EleventyEdgePlugin);

   return {
      dir: {
         input: "src",
         output: "dist"
      }
   };
   };
`
Create a file in the root directory called "netlify.toml". We are going to place our edge function in a page called "functionpage.liquid". So in our toml file we will define a path to /functionpage. Only requests to this path will call the edge. We will call our function "myfirstedge"
`
   [[edge_functions]]
   function = "myfirstedge"
   path = "/functionpage/"
`

If we now just run npx netlify dev, the netlify cli will generate scaffolding for our edge functions. 
`
   npx netlify dev
`
Answer yes to the terminal prompts
`
   ◈ Netlify Dev ◈
   ◈ Ignored general context env var: LANG (defined in process)
   ? Would you like to configure VS Code to use Edge Functions? Yes
   ? A new VS Code settings file will be created at /Users/wdunbar/jamstack/grad-edge/.vscode/settings.json Yes
`

This will generate a directory called netlify/edge-functions. We will create a new file in this directory called myfirstedge.js
`
   import {
      EleventyEdge,
      precompiledAppData,
   } from "./_generated/eleventy-edge-app.js";
   
   export default async (request, context) => {
      try {
      let edge = new EleventyEdge("edge", {
         request,
         context,
         precompiled: precompiledAppData,
   
         // default is [], add more keys to opt-in e.g. ["appearance", "username"]
         cookies: [],
      });
      console.log("We are in the edge");
      edge.config((eleventyConfig) => {
         eleventyConfig.addGlobalData("SomeData", {
            "hello": "world"
         });
      });
   
      return await edge.handleResponse();
      } catch (e) {
      console.log("ERROR", { e });
      return context.next(e);
      }
   };
`

Finally we can add the edge to our functionpage.liquid file

``
   The content outside of the `edge` shortcode is generated with the Build.

   {% edge %}
   The content inside of the `edge` shortcode is generated on the Edge.

   <pre>
   {{ eleventy | json }}
   {{SomeData | json}}
   </pre>
   {% endedge %}
``




