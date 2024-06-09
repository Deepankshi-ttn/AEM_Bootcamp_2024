1. Create a servlet and register it on path /bin/training/configtest and extension as html, method as GET. It should print the hardcoded message "This is my first servlet."




### Steps to Set Up and Test the AEM Project

1. **Start the Author Instance**:
   ```sh
   deepankshi@TTNPL-6654:~/OSGiExercise/Author$ java -jar aem-author-p5502.jar
   ```

2. **Prepare the Author Folder**:
   - Copy `aem.jar` and `license.properties` into the author folder.

3. **Create the `crx-quickstart` Folder**:
   - The folder structure `crx-quickstart` will be created as part of the setup process.

4. **Generate the AEM Project Archetype**:
   ```sh
   deepankshi@TTNPL-6654:~/OSGiExercise/Author$ mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=43 -D appTitle="OSGI Demo" -D appId="OSGIdemo" -D groupId="com.OSGIdemo" -D aemVersion=6.5.17 -D singleCountry=n -D includeExamples=y -D includeErrorHandler=y
   ```
5. **Add Dependencies**:
   - Add the necessary dependencies in `pom.xml` and `pom.xml` in the `core` module.

6. **Open the Project in IntelliJ**:
   - Double-click on the `OSGIdemo` folder and open it with IntelliJ.
7. Code for servlet to print message "This is my first servlet"

	package com.OSGIdemo.core.servlets;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;

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

    @Override
    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        response.setContentType("text/html");
        response.getWriter().write("This is my first servlet.");
    }
}

8. **Build and Deploy the Project**:
   deepankshi@TTNPL-6654:~/OSGiExercise/Author/OSGIdemo$ mvn clean install -			 PautoInstallPackage
   

9. **Verify the Deployment**:
   - Once the build is successful, open a web browser and navigate to:
     ```
     http://localhost:5502/bin/training/configtest.html
     ```
   - Verify that the servlet it will print the message "This is my first servlet. "



