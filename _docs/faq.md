---
layout: gradle
title: FAQ
permalink: /docs/faq/
---


### How can I use my credentials?

To use several credentials, you should use ‘a credentialId’ parameter for any task for more information see <a href="{{ site.url }}/docs/auth/" target="_blank">authentications</a>

    $ gradle <TASK NAME> -PcredentialId=<CREDENTIAL ID>

With the last command line you are able to deploy your code to different accounts if you want.

    $ gradle <TASK NAME ONE> -PcredentialId=<CREDENTIAL ONE>
    $ gradle <TASK NAME TWO> -PcredentialId=<CREDENTIAL TWO>

To more information  about credentials management enter <a href="{{ site.url }}/docs/credentials/" target="_blank">here</a>

### How can I upload my local code to my organization without a class called 'Class1.cls'?

To upload your code you should execute Upload task using excludes parameter as:

    $ gradle upload -Pexcludes=**/Class1.cls -PcredentialId=myId

This command line is using a wildcard. Also, it supports folders name and files name. This parameter is available to all task but Retrieve task. Other use are listed below:

To exclude classes and triggers:

    $ gradle upload -Pexcludes=classes,triggers -PcredentialId=myId

To exclude Account.object file:

    $ gradle upload -Pexcludes=objects/Account.object -PcredentialId=myId

Also you should use Deploy task for more information see <a href="{{ site.url }}/docs/deployment/" target="_blank">deployment tasks</a>

### Which are difference between upload task and deploy task?


The upload task, upload code from your local repository without truncating code directly as it is. This task is usually used for ‘package organization.’ Also, it can be used in several organizations.

While that Deploy task to upload code as a first step truncates your code and uploads that code. As a second step, it uploads  your code to your organization, usually this task is used for development organizations.

For more information visit <a href="{{ site.url }}/docs/deployment/" target="_blank">deployment tasks</a>

### How can I create my custom upload task using interceptors?

For example, to deploy for development organization, we need to remove ``` @deprecated ``` annotation for each class.

First step: Import the upload class in your build.gradle  file.
``` 
import org.fundacionjala.gradle.plugins.enforce.tasks.salesforce.deployment.Upload
```

Second step: Create a closure with a file parameter. In this case, it represents each class file in your code.
{% highlight groovy linenos=table%}
def annotation = "@deprecated"

def removeDeprecated = { classFile->
 classFile.text = classFile.text.replaceAll(annotation, '')
 }
{% endhighlight %}

Third step create a new task and add the closure created.

{% highlight groovy linenos=table%}
task UploadToRemoveDeprecated(type: Upload){
 interceptor('classes','removeDeprecated', removeDeprecated)
 interceptors = ['removeDeprecated']    
}
{% endhighlight %}
For more information visit <a href="{{ site.url }}/docs/undeploy/#undeploy-task-using-interceptors" target="_blank">undeploy task</a>

>***Note:*** This interceptor already exists by default for deployment tasks.

### What are tasks supporting interceptors?

The tasks that support interceptors are undeploy, deploy, upload and update.