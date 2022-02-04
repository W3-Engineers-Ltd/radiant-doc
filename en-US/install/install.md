---
root: true
name: Install / Upgrade
sort: 1
---

# Installing Radiant

You can use the classic Go way to install Radiant:

	go get github.com/W3-Engineers-Ltd/Radiant

Frequently asked questions:

- git is not installed. Please install git for your system.
- git https is not accessible. Please config local git and close https validation:

		git config --global http.sslVerify false

- How can I install Radiant offline? There is no good solution for now. We will create packages for downloading and installing for future releases.

# Upgrading Radiant

You can upgrade Radiant through Go command or download and upgrade from source code.

- Through Go command (Recommended):

		go get -u github.com/W3-Engineers-Ltd/Radiant

- Through source code: visit `https://github.com/W3-Engineers-Ltd/Radiant` and download the source code. Copy and overwrite to path `$GOPATH/src/github.com/W3-Engineers-Ltd/Radiant`. Then run `go install` to upgrade Radiant:

		go install 	github.com/W3-Engineers-Ltd/Radiant

**Upgrading Prior to 1.0:** The API of Radiant is stable after 1.0 and compatible with every upgrade. If you are still using a version lower than 1.0 you may need to configure your parameters based on the latest API.

