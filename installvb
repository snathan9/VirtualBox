#!/bin/bash
# Initial setup on new machine
#
if [ "$UID" -ne 0 ]
  then echo "This script should be run as root."
  exit 1
fi
#
echo "Edit file to update for correct/recent:"
echo "  (1) Distribution"
echo "  (2) Extension pack URL"
echo "  (3) Guest Additions URL - to be installed on Guest"
echo "  (4) Designated VB user"
echo "  (5) Location of VM away from default (Optional)"
echo "Shell Variables are: DEB_DIST, EXT_PACK_URL, GUEST_ADD_URL, VB_USER_ID"
echo "VB_BASE_LOC (Optional)"
echo "Run as root ..."
echo
read -p "Press [Enter] key to start or Ctrl-C to break and update script ..."
#
#
# Debian-based Linux distributions
# Trusty example below - CHANGE
DEB_DIST="deb http://download.virtualbox.org/virtualbox/debian trusty contrib"
#
# Version 4.3.12 example below - CHANGE
# The new version # will depend on version for base install (above)
EXT_PACK_URL="http://download.virtualbox.org/virtualbox/4.3.12/Oracle_VM_VirtualBox_Extension_Pack-4.3.12-93733.vbox-extpack"
EXT_PACK_FILE=$(basename $EXT_PACK_URL)
#
# GuestAdditions
# Update URL for latest version
# URL to follow verion # of base and extension pack - CHANGE
GUEST_ADD_URL="http://download.virtualbox.org/virtualbox/4.3.12/VBoxGuestAdditions_4.3.12.iso"
#
# Define the Designated VB User - CHANGE
VB_USER_ID=senthil
# Define the location for VirtualBox
# Optionally CHANGE
#VB_BASE_LOC="$HOME/avm"
VB_BASE_LOC="/home/$VB_USER_ID/avm"
VB_DLS_LOC=$VB_BASE_LOC/downloads
VB_DVD_LOC=$VB_BASE_LOC/dvd
VB_VMS_LOC=$VB_BASE_LOC/vms
VB_BIN_LOC=$VB_BASE_LOC/bin
# Share base location
VB_SHR_LOC=$VB_BASE_LOC/shares
# Individual Shared folder location path for VMs
# Check createvm script to designate path
VB_VM_HOST_PATH=$VB_SHR_LOC/share1
#
# Create directories for VB locations
mkdir -p $VB_BASE_LOC
mkdir -p $VB_DLS_LOC
mkdir -p $VB_DVD_LOC
mkdir -p $VB_VMS_LOC
mkdir -p $VB_BIN_LOC
mkdir -p $VB_SHR_LOC
mkdir -p $VB_VM_HOST_PATH
#
# Set user & permissions to VM location
chown -R $VB_USER_ID:$VB_USER_ID $VB_BASE_LOC
chmod 770 $VB_VMS_LOC
#
# Assign VB user to vboxusers group
usermod -a -G vboxusers $VB_USER_ID
#
echo
echo "Including Debian-based Linux distributions in apt sources"
read -p "Press [Enter] key to continue or Ctrl-C to break ..."
# Include Debian-based Linux distributions
echo $DEB_DIST >> /etc/apt/sources.list
#
echo
echo "Including Oracle public key for apt-secure"
read -p "Press [Enter] key to continue or Ctrl-C to break ..."
# Oracle public key for apt-secure
wget -q http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc \
  -O- | sudo apt-key add -
#
echo
echo "Updating apt, OS and installing dkms"
read -p "Press [Enter] key to continue or Ctrl-C to break ..."
apt-get update
apt-get -y upgrade
#apt-get -y dist-upgrade # May be required sometimes
apt-get -y install dkms
#
echo
echo "Downloading extension pack"
read -p "Press [Enter] key to continue or Ctrl-C to break ..."
# Download extension pack to the DVD folder
cd $VB_DVD_LOC
wget "$EXT_PACK_URL"
#
echo
echo "Downloading Guest Additions"
read -p "Press [Enter] key to continue or Ctrl-C to break ..."
# Download Guest Additiona to the DVD folder
cd $VB_DVD_LOC
wget "$GUEST_ADD_URL"
#
echo "-------------------------------------------------------"
#
apt-cache search virtualbox-4
#aptitude search virtualbox
#
echo
echo "Above is the list of virtualbox software available"
echo "Install required version of VirtualBox in another terminal first"
echo "e.g. virtualbox-4.3  - Oracle VM VirtualBox"
echo "Make sure you install from Oracle Distribution Sources and not Debian Sources"
echo "Ignore the default options such as: "
echo "  virtualbox - x86 virtualization solution - base binaries"
echo
echo "Return here after completing the install of VB (in another window)"
read -p "Press [Enter] key to continue or Ctrl-C to break ..."
#
echo
echo "Installing Extension Pack"
read -p "Press [Enter] key to continue or Ctrl-C to break ..."
VBoxManage extpack install $EXT_PACK_FILE
#
echo
echo "Open another Terminal as designated VB User $VB_USER_ID"
echo
echo "Run the following commands (as ${VB_USER_ID})"
echo
echo "#Setting default VirtualBox location:"
echo
echo "VBoxManage setproperty machinefolder $VB_VMS_LOC"
echo
echo "#Setting to Allow ANY guest resolution"
echo
echo "VBoxManage setextradata global GUI/MaxGuestResolution any"
echo
read -p "Press [Enter] key to continue or Ctrl-C to break ..."
#
echo "Done!"
#
exit
