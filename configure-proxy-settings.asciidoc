:toc: macro
toc::[]

:doctype: book
:reproducible:
:source-highlighter: rouge
:listing-caption: Listing

== Configure settings to use an HTTP Proxy for internet access

*Note* : If you use a proxy to connect to the Internet, you have to manually configure it in Maven, Sencha Cmd and Eclipse. This is explained in the following sub-sections.

=== Proxy settings for Maven

Open the file `conf/.m2/settings.xml` in an editor

image::images/download-install/devon_guide_environment_setup_3_proxy_maven.png[, width="450", link="images/download-install/devon_guide_environment_setup_3_proxy_maven.png"]

Remove the comment tags around the `<proxy>` section at the beginning of the file.

Then update the settings to match your proxy configuration.

image::images/download-install/devon_guide_environment_setup_4_proxy_maven.png[,width="450", link="images/download-install/devon_guide_environment_setup_4_proxy_maven.png"]

If your proxy does not require authentication, simply remove the `<username>` and `<password>` lines.

=== Proxy settings for Sencha Cmd

Open the file `software/Sencha/Cmd/default/sencha.cfg` in an editor

image::images/download-install/devon_guide_environment_setup_5_proxy_sencha.png[, width="450", link="images/download-install/devon_guide_environment_setup_5_proxy_sencha.png"]

Search for the property definition of `cmd.jvm.args` (around line 45).

Comment the existing property definition and uncomment the line above it.

Then update the settings to match your proxy configuration.

image::images/download-install/devon_guide_environment_setup_6_proxy_sencha.png[, width="450", link="images/download-install/devon_guide_environment_setup_6_proxy_sencha.png"]

If your proxy does not require authentication, simply remove the `-Dhttp.proxyUser`, `-DhttpProxyPassword`, `-Dhttps.proxyUser` and `-Dhttps.proxyPassword` parameters.

=== Proxy settings for Eclipse

Open eclipse by executing `eclipse-main.bat`.

image::images/download-install/devon_guide_environment_setup_7_proxy_eclipse.png[, width="350", link="images/download-install/devon_guide_environment_setup_7_proxy_eclipse.png"]

In the Eclipse preferences dialog, go to `General - Network Connection`.

image::images/download-install/devon_guide_environment_setup_8_proxy_eclipse.png[, width="450", link="images/download-install/devon_guide_environment_setup_8_proxy_eclipse.png"]

Switch from `Native` to `Manual`

Enter your proxy configuration

image::images/download-install/devon_guide_environment_setup_9_proxy_eclipse.png[, width="450", link="images/download-install/devon_guide_environment_setup_9_proxy_eclipse.png"]

That's it! You are now able to connect to the internet via the HTTP proxy you configured with all the tools.