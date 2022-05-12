#Installing the AWS CLI in macOS Catalina
![Laptop With AWS Logo](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6kuogym4i9fdtedvcbwu.png)

_This process works on macOS 10.13 or newer, including macOS 12 Monterey._

Amazon Web Services (AWS) has posted a guide on their website on how to install the AWS CLI on macOS, however, it is mostly a manual installation, and require the user to manually configure their .bash_profile, adding a path value making it for a complicated experience.
I'm here to tell you - it doesn't have to be this hard. In fact, if you have some prerequisites already installed (which you in many cases do), it's a one-line command to install it.

## Brew
If you already have the brew command line tool installed, you can skip this entire section. If you have never heard of Brew, here's the TLDR:
Homebrew or simply Brew, is a package manager for macOS, much like APT, YUM or RPM for various Linux distributions. Unfortunately, macOS doesn't come with one, so Brew has become the defacto standard for most developers and Apple Powerusers. Brew allows you to run a single command to install and manage a myriad of different tools such as the AWS CLI with a simple search instead of having to manually download, install and in many cases package and configure the application. Brew does it all for you.

You can find complete documentation for Brew on their website: brew.sh, but the way to install it is using:

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

![Installing Homebrew](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6jw51k1jluqbdvtk5gig.png)

Note that if the installation should fail with errors such as missing developer tools, you must first install the xcode tools. You can do this using:
xcode-select --install

## Installing the AWS CLI
With Homebrew installed, we can move on to installing the actual AWS CLI.

brew install awscli
Press “Y” to accept the install if required.
It's that simple. For most users, the CLI is now installed, and you can verify the install using:

```bash
aws --version
```

![Terminal With Homebrew](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4h7e8cujwyztga5h1otc.png)

## Potential Error Messages
Some users may encounter an error message after Brew has finished installing, saying:

> Warning: awscli 1.16.250 is already installed, it's just not linked.
> You can use `brew link awscli` to link this version.

To fix this, do as the error message says, and run:
brew link awscli
This should fix the issue.

## Where To Go From Here
Now that you have Brew installed, I highly recommend you check out the package [cask]. Cask is a brew extention that allow you to manage your normal graphical applications such as Firefox and many more. While this sounds stupid to beginwith, intalling applications from cask, allow you to one-line update all your applications without having to go each applications website and download the updates manually - even better? No GUI updater that you need to go through. Check it out!
Have you already been using Brew and cask and have tips? Leave them in the comments below!
