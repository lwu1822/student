---
toc: true
comments: true
layout: post
title: Getting Started with Github Pages
description: Setting up Github Pages and how to give your Github Pages blog a custom domain
author: Lily Wu
---

<br>

# Setting up Github Pages

# Overview 

You will want to create a Github Pages blog for this class. This is a place where you can code, complete the hacks and submit them through a review ticket, and record what you have learned. 
<br>

# Setup

1. Fork the [student repository](https://github.com/nighthawkcoders/student) by clicking on "Fork". 
    ![]({{ site.baseurl }}/images/fork.jpg)

    This will direct you to a page where you can give a name and description for your repository. Keep "Copy the main branch only" checked and then click on "Create fork". 

2. Once your repository is created, click on the green "Code" button and copy the HTTPS link provided.

3. In WSL, go to your vscode directory (`cd ~/vscode`). Then clone the repository with `git clone <link copied from step 3>`.

4. Open your repository in VS Code with `code <repository name>`. Now you can start code, code, coding!

<br>

# Running Github Pages locally

You can run your Github Pages blog locally. This means that when you will be able to see your website update as you edit your code through a URL that starts with http://localhost. This is helpful because you can test locally to see if your code works, and push error-free code to your repository.

# Steps

You can also view the steps in the student repository's [README](https://github.com/nighthawkcoders/student/blob/main/README.md)

If using Windows, run the following commands in WSL (check out the comments if you're interested in what the commands do):

```bash
# install Ruby 
# apt install installs packages for Ubuntu
sudo apt install ruby-full build-essential zlib1g-dev
# avoid root user, set up a gem installation directory for your user account
# the following "echo" commands adds the installation directory into the .bashrc file
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
# run the .bashrc file so that WSL knows the installation directory
source ~/.bashrc
# install jekyll and bundler
gem install jekyll bundler
```


<br>

If using MacOS, run the following commands in the terminal:

```bash
# ruby
brew install chruby ruby-install xz
ruby-install ruby 3.1.3
# configure ruby into shell .zshrc or change to .bash_profile
echo "source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh" >> ~/.zshrc
echo "source $(brew --prefix)/opt/chruby/share/chruby/auto.sh" >> ~/.zshrc
echo "chruby ruby-3.1.3" >> ~/.zshrc # run 'chruby' to see actual version
#
# quit and relaunch terminal
#
# install jekyll
gem install jekyll
```


1. Open your terminal in VS Code through View -> Terminal. Alternatively, the shortcut <code>Ctrl + `</code> can be used. (You can also run the commands in WSL, but opening up VS Code's terminal in your student repository ensures you are in the correct directory.)

2. Type `bundle install`. This command installs the dependencies in your Gemfile.

3. Type `make`. This compiles the program.

4. Go to the localhost link provided in the output:
    ![]({{ site.baseurl }}/images/output.jpg)

Now, whenever the blog is edited in VS Code, the local website will show the changes upon refresh. 

Please note that these changes are only stored locally on your computer until you commit the changes. If you go to your actual blog on the web (https://<your username>.github.io/student), you will notice that the changes are not reflected there. To ensure that the actual website updates, you will need to commit your changes to your Github repository. 

<br>

# Give Github Pages a custom domain

# Overview

AWS Route 53 is a DNS service that provides routing to web applications. When you enter a URL in your web browser, the request is sent to a DNS resolver. The resolver finds a Route 53 server that can translate the URL into its corresponding IP address. The Route 53 server sends the information back to the resolver, who then sends it to the browser. The browser is able to access the webpage through the IP address it received. 


![]({{ site.baseurl }}/images/route53.jpg)

In addition, you can use Route 53 to create a domain that maps to your Github Pages blog. In the example below, the domain https://lily.nighthawkcodingsociety.com/ will point to https://lwu1822.github.io/student/.

<br>

# Steps

## AWS

1. Go to AWS Route 53, and click on "Hosted zones". 

2. Click on "Create record". 

    ![]({{ site.baseurl }}/images/createRecord.jpg)

3. Create a subdomain in "Record name". Change the "Record type" to CNAME. CNAME creates an alias that maps one domain name to another domain. Finally, in the "Value" section, enter your Github URL in the form of: `<username>.github.io`. **Be sure to omit the link to your repository**. 

    ![]({{ site.baseurl }}/images/details.jpg)

4. Click on "Create records". Your record should look something like this:

    ![]({{ site.baseurl }}/images/record.jpg)

## Github Pages

1. Create a Github pages repository and click on "Settings".

2. In the sidebar, click on "Pages". Under "Build and deployment", ensure that the Github Pages is being built from the main branch. If you forked nighthawkcoders/student, you might need to specify that before you can move on to Step 3.

    ![]({{ site.baseurl }}/images/deploy.jpg)

3. Enter the domain you configured in Hosted zones.

    ![]({{ site.baseurl }}/images/customDomain.jpg)

    Github pages will run a DNS check. Once the check is successful, access your Github pages with your new domain name (ex: lily.nighthawkcodingsociety.com). The domain should redirect to your Github Pages blog.

    **Troubleshooting**: If the check is unsuccessful, check to see that you did the "AWS" steps properly. Also check to see if you spelled the domain correctly in Github Pages. If that does not work, delete the record in AWS and create it again. (It took me multiple times to get it to work, even though I typed the correct domain name. So if you encounter this error, you may have to be patient.)

4. Click "Enforce HTTPS" so that the domain will be accessed over HTTPS.

<br>

## Afterwards

In `_config.yml`, change the line that says `baseurl` to:

```
baseurl: ""
```

This ensures that Github Pages is formatted correctly. This is because originally, the Github Pages site was under the subdirectory `student` (ex: lwu1822.github.io/**student**). After configuring a domain/subdomain,  the baseurl would be "" since the URL would look like https://lily.nighthawkcodingsociety.com/ (no subdirectory).

<br>

## Troubleshooting

### Accessing the custom domain name returns a default Nginx page

While accessing the custom domain for the first time, you may encounter the default Nginx message or the default page for whatever web server was used instead of Github Pages:

![]({{ site.baseurl }}/images/nginxDefault.png)

This issue may occur if you had accessed the custom domain in your browser before you configured it in Github Pages. 

I tried refreshing the page multiple times, but the message persisted. Even after twelve hours, the message was still there. I then opened up the "Network" tab in Chrome Dev Tools, and reloaded the page. For some reason, the Nginx message changed to my Github Pages.

I'm not sure if this solution always works, so a backup method is to open up another browser and paste the custom domain into the search bar.  


<!-- Configuring AWS Route 53 Domain to Point to Github Pages-->