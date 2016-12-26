---
id: 64
title: Integrating Oracle Access Manager 11.1.2.2.0 with Oracle Mobile Authenticator
date: 2014-06-27T18:35:26+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=64
permalink: /2014/06/27/integrating-oracle-access-manager-11-1-2-2-0-with-oracle-mobile-authenticator/
categories:
  - 11.1.2.2.x
  - Mobile
  - Oracle
  - Oracle Access Manager
---
The following assumes a vanilla install of Oracle Access Manager 11.1.2.2.0, with all the default port values. No further customization has been made. The Admin Server is to be the only service running during the configuration, otherwise the changes being made will occur in real time and you will be locked out of the OAM Console.
<!-- more --> 

<h1>Configure Oracle Access Manager</h1>
<h2>Enable the Mobile and Social Service</h2>
<ol>
	<li>Log into OAM Admin Console.
Open a web broswer to <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">http://&lt;servername&gt;:7001/oamconsole/</span> and when presented with the Forms Credential Collector enter the credentials for the weblogic user.</li>
	<li>From the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Launch Pad</span> tab select <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Available Services</span> under the configuration panel.</li>
	<li>Enable the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Mobile and Social Service</span> by clicking the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Enable</span> button next door to the Mobile and Social entry.</li>
	<li>Once enabled, close the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Available Services</span> tab to return to the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Launch Pad</span>.</li>
</ol>
<h2>Configure OAuth for Oracle Mobile Authenticator</h2>
<ol>
	<li>From the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Launch Pad</span> select the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">OAuth Service</span> link within the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Mobile and Social</span> panel.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">OAuth Identity Domains</span> tab should now be visible.</li>
	<li>Select the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Default Domain</span> link within the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">OAuth Identity Domains</span> table.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Default Domain</span> tab should now be visible.</li>
	<li>Select the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Resource Servers</span> tab.</li>
	<li>Within the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">User Profile Services</span> panel select the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">UserProfile</span> link.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">UserProfile</span> tab should now be visible.</li>
	<li>Expand the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Resources URIs</span> panel at the bottom of the page.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">/secretkey tab</span> should know be visible in the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Resource URIs</span> panel.</li>
	<li>Select the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">/secretkey</span> tab.</li>
	<li>Expand the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Attributes</span> panel within the /secretkey tab</li>
	<li>Update the following attributes to the following
<table>
<tbody>
<tr>
<td><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">basicauth.allowed</span></td>
<td><strong><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">true</span></strong></td>
</tr>
<tr>
<td><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">keyAttributeName</span></td>
<td><strong><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">mobile</span></strong></td>
</tr>
</tbody>
</table>
</li>
	<li>Click the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Apply</span> button to save the changes.</li>
	<li>Once successful you should have a Confirmation saying that the changes were saved successfully.</li>
	<li>Close the following tabs
<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">UserProfile</span>.
<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;"> DefaultDomain</span>.
<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;"> OAuth Identity Domains.</span></li>
</ol>
<h2>Create Instance of TOTPModule</h2>
<ol>
	<li>From the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Launch Pad</span> select the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Authentication Modules</span> link within the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Access Manager</span> panel.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Authentication Modules</span> tab should now be visible.</li>
	<li>In the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Search</span> panel, enter <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">TOTPModule</span> in the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Name</span> field and click the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Search</span> Button.</li>
	<li>In the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Search Results</span> panel click on <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">TOTPModule</span> link.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">TOTPModule</span> tab should now be visible.</li>
	<li>In the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Authentication Module</span> panel click the Steps tab.</li>
	<li>Select the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">OTPAuthentication</span> Step and update the following values in the Step Details panel.
<table>
<tbody>
<tr>
<td><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">KEY_OTP_SECRETKEY_ATTRIBUTE</span></td>
<td><strong><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">mobile</span></strong></td>
</tr>
<tr>
<td><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">KEY_IDENTITY_STORE_REF</span></td>
<td><strong><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">UserIdentityStore1</span></strong></td>
</tr>
</tbody>
</table>
</li>
	<li>Click the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Save</span> button in the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Step Details</span> panel.</li>
	<li>Click the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Apply</span> button in the Authentication Model panel.</li>
	<li>Once successful you should have a Confirmation saying that the changes were saved successfully.</li>
	<li>Close the following tabs.
<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">TOTPModule</span>.
<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;"> Authentication Modules</span>.</li>
</ol>
<h2>Configure TOTP Plugin parameters</h2>
<ol>
	<li>From the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Launch Pad</span> select the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Plug-ins</span> link within the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Access Manager</span> panel.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Plug-ins</span> tab should now be visible.</li>
	<li>Select the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">TOTPPlugin</span> link within the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Plug-ins</span> table and update the following values in the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Plug-in Details: TOTPPlugIn</span> panel.
<table>
<tbody>
<tr>
<td><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">KEY_OTP_SECRETKEY_ATTRIBUTE</span></td>
<td><strong><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">mobile</span></strong></td>
</tr>
<tr>
<td><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">KEY_IDENTITY_STORE_REF</span></td>
<td><strong><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">UserIdentityStore1</span></strong></td>
</tr>
</tbody>
</table>
</li>
	<li>Click the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Save</span> button within the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Plug-in Details: TOTPPlugIn</span> panel.</li>
	<li>There is no notification saying if the change was successful, so try clicking on another plugin and then back to the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">TOTPlugin</span> to see if the details have been saved.</li>
	<li>Close the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Plug-ins</span> tab.</li>
</ol>
<h2>Create OTP Authentication Scheme</h2>
<ol>
	<li>From the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Launch Pad</span> select the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Authentication Schemes</span> link within the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Access Manager</span> panel.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Authentication Schemes</span> tab should now be visible.</li>
	<li>In the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Search</span> panel, enter <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">LDAPScheme</span> in the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Name</span> field and click the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Search</span> Button.</li>
	<li>In the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Search Results</span> panel click on <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">LDAPScheme</span> link.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">LDAPScheme</span> tab should now be visible.</li>
	<li>Click the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Duplicate</span> button within the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Authentication Scheme</span> panel.</li>
	<li>Another tab titled <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">LDAPScheme</span> should now be visible.</li>
	<li>In the second <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">LDAPScheme </span> tab update the following details
<table>
<thead></thead>
<tbody>
<tr>
<td><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Name</span></td>
<td><strong><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">TOTPScheme</span></strong></td>
</tr>
<tr>
<td><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Authentication Module</span></td>
<td><strong><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">TOTPModule</span></strong></td>
</tr>
<tr>
<td><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Challenge URL</span></td>
<td><strong><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">/pages/getOTP.jsp</span></strong></td>
</tr>
</tbody>
</table>
</li>
	<li>In the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Authentication Scheme</span> panel click the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Apply</span> button.</li>
	<li>Once successful you should have a Confirmation saying that the changes were saved successfully.</li>
	<li>Close the following tabs.
<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">TOTPScheme</span>.
<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">LDAPScheme</span>.
<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Authentication Scheme</span>.</li>
</ol>
<h2>Update IAMSuite Application Domain</h2>
<ol>
	<li>From the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Launch Pad</span> select the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Application Domains</span> link within the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Access Manager</span> panel.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Application Domain</span>s tab should now be visible.</li>
	<li>In the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Search</span> panel, enter<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;"> IAM Suite</span> in the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Name</span> field and click the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Search</span> Button.</li>
	<li>In the<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;"> Search Results</span> panel click on IAM Suite link.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;"> IAM Suite <span style="color: #000000;">tab</span></span> should now be visible.</li>
	<li>In the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Application Domains</span> panel click the Authentication Policies tab.</li>
	<li>In the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Authentication Policies</span> table click on the<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;"> OAM Admin Console Policy</span> link</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">IAM Suite: OAM Admin Con...</span> tab should now be visible.</li>
	<li>Select the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Advanced Rules</span> tab within the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Authentication Policy</span> panel.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Advanced Rules</span> tab should now be visible within the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Authentication Policy</span> panel.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Post-Authentication</span> tab should now be visible within the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Advance Rules</span> tab.</li>
	<li>Select the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Post-Authentication</span> tab.</li>
	<li>In the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Post-Authentication</span> panel click the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">+</span> button.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Add Rule</span> model should now be visible and you can provide the following values
<table>
<thead></thead>
<tbody>
<tr>
<td><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Rule Name</span></td>
<td><strong><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">OTP</span></strong></td>
</tr>
<tr>
<td><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Condition</span></td>
<td><strong><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">'true'=='true'</span></strong></td>
</tr>
<tr>
<td><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">If condition is true</span></td>
<td><strong><span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">TOTPScheme</span></strong></td>
</tr>
</tbody>
</table>
</li>
	<li>Click the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Add</span> button.</li>
	<li>The <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Rule Name</span> should now appear in the table.</li>
	<li>Click the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Apply</span> button within the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Authentication Policy</span> panel.</li>
	<li>Once successful you should have a Confirmation saying that the changes were saved successfully.</li>
	<li>Close the following tabs
<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">IAM Suite: OAM Admin Con...</span>.
<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">IAM Suite</span>.
<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">Application Domain<span style="color: #000000;">.</span></span></li>
</ol>
<h2>Start Oracle Access Manager Managed Instance</h2>
<ol>
	<li>Log into WebLogicAdmin Console.
Open a web broswer to <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">http://&lt;servername&gt;:7001/console/</span> and when presented with the Forms Credential Collector enter the credentials for the weblogic user.</li>
	<li>In the <span style="font-family: courier new,courier; font-size: 8pt; color: #3366ff;">Home Page</span> panel under <span style="font-family: courier new,courier; color: #3366ff;">Domain Configurations</span>, select the <span style="font-family: courier new,courier; font-size: 8pt; color: #3366ff;">Servers </span>link.</li>
	<li>The <span style="font-family: courier new,courier; font-size: 8pt; color: #3366ff;">Summary of Servers</span> panel should now be visible.</li>
	<li>Select the <span style="color: #3366ff; font-size: 8pt; font-family: courier new,courier;">Control</span> tab to bring up the list of servers available.</li>
	<li>Select the <span style="color: #3366ff; font-size: 8pt; font-family: courier new,courier;">oam_server1</span> checkbox and click the <span style="color: #3366ff; font-size: 8pt; font-family: courier new,courier;">Start</span> button.</li>
	<li>Wait for the <span style="color: #3366ff; font-size: 8pt; font-family: courier new,courier;">oam_server1</span> instance to start.</li>
</ol>
&nbsp;
<h1>Configure Oracle Mobile Authenticator configuration page</h1>
In order for a user to have a One Time Pin generated their device needs to be registered with Oracle Access Manager via the Mobile and Social Oauth service. To do this a special link needs to be added to a web page that configures the Oracle Mobile Authenticator to add user accounts. Once the user has added the OMA client to their device (through one of the following links <a href="https://play.google.com/store/apps/details?id=oracle.idm.mobile.authenticator" target="_blank">Android </a>or <a href="https://itunes.apple.com/us/app/oracle-mobile-authenticator/id835904829?ls=1&amp;mt=8" target="_blank">IOS</a>) they can click the special link to configure the device with their account. The format of the link is

<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">oraclemobileauthenticator://settings?LoginURL::=http://&lt;servername&gt;:14100/ms_oauth/resources/userprofile/secretkey</span>
<h2> Create HTML Page with link</h2>
<ol>
	<li>Create a HTML file called <span style="font-family: courier new,courier; font-size: 8pt; color: #3366ff;">oma.html</span></li>
	<li>Set it contents to

[code lang="html" title="oma.html"]&lt;/pre&gt;
&lt;html&gt;
    &lt;head&gt;
        &lt;title&gt;OMA Configuration&lt;/titile&gt;
    &lt;/head&gt;
    &lt;body&gt;
        &lt;a href=&quot;oraclemobileauthenticator://settings?LoginURL::=http://&lt;servername&gt;:14100/ms_oauth/resources/userprofile/secretkey&quot;&gt;Open Me&lt;/a&gt;
    &lt;/body&gt;
&lt;/html&gt;
[/code]

</li>
	<li>Upload the file to a webserver.</li>
</ol>
<h2>Configure Oracle Mobile Authenticator</h2>
<ol>
	<li>Install the Oracle Mobile Authenticator application via <a href="https://play.google.com/store/apps/details?id=oracle.idm.mobile.authenticator" target="_blank">Android </a>or <a href="https://itunes.apple.com/us/app/oracle-mobile-authenticator/id835904829?ls=1&amp;mt=8" target="_blank">IOS</a></li>
	<li>Open a Web Browser on the device and enter the URL where <span style="font-family: courier new,courier; font-size: 8pt; color: #3366ff;">oma.html</span> was uploaded.
<span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">http://&lt;servername&gt;:&lt;port&gt;/</span><span style="font-family: courier new,courier; font-size: 8pt; color: #3366ff;">oma.html</span></li>
	<li>The Oracle Mobile Authenticator will open informing that <span style="font-size: 8pt; font-family: courier new,courier; color: #3366ff;">App configuration is about to be updated</span>.</li>
	<li>The user will click the <span style="font-family: courier new,courier; font-size: 8pt; color: #3366ff;">Accept </span>button to continue</li>
	<li>Next the user will be asked to provide their credentials to log into the Oracle Access Manager.</li>
	<li>Once succesfull the account will be shown with the current One Time Pin.</li>
</ol>
<h1>Access Oracle Access Manager Admin Console</h1>
Now that the One Time Pin configuration has been complete, it is possible to test the service by connecting to the Oracle Access Manager Admin Console. By doing this the user should first be prompted for Credentials via Forms, and then upon successful credentials validation be prompted to supply a One Time Pin which is generated on the Oracle Mobile Authenticator application.
<h2>Connect to the OAM Admin Console</h2>
<ol>
	<li>Log into OAM Admin Console.
Open a web broswer to <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">http://&lt;servername&gt;:7001/oamconsole/</span> and when presented with the Forms Credential Collector enter the credentials for the weblogic user.</li>
	<li>The next screen should contain a single field <span style="font-family: courier new,courier; font-size: 8pt; color: #3366ff;">One Time Pin</span>:</li>
	<li>Open the Oracle Mobile Authenticator application and the One Time Pin for the weblogic user should be shown, with an indicator for when it is going to change. Try and select one that is not going to be expiring shortly and enter this value in the <span style="font-family: courier new,courier; font-size: 8pt; color: #3366ff;">One Time Pin</span>: field and press the <span style="color: #3366ff; font-size: 8pt; font-family: courier new,courier;">Login </span>button.</li>
	<li>If successful the Oracle Access Manager Admin Console should be presented.</li>
</ol>
<h1>Conclusion</h1>
I have tried my best to simplify the process and clean up some of the documentation mistakes that are present. For example it took me a while to figure out that I needed to update the <span style="color: #3366ff; font-family: courier new,courier,monospace; font-size: 8pt;">keyAttributeName</span> field to reflect my attribute in LDAP, as other wise I was getting an <span style="font-size: 8pt; font-family: courier new,courier; color: #3366ff;">secret key is already present</span> error message when trying to register an account with the Oracle Mobile Authenticator.

I also have not covered how to enable the <span style="color: #3366ff; font-size: 8pt; font-family: courier new,courier;">TOTPPlugin</span> with the Google Authenticator and this will be for another article. Next steps is to automate this via WLST and update with images to help guide the reader.

Any questions feel free to leave a comment and I will try my best to answer them as quickly as possible.

&nbsp;

&nbsp;

&nbsp;

&nbsp;