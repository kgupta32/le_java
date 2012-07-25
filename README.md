Logging To Logentries from Java
==============================

Logentries currently supports log4j logging from Java as well as PaaS, such as CloudBees

--------------------------------------------------------------

Simple Usage Example
--------------------

	public class HelloLoggingWorld {
	
		private static Logger log = LogManager.getRootLogger();
		
		public static void main(String[] args) throws IOException
		{
			log.warn("Warning log sent from java class");
		}
	}


--------------------------------------------------------------

To configure log4j, you will need to perform the following:

    * (1) Obtain your Logentries account key.
    * (2) Setup Log4j (if you are not already using it).
    * (3) Configure the Logentries Log4j plugin.

You can obtain your Logentries account key on the Logentries UI, by clicking account in the top left cornercand then display account key on the right.


log4j Setup
-----------

If you don't already have log4j set up in your project, please follow these steps:

Download log4j from:

https://logging.apache.org/log4j/1.2/download.html

Retrieve log4j jar file and place it the `WEB-INF/lib` folder of your project.

Then add it to the build path from within your project.

Logentries log4j Plugin Setup
-----------------------------

The next file you need is logentries-1.1.4.jar which is the plugin for log4j. It is available from github at:

https://github.com/logentries/le_java/downloads

Place this in the `WEB-INF/lib` folder of your project and add it to the buildpath as done above with log4j jar.

The second file required is called log4j.xml and is available again on github on projects pages.

Add this file to your project as it is the config which adds the plugin for log4j to send logs to Logentries. This file should be in `WEB-INF/classes`

In this file, you will see the following:

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE log4j:configuration>
	<log4j:configuration debug="true">
	<appender name="le" class="com.logentries.log4j.LeAppender">
		<!-- Enter your Logentries account key, like bc0c4f90-a2d6-11e1-b3dd-0800200c9a66 -->
		<param name="Key" value="LOGENTRIES_ACCOUNT_KEY" />
		<!-- Enter log location, like servername/logname -->
		<param name="Location" value="LOGENTRIES_LOCATION" />
		<param name="Debug" value="false" />
		<param name="SSL" value="false" />
		<layout class="org.apache.log4j.PatternLayout">
			<param name="ConversionPattern"
				value="%d{yyyy-MM-dd HH:mm:ss ZZZ} %-5p (%F:%L)  %m" />
		</layout>
	</appender>
	<logger name="example">
		<level value="debug" />
	</logger>
	<root>
		<priority value="debug"></priority>
		<appender-ref ref="le" />
	</root>
	</log4j:configuration>

Replace the value "LOGENTRIES_ACCOUNT_KEY" with your account-key obtained earlier. Also replace the "LOGENTRIES_LOCATION" value. This should be in the following format:

    `hostname/logname.log`
    
For debugging purposes set the debug parameter to true. The appender will display debug information on console. You can also activate SSL encryption when used in public networks. Note that SSL encryption may be expensive in terms of CPU usage.


Logging Messages
----------------

With that done, you are ready to send logs to Logentries.

In each class you wish to log from, enter the following using directives at the top if not already there:

	import org.apache.log4j.Logger;
	import org.apache.log4j.LogManager;

Then create this object at class-level:

	private static Logger log = LogManager.getRootLogger();

Now within your code in that class, you can log using log4j as normal and it will log to Logentries.

Example:

	log.debug("Debugging Message");
	log.info("Informational message");
	log.warn("Warning Message");

