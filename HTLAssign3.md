Q) Read the color property from the current node and set it as the backgorund of the page.

Step 1: Create the Sling Model
1) Navigate to your core module:

2) Create a new Java class named PageBackgroundModel.java:

3) Add the following code to define the Sling Model:

package com.ttn.demo.core.models;

import org.apache.sling.api.resource.Resource;
import org.apache.sling.models.annotations.DefaultInjectionStrategy;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.Self;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

@Model(adaptables = Resource.class, defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
public class PageBackgroundModel {

    @ValueMapValue
    private String color;

    @Self
    private Resource resource;

    public String getColor() {
        return color != null ? color : "#FFFFFF"; // Default color if not set
    }
}
Step 2: Create the HTL (Sightly) File to Apply the Background Color

1) Create the Component Folder:

Navigate to your component directory and create a new folder for your component.

mkdir -p /apps/SlingProject/components/content/pagebackground
cd /apps/SlingProject/components/content/pagebackground

2)  Create the HTL File:
	Create a new HTL file named pagebackground.html.
	
	Add the following code:
	
	<!-- pagebackground.html -->
<div class="page-background" data-sly-use.pageBackground="com.ttn.demo.core.models.PageBackgroundModel" style="background-color: ${pageBackground.color};">
    <h2>Page Background Color</h2>
    <p>The background color is set based on the current node's color property.</p>
</div>

Step 3: Create Component Configuration

Ensure your component is properly defined. If not already created, define it in the component configuration file.

1) Create or update the .content.xml file:

Add the following content:

<?xml version="1.0" encoding="UTF-8"?>
<jcr:root
    xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    xmlns:cq="http://www.day.com/jcr/cq/1.0"
    jcr:primaryType="cq:Component"
    jcr:title="Page Background"
    sling:resourceSuperType="core/wcm/components/page/v2/page"
    componentGroup="Training"/>
    
Step 4: Add the Color Property to a Node

1) Open CRXDE Lite: http://localhost:4502/crx/de/index.jsp

2) Navigate to the Page Node:
- Go to the page where you want to apply the background color.
- Add a property named color with a string value representing the color (e.g., #FF5733).

Step 5: Add the Component to a Page

1) Open a Page for Editing:

- Open any page in AEM where you want to add the page background component.

2) Add the Component:

- In the AEM editor, drag and drop the "Page Background" component onto the page.

Step 6: Build and Deploy the Project

1) Build the project using Maven:

mvn clean install -PautoInstallPackage

2) Deploy the built package to your local AEM instance.

Step 7: Test the Component

1) Open the Page:
Navigate to the page where you added the page background component.
2) Verify the Output:
The background color of the page is set based on the color property of the current node.





















