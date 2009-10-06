This .targets file and associated .dll file provide the means to minify your
C#/VB projects' embedded resources (.js and .css files).

RELEASE NOTES: No .css minification is in place in this .targets file YET.
               But there will be soon!

== Applying automatic minification to your .js and .css embedded resources ==

To apply to your project, copy the JsCssMinification.targets and MinifierMsBuildTask.dll files
to the directory with your project in it or some common directory.

add the following line just BELOW the last .targets import:

	<Import Project="$(MSBuildToolsPath)\Microsoft.CSharp.targets" /> <!-- existing line -->
	<Import Project="JsCssMinification.targets" />  <!-- Add this line -->

That's all you need to do.  All EmbeddedResource .js items in your project
will automatically be minified in any build where $(Configuration) == 'Release'
or $(JsPack) == 'true'.  It can be suppressed by setting $(JsPack) == 'false'.

== PRESERVING COPYRIGHT NOTICES IN JAVASCRIPT FILES ==
Minification removes all comments in .js files.  To keep a copyright notice at
the top of your .js file, you must add metadata to your EmbeddedResource item,
as shown here:

	<PropertyGroup>
		<StandardCopyright>
Copyright (c) 2009, Andrew Arnott. All rights reserved.
Code licensed under the Ms-PL License:
http://opensource.org/licenses/ms-pl.html
</StandardCopyright>
	</PropertyGroup>

	<ItemGroup>
		<EmbeddedResource Include="OpenId\RelyingParty\OpenIdRelyingPartyAjaxControlBase.js">
			<Copyright>$(StandardCopyright)</Copyright>
		</EmbeddedResource>
	</ItemGroup>

== Javascript errors from minification ==
The Dean Edwards .js minifier is very aggressive (and thus very effective at
shrinking files).  As a result, all semicolons are strictly required to be
after every statement.  Missing semicolons will result in javascript errors
at runtime from browsers.  If you find yourself getting these errors, 
FireFox does a better job than IE at reporting exactly where the missing
semi-colon is.
You can read more about Javascript minification including less aggressive
algorithms here: http://developer.yahoo.com/yui/compressor/