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
`Npm init –y`
Install Eleventy
`Npm install @11ty/eleventy --save dev`
Create a basic eleventy project setup with src and dist directories. 

### Function setup
Install the Netlify CLI
`npm install netlify-cli`
Create a simple eleventy config file
`const { EleventyEdgePlugin } = require("@11ty/eleventy");

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





