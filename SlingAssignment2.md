-> How Sling will Call the Script on the basis of url.

Sling splits url for different necessary information from URL.Then Sling repository(JCR) looks for the request resource. Sling breaks a url in different parts like protocol, host, content path, selector(s) etc, and from the content path and extension sling look for the resource, if resource is not present at the content path sling looks for the resourceType to serve the request. 

