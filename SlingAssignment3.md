-> Have a page /content/SlingProject/English/test.html which has resourceType    SlingProject/components/page/basepage

curl -u admin:admin -F"sling:resourceType=sling:OrderedFolder" -F"jcr:primaryType=nt:unstructured" -F"file=http://localhost:8080/content/SlingProject/English/test.html;type=text/html" http://localhost:8080/content/SlingProject/English/test.html
