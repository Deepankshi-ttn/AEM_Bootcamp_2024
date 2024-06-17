Question 1: Create a component with a dialog and having color drop down (green, blue, black etc) and one rich text editor for authoring text. This component is supposed to render text and apply chosen color to text.

Answer:

Step 1: create a node with primaryType cq:component and create a html file into that.


![img_1.png](img_1.png)

Step 2: Create a node with name cq:dialog into the component node of type nt:unstructured and add property sling:resourceType with value cq/gui/components/authoring/dialog, jcr:title with value Properties.

![img_7.png](img_7.png)

Step 3: Create a node with name content under the cq:dialog node of type nt:unstructured and add property sling:resourceType with value granite/ui/components/coral/foundation/fixedcolumns.
![img_2.png](img_2.png)
Step 4: Create a node with name items under the content node of type nt:unstructured.
![img_3.png](img_3.png)
Step 5: Create a node with name column under the items node of type nt:unstructured and add property sling:resourceType with value granite/ui/components/coral/foundation/container.

![img_4.png](img_4.png)
Step 6: Create a node with name items under the column node of type nt:unstructured.
![img_5.png](img_5.png)
Step 7: Create two nodes one for text and second for select both should be nt:unstructured and add property fieldlable with value Text/Color, name with value ./text/./color, sling:resourceType with value granite/ui/components/coral/foundation/form/textfield/granite/ui/components/coral/foundation/form/select.

![img_6.png](img_6.png)
Step 8: Create a node with name green,blue,black under the items node of type nt:unstructured and add property text with value green,blue,black.

![img_8.png](img_8.png)


