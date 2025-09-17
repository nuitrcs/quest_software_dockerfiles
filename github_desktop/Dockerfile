## Use Debian bookworm as the base image
FROM debian:bookworm

## Update package lists and ensure package versions are up to date
RUN apt-get update && apt-get upgrade -y

## Install necessary packages
RUN apt-get install -y wget vim build-essential openssh-server openssh-client git curl 
RUN apt-get install -y nautilus gnome-terminal emacs gedit xorg 

## Install Firefox
## From https://support.mozilla.org/en-US/kb/install-firefox-linux#w_install-firefox-deb-package-for-debian-based-distributions
## With help from copilot \_('.')_/

# Create keyrings directory
RUN install -d -m 0755 /etc/apt/keyrings

# Download and convert Mozilla's GPG key
RUN wget -q https://packages.mozilla.org/apt/repo-signing-key.gpg -O - | gpg --dearmor -o /etc/apt/keyrings/mozilla.gpg
# Add Mozilla's APT repository
RUN echo "deb [signed-by=/etc/apt/keyrings/mozilla.gpg] https://packages.mozilla.org/apt mozilla main" > /etc/apt/sources.list.d/mozilla.list

# Set package pinning
RUN printf 'Package: *\nPin: origin packages.mozilla.org\nPin-Priority: 1000\n' > /etc/apt/preferences.d/mozilla

# Install Firefox
RUN apt-get update && apt-get install -y firefox

## Install github desktop

## This didnt work
## From https://github.com/shiftkey/desktop
#RUN wget -qO - https://apt.packages.shiftkey.dev/gpg.key | gpg --dearmor | tee /usr/share/keyrings/shiftkey-packages.gpg > /dev/null
#RUN sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/shiftkey-packages.gpg] https://apt.packages.shiftkey.dev/ubuntu/ any main" > /etc/apt/sources.list.d/shiftkey-packages.list'

## The apt package feed should remain updated from the shiftkey/desktop repository 
## https://github.com/shiftkey/desktop?tab=readme-ov-file#debianubuntu
## From https://github.com/shiftkey/desktop/issues/1062#issuecomment-2719160458
RUN wget -qO - https://mirror.mwt.me/shiftkey-desktop/gpgkey | gpg --dearmor | tee /usr/share/keyrings/mwt-desktop.gpg > /dev/null
RUN sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/mwt-desktop.gpg] https://mirror.mwt.me/shiftkey-desktop/deb/ any main" > /etc/apt/sources.list.d/mwt-desktop.list'

RUN apt update -y && apt install github-desktop -y 


