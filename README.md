# Build Docker image

Clone the repo, change into the `terraform-dev-docker` folder and then build the docker image.

```
C:\> git clone https://github.com/paulbouwer/terraform-dev-docker .
C:\> cd C:\GitHub\paulbouwer\terraform-dev-docker
C:\> docker build -t terraform-dev:0.5 .
```

# Use Docker image

> Note: Make sure there is at least 2GB RAM allocated to Docker (if using Docker for Windows/Mac), otherwise you will get link errors when attempting to build go code.

## Create Go workspace

Create the following Go workspace folders for the terraform project on your Windows Machine.

```
C:\projects\terraform
.vscode
bin
pkg
src
```

## Clone the terraform repo

Clone the terraform repo from GitHub into the following specific location in your Go workspace.

```
C:\> mkdir C:\projects\terraform\src\github.com\hashicorp\terraform 
C:\> cd C:\projects\terraform\src\github.com\hashicorp\terraform 
C:\projects\terraform\src\github.com\hashicorp\terraform> git clone https://github.com/hashicorp/terraform.git .
```

## Start the Docker container

Start the `terraform-dev container`, ensuring that you mount the volumes to your project folder.

```
$ docker run -it --rm \
   -v C:\projects\terraform\src\github.com\hashicorp\terraform:/go/src/github.com/hashicorp/terraform \
   -v C:\projects\terraform\pkg:/go/pkg \
   -v C:\projects\terraform\bin:/go/bin \
   terraform-dev:0.5 bash
```

This will give you a `bash` prompt in the docker container in the terraform folder from your cloned terraform repo. 

```
root@04756c97e24f:/go/src/github.com/hashicorp/terraform# 
```

Run the following to build the packages and binary for Windows. This is necessary so that the Go extension for Visual Studio Code can provide package completion and intellisense.

```
root@04756c97e24f:/go/src/github.com/hashicorp/terraform# GOOS=windows go install -v
```

# Use Visual Studio Code

> This assumes you have `Go` installed. If you do not, download it for your platform from [https://golang.org/dl/](https://golang.org/dl/). 


## Configure Visual Studio Code Workspace Settings

Copy the `settings.json` file from the `.vscode` folder into your `C:\Projects\terraform\.vscode` folder. 

The `go.gopath` value in the settings file sets up a `GOPATH` for this project. 

The `go.toolsGopath` value in the settings file sets up a location independent of `GOPATH` to install the tools required by this extension.

## Start Visual Studio Code

> If you don't have Visual Studio Code installed, then download it from [https://code.visualstudio.com/Download](https://code.visualstudio.com/Download).

Start Visual Studio Code and open the `C:\Projects\terraform\` folder. 

You can also just do the following from the `C:\Projects\terraform\` folder in your faavourite console.

```
C:\projects\terraform> code .
```

## Install Go for Visual Studio Code extension

### Install Extension

Hit `Ctl-Shift-P` in Visual Studio Code. 

Type `install extensions` and pick `Extensions: Install Extensions` from the drop down list.

Type `go` in the Extensions search box.

Click the **Install** button next to the `Go 0.6.55` extension by **lukehoban**.
s
To see more information about this extension you can visit [Visual Studio Marketplace - 
Go for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=lukehoban.Go).

### Install Tools

Hit `Ctl-Shift-P`, type `go: install` and select `Go: Install Tools` from drop down. You can follow the progress in the Output tab of the Go console in Visual Studio Code.

## Install Fira Code font in Visual Studio Code

Programmers use a lot of symbols, often encoded with several characters. For the human brain, sequences like ->, <= or := are single logical tokens, even if they take two or three characters on the screen. Your eye spends a non-zero amount of energy to scan, parse and join multiple characters into a single logical one. Ideally, all programming languages should be designed with full-fledged Unicode symbols for operators, but that’s not the case yet.

[Fira Code](https://github.com/tonsky/FiraCode) is an extension of the Fira Mono font containing a set of ligatures for common programming multi-character combinations. This is just a font rendering feature: underlying code remains ASCII-compatible. This helps to read and understand code faster.

Download and install the fonts. For Windows, install the [OTF](https://github.com/tonsky/FiraCode/tree/master/distr/otf) versions.

The [instructions](https://github.com/tonsky/FiraCode/wiki/VS-Code-Instructions) for installing Fira Code into Visual Studio Code are simple.

Hit `Ctl-Shift-P` in Visual Studio Code.

Type `settings` and pick `Preferences: Open User Settings` from the drop down list.

Add the following into the `settings.json` file, save and you can now enjoy the Fira Code font and ligatures in your code.

```
"editor.fontFamily": "Fira Code",
"editor.fontSize": 14,
"editor.fontLigatures": true
```
