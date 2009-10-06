This .targets file and associated .dll file provide the means to minify your
C#/VB projects' embedded resources (.js and .css files).

== Applying automatic minification to your .js and .css embedded resources ==

To apply to your project, copy the JavascriptPacker.targets and JsPackMsBuildTask.dll files
to the directory with your project in it or some common directory.

add the following line just BELOW the last .targets import:

	<Import Project="$(MSBuildToolsPath)\Microsoft.CSharp.targets" /> <!-- existing line -->
	<Import Project="JavascriptPacker.targets" />  <!-- Add this line -->

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