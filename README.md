# How to create an Umbraco v7 Package

1) Open up Visual Studio 2019
2) Create a new project for WEB
3) Choose ASP.NET Web Application(.NET Framework)
	- Give it a project name with a .Web (example: MarkDownEditor.Web)
	- Set the location of the git repo
	- Make sure the famework is .NET Framework 4.5.2
4) In Visual Studio 2019 open up neget package manger console (Tools > Nuget Package Manager > Package Manager Console)
5) Type in: Install-Package UmbracoCms -Version 7.15.1
6) Once Umbraco 7 is done installing:
	- Build >  Rebuild Solution
7 Build Umbraco 7 site and go to localhost:
	- Ctl > f5
	- Use the same username and password (u: admin@admin.com p: 0123456789)
	- Click Install (you want the default install) 
8) Go back to property Editor in Visual Studio and rename the solution
	- Right click on the project name (example:MarkDownEditor.Web)
	- Choose Rename
	- Remove the .Web
9) Next add a class Library
	- Right click on the project and choose Add > New Project
	-  C# > Class Library (.NET Framework)
	- Give it a name
	- Set the location of the git repo
	- Make sure the famework is .NET Framework 4.5.2
	- Click Create
10) Remove the default class (Class1.cs)
11) Commit/Push back into github:
12) Open Web.config and set debug=true
13) CTRL + F5 on the .Web project and that will open up Umbraco
14) Go to the backoffice /umbraco
15) Login (u: admin@admin.com p: 0123456789)
16) Create a folder called 'App_Plugins' in the class project (example:MarkDownEditor)
17) Make sure to show all files in the Solution Explorer
18) Create a new folder in the App_Plugins's folder named the same name as your package.
19) Inside the package folder create a js and css folder
20) Add a package.manifest file:
	- Add > New Item > General > Application Manifest File 
	- name it 'package.manifest'
	- Click Add
	- delete all the code inside the file it's not needed.
21) Add a HTML file:
	- Add > New Item > Web > HTML File
	- Name it the same as the package ( Could be a better way?)
	- Click Add
22) Add a CSS file inside the css folder:
	- Add > New Item > Web > Style Sheet
	- Name it the (package name).styles.css ( Could be a better way?)
	- Click Add
23) Add a JS file inside the js folder:
	- Add > New Item > Web > JavaScript File
	- Name it the (package name).controller.js ( Could be a better way?)
	- Click Add
24) Modify the package.manifest file:
Refference: https://our.umbraco.com/documentation/extending/property-editors/package-manifest
Example:
```
{
  "propertyEditors": [
    {
      "alias": "MarkDownEditor",
      "name": "Mark Down Editor",
      "icon": "icon-smiley-inverted",
      "group": "media",
      "editor": {
        "view": "~/App_Plugins/MarkDownEditor/MarkDownEditor.html"
      }
    }
  ],
  "javascript": [
    "~/App_Plugins/MarkDownEditor/MarkDownEditor.controller.js"
  ],
  "css": [
    "~/App_Plugins/MarkDownEditor/MarkDownEditor.styles.css"
  ]
}
```
25) Modify the HTML file.

Example:
```
<div ng-controller="markDownController" class="markDownController ng-scope">
    <p>My Mark Down Editor</p>
</div>
```
26) Modify the JavaScript file.

Example:
```
function markDownEditor($scope, $http, $window) {

}
```
angular.module('umbraco').controller('markDownEditor', markDownEditor);

27) Get the App_Plugins class library to copy over to the .WEB folder:
  	- Right click on the .Web project > Properties > Compile > Build Events
	- Paste in the following code in the Post-build event Command Line:
	```xcopy "C:\projects\MarkDownEditor\MarkDownEditor\App_Plugins\*" "$(ProjectDir)App_Plugins\" /Y /S /D ```
	- Click OK
	- Click Save All
28) Build > Rebuild Solution
29) Login to Umbraco backoffice to create a datatype
30) Create a new Data Type:
	- Developer > Data Types > New data type
	- Choose property Editor > <Alias> (Mark Down Editor)
31) Add new Data Type to Home page:
	- Settings > Document Types > Home
	- Add Property:
		-- Enter name and description
		-- Add Editor
		-- Choose Mark Down Editor from Available editors
	- Submit
	- Save
