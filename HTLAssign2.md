Q) Create a list of resources under /content/my-options and add two properties "text"
and "value" on each resource. Create a dropdown (HTMl select) using the resources
list as options.

Step 1: Create Resources Under /content/my-options

First, let's create a list of resources under /content/my-options and add the required properties.

1) Create Resources in CRXDE Lite:

- Open AEM CRXDE Lite: http://localhost:4502/crx/de/index.jsp
- Navigate to /content and create a new node named my-options of type nt:unstructured.
- Under /content/my-options, create several child nodes, each representing an option, and add properties to them.
/content/my-options
    ├── option1
    │   ├── text (String) : "Option 1"
    │   └── value (String) : "value1"
    ├── option2
    │   ├── text (String) : "Option 2"
    │   └── value (String) : "value2"
    └── option3
        ├── text (String) : "Option 3"
        └── value (String) : "value3"


2) Adding Properties to Each Resource:

- For each option node (e.g., option1), add two properties: text and value.

Step 2: Create Sling Model to Fetch the Options

Next, create a Sling Model to fetch the list of options from /content/my-options.

1) Create the Sling Model:

Navigate to your core module and create a new Java class named OptionsModel.java.

--->  Add the following code:

package com.ttn.demo.core.models;

import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.models.annotations.DefaultInjectionStrategy;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.Self;

import javax.annotation.PostConstruct;
import java.util.ArrayList;
import java.util.List;

@Model(adaptables = Resource.class, defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
public class OptionsModel {


    private List<Option> options;


    @Self
    private Resource resource;


    @PostConstruct
    protected void init() {
        options = new ArrayList<>();
        ResourceResolver resolver = resource.getResourceResolver();
        Resource optionsResource = resolver.getResource("/content/my-options");


        if (optionsResource != null) {
            for (Resource optionResource : optionsResource.getChildren()) {
                String text = optionResource.getValueMap().get("text", String.class);
                String value = optionResource.getValueMap().get("value", String.class);
                options.add(new Option(text, value));
            }
        }
    }


    public List<Option> getOptions() {
        return options;
    }


    public static class Option {
        private String text;
        private String value;


        public Option(String text, String value) {
            this.text = text;
            this.value = value;
        }


        public String getText() {
            return text;
        }


        public String getValue() {
            return value;
        }
    }
}


Step 3: Create the HTL (Sightly) File to Display the Dropdown

1) Create the Component Folder:

   Navigate to your component directory and create a new folder for your component.
   
   mkdir -p /apps/SlingProject/components/content/optionsdropdown
   
   cd /apps/SlingProject/components/content/optionsdropdown
   
2) Create the HTL File:

            Create a new HTL file named optionsdropdown.html.
            
---->	Add the following code:

	<!-- optionsdropdown.html -->
<div class="options-dropdown">
    <select>
        <option value="">Select an option</option>
        <data-sly-use.optionsModel="com.ttn.demo.core.models.OptionsModel">
            <data-sly-list.option="${optionsModel.options}">
                <option value="${option.value}">${option.text}</option>
            </data-sly-list>
        </data-sly-use.optionsModel>
    </select>
</div>

Step 4: Create Component Configuration

Ensure your component is properly defined. If not already created, define it in the component configuration file.

1) Create or update the .content.xml file:

Add the following content:

<?xml version="1.0" encoding="UTF-8"?>
<jcr:root
    xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    xmlns:cq="http://www.day.com/jcr/cq/1.0"
    jcr:primaryType="cq:Component"
    jcr:title="Options Dropdown"
    sling:resourceSuperType="core/wcm/components/page/v2/page"
    componentGroup="Training"/>
    
Step 5: Add the Component to a Page

1) Open a Page for Editing:
   Open any page in AEM where you want to add the options dropdown component.
   
2) Add the Component:
   In the AEM editor, drag and drop the "Options Dropdown" component onto the page.
   
Step 6: Build and Deploy the Project
1) Bu1ild the project using Maven:
mvn clean install -PautoInstallPackage
2) Deploy the built package to your local AEM instance.

Step 7: Test the Component

1) Open the Page:
   Navigate to the page where you added the options dropdown component.
   
2) Verify the Output:
   The dropdown is populated with options from /content/my-options.
































