Create a OSGI configuration service and include one string variable. This variable should have the value for author environment as " This servlet is executed from author environment" and for publisher the value of the variable should be " This servlet is executed from publisher environment". Use this configuration service in the above servlet and read the value from configuration service and print this value as servlet output. When we execute the servlet on author and publisher it should print output as configured.

Solution->  

Step 1: Set Up the AEM Project
1) Start the Author Instance:

deepankshi@TTNPL-6654:~/OSGiExercise/Author$ java -jar aem-author-p5502.jar

2) Prepare the Author Folder:

Copy aem.jar and license.properties into the author folder.
Start the AEM instance, which will create the crx-quickstart folder.

3) Generate the AEM Project Archetype:

deepankshi@TTNPL-6654:~/OSGiExercise/Author$ mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=43 -D appTitle="OSGI Demo" -D appId="OSGIdemo" -D groupId="com.OSGIdemo" -D aemVersion=6.5.17 -D singleCountry=n -D includeExamples=y -D includeErrorHandler=y

4) Open the Project in IntelliJ:

Double-click on the OSGIdemo folder and open it with IntelliJ.


Step 2: Create OSGi Configuration Service
1) Create Configuration Interface:

Navigate to your project's core module and create a new interface file named EnvironmentConfig.java.

Add the following code:

package com.OSGIdemo.core.services.services;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

public interface EnvironmentConfig {
    @ObjectClassDefinition(name = "Environment Configuration")
    public @interface EnvironmentConfigg {

        @AttributeDefinition(name = "Environment Message")
        String environmentMessage() default "This servlet is executed from an unknown environment";
    }
}


2) Create Configuration Service:

Create a class that implements this configuration. Create a new file named EnvironmentConfigService.java.

Add the following code:

package com.OSGIdemo.core.services.services;

import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Modified;
import org.osgi.service.metatype.annotations.Designate;

@Component(service = EnvironmentConfigService.class)
@Designate(ocd = EnvironmentConfig.class)
public class EnvironmentConfigService {
    private volatile String environmentMessage;

    @Activate
    @Modified
    protected void activate(EnvironmentConfig config) {
        this.environmentMessage = config.environmentMessage();
    }

    public String getEnvironmentMessage() {
        return environmentMessage;
    }
}

Step 3: Create the Servlet

Navigate to your servlet directory and create a new servlet file named ConfigTestServlet.java.

Add the following code:

package com.OSGIdemo.core.services.services;

import com.org.osgi.core.config.EnvironmentConfigService;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import javax.servlet.Servlet;
import java.io.IOException;

@Component(
        service = { Servlet.class },
        property = {
                "sling.servlet.paths=/bin/training/configtest",
                "sling.servlet.methods=GET",
                "sling.servlet.extensions=html"
        }
)
public class ConfigTestServlet extends SlingAllMethodsServlet {
    private static final long serialVersionUID = 1L;

    @Reference
    private EnvironmentConfigService environmentConfigService;

    @Override
    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        response.setContentType("text/html");
        String message = environmentConfigService.getEnvironmentMessage();
        response.getWriter().write(message);
    }
}


Step 4: Create OSGi Configuration Files

Navigate to your AEM project's config folder and create two configuration files:

1) For the author environment:
Create the file com.org.osgi.core.config.EnvironmentConfig~author.config:

Add the following content:
environmentMessage="This servlet is executed from author environment"


2) For the publisher environment:
Create the file com.org.osgi.core.config.EnvironmentConfig~publish.config:

Add the following content:
environmentMessage="This servlet is executed from publisher environment"

Step 5: Build and Deploy the Project

1) Build the project using Maven:
mvn clean install -PautoInstallPackage

2) Deploy the built package to your local AEM instance.

Step 6: Test the Servlet
1) Test on Author Instance:
Navigate to the URL of your servlet on the author instance:

http://localhost:5502/bin/training/configtest.html

You should see the message:
This servlet is executed from author environment

2) Test on Publisher Instance:
Navigate to the URL of your servlet on the publish instance (assuming it's running on port 4503):

http://localhost:4503/bin/training/configtest.html

You should see the message:
This servlet is executed from publisher environment






