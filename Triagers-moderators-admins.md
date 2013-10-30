## Bug Triagers
In most of the [[Free Open Source Software (FOSS) | http://en.wikipedia.org/wiki/Free_and_open-source_software]], one individual or a group of developers are responsible for bug triage, i.e. checking whether newly reported issues to the issue tracking system are indeed valid, whether they are duplicate of a previously reported issue, how severe they are and how much priority they have for the project, and which developer might have more expertise and interest in resolving the issue.

In this project, currently, DFKI and FBK have got a team of 2 persons responsible for bug triage. They have an internal list of developers with their field of expertise and interest. They try to assign incoming issues which are not yet assigned to any developer, to the proper ones. Of course, developers may make themselves volunteer or directly assign an issue to themselves. Moreover, they could toss issues among each other, i.e. agree to reassign a previously assigned issue to developer A to developer B, for some reasons. However, the purpose of the bug triage mechanism is to make sure that no important issue will remain without taking care of it and unresolved.

## Mailing List Moderators
Mailing list moderators are responsible for accepting new subscription requests, controlling the content and detecting Spams, banning abusers, etc. Currently, the mailing list moderators are the bug triagers.

## Website Admins
Currently, the website admins are the [[release managers | https://github.com/hltfbk/Excitement-Open-Platform/wiki/How-do-we-make-a-new-release%3F]], who are responsible for generating a new revision of the [[EOP website | http://hltfbk.github.io/Excitement-Open-Platform/]] using Maven for each new [[EOP]] release.

## Managing the Other Technical Infrastructure

FBK is currently responsible for managing the Git repositories on Github, the Maven internal repository (i.e. artifactory), Jenkins, and also uploading the artifacts of each major distribution release to the Maven Central repository. 

The person in charge of it will answer the questions like how "I'm not able to clone the repository", "I'm not able to do a pull request", "I can't see the EOP github repository", "How can I add a new resource into Artifactory", "Artifactory seems down and I'm not able to build the EOP project", "maven provides this error when I try to build the project".  "I would change the pom file as ...".  "According to Jenkins my build is unsuccessful and I don't know the reason".