Q) Print current time.

Step 1: Create the Sling Model
1) Navigate to your core module:

2) Create a new Java class named CurrentTimeModel.java:

3) Add the following code to define the Sling Model:

package com.ttn.demo.core.models;

import org.apache.sling.api.resource.Resource;
import org.apache.sling.models.annotations.DefaultInjectionStrategy;
import org.apache.sling.models.annotations.Model;

import javax.annotation.PostConstruct;
import java.text.SimpleDateFormat;
import java.util.Date;

@Model(adaptables = Resource.class, defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
public class CurrentTimeModel {

    private String currentTime;

    @PostConstruct
    protected void init() {
        SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        currentTime = formatter.format(new Date());
    }

    public String getCurrentTime() {
        return currentTime;
    }
}
This code defines a Sling Model that adapts from a Resource and includes a PostConstruct method to initialize the current time.


Step 2: Create the HTL (Sightly) File to Display the Time
1) Create the Component Folder:
Navigate to your component directory and create a new folder for your component.


mkdir -p /apps/SlingProject/components/content/currenttime
cd /apps/SlingProject/components/content/currenttime

2) Create the HTL File:
Create a new HTL file named currenttime.html.

Add the following code:

<!-- currenttime.html -->
<div class="current-time">
    <h2>Current Time:</h2>
    <p>${currentTime.currentTime}</p>
</div>

3) Create the Component Dialog (Optional):

Step 3: Update Component Configuration
Create or update the cq:Component file.

<?xml version="1.0" encoding="UTF-8"?>
<jcr:root
    xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    xmlns:cq="http://www.day.com/jcr/cq/1.0"
    jcr:primaryType="cq:Component"
    jcr:title="Current Time"
    sling:resourceSuperType="core/wcm/components/page/v2/page"
    componentGroup="Training"/>
    
Step 4: Add the Component to a Page
1) Open a Page for Editing:
   Open any page in AEM where you want to add the current time component.

2) Add the Component:
In the AEM editor, drag and drop the "Current Time" component onto the page.

Step 5: Build and Deploy the Project

1) Build the project using Maven:

mvn clean install -PautoInstallPackage
2) Deploy the built package to your local AEM instance.

Step 6: Test the Component
1) Open the Page:

Navigate to the page where you added the current time component.

2) It will display the current time.















