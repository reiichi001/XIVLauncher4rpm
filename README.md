# XIVLauncher4rpm

RPMs and build files for native versions of <a href=https://github.com/goatcorp/FFXIVQuickLauncher>FFXIVQuickLauncher</a>. Currently only tested on
Fedora 35, 36, and openSUSE LEAP 15.4. It'll probably work for other distros as well, if you can work out the dependencies. Try at your own risk
(minimal as it probably is).

**Warning! This version is not officially supported. It works for me, but ymmv**

If you don't know what you're doing, I'd suggest following these instructions: <a href=https://goatcorp.github.io/faq/steamdeck>XIVLauncher Steam Deck
Installation Guide</a>. It says Steam Deck, but it should work for most Linux distibutions. Or get it directly from
<a href=https://flathub.org/apps/details/dev.goats.xivlauncher>Flathub</a>.

If you'd like to live on the edge, or the flatpak version is unsuitable for some reason, read on.

## Installation

### Fedora 35+

Either install it by double-clicking in your graphical environment (or single-clicking if you have it set that way), or open a terminal and use your
package manager to install it. You do know what you're doing, right? In Fedora, the command is:

```
sudo dnf install <filename.rpm>
```

As a last resort, you could install it with

```
sudo rpm -i <filename.rpm>
```

But that might not play nice with your disto's package manager.

### openSUSE

I've only tested this on openSUSE LEAP 15.4. It installs fine there, but Tumbleweed and prior versions of LEAP might have different package names.
If that's the case, the rpm will probably complain about not finding anything to provide certain packages.

Ignore the warning that the rpm is unsigned. I don't know how to do that yet, and depending on how difficult it
is, I might not ever bother.

```
sudo zypper install <filename.rpm>
```

## Building it yourself

### Fedora 35+

**Setting up the environment**

By default, Fedora will want to build in `~/rpmbuild`. If it's somewhere different for your setup, replace ~/rpmbuild with your own build directory.
When you clone the git repository from github, DO NOT clone it into this folder, do it somewhere else. I'll use `~/build` for these instructions.

Download the required development packages:

```
sudo dnf rpmdevtools rpm-build
```

Set up the build directories if they don't already exist (`~/rpmbuild` and subfolders). Do NOT run with sudo:

```
rpmdev-setuptree
```

Install all the dependencies needed for the build. Theoretically this should be done by the rpmbuild tool, but it doesn't always work.

```
sudo dnf install aria2 SDL2 libsecret libattr fontconfig lcms2 libXcursor libXrandr libXdamage libXi gettext freetype mesa-libGLU libSM libgcc libpcap libFAudio desktop-file-utils jxrlib dotnet-sdk-6.0 git
```

**Compiling the code**

Now pull the source code.

```
mkdir -p ~/build
cd ~/build
git clone https://github.com/rankynbass/XIVLauncher4rpm.git
cd XIVLauncher4rpm
```
Now you can build the rpms.

`rpmbuild -bb XIVLauncher4rpm.spec` to just build a binary rpm or `rpmbuild -ba XIVLauncher4rpm.spec` if you want to build source rpm as well.
Be aware that the srpm will include the source files from goatcorp/FFXIVQuickLauncher repository, so it will be fairly large.

In the end you should have an rpm file in `~/rpmbuild/RPMS/x86_64/` called `XIVLauncher-<version>.x86_64.rpm`. If you build sources as well, that 
will be in `~/rpmbuild/SRPMS/`.

Install as mentioned in the "Installing" section. You can also build from the srpm with `rpmbuild --rebuild <filename.src.rpm>`.

### OpenSUSE

Instructions to follow. Probably not much different from fedora, but the package names will be different.
