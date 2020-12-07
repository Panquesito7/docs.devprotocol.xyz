---
title: Password protect your spacebook 🔒 

date: 2020-11-20
permalink: /tips-and-tricks/encryption/index.html
eleventyNavigation:
  key: Password protection
  order: 65 
---
By default, your spacebook is setup to be [non-discoverable](/privacy) but if you wish to protect your entire site behind a password you can easily enable encryption for your spacebook powered by [Staticrypt](https://github.com/robinmoisson/staticrypt). (Netlify does not allow for basic authentication on their free tier -- if you are willing to pay $20 a month for basic auth you do not need to use this feature.)

## Step 1: Set a password in Netlify

* Visit your Netlify dashboard and go to **Site settings > Build & deploy > Environment**

* Click **Edit variables** and a new variable with the key PASSWORD. The "value" you set will be the password people use to unlock the site:

![Password example](/static/img/password.png)

If you ever change this password, all of your users will be prevented from accessing your content until they have the new password.

## Step 2: Enable encryption

In your **_data/site.json** file, set encryption to true:

```
{
  ...
  "enableEncryption": true 👈 set this to true!
  ...
}

```

This will enable a logout button that appears in your header whenever encryption is enabled. 

## Step 3: Change your build command

Now you need to change the build command in your **netlify.toml** file. Comment out the current **npm run build** command and enable the command directly below it like so:  

```
[build]
  publish = "_site"
  #command = "npm run build" 
  command = "npm run build && set -e && find ./_site -type f -name '*.html' -exec staticrypt -f password_template.html {} $PASSWORD -o {} \\;"
  
```

That's it! 👍

When you deploy your site all of your pages will be encrypted at build time and require a password to access. The passphrase is stored in local storage and users remain logged in for future visits until they click the logout button or an administrator changes the password. 

::: callout
**Did you know?** Because all of your pages are being decrypted on the fly at the time of access, there is a small "flash of unstyled HTML" that occurs between each page load. *I will happily accept a pull request to smooth this out, perhaps with a loading spinner or some other mechanism that looks a bit better* 😊
:::