# stk-server
Guid how to install stk Server
Stolen from https://itrig.de/index.php?/archives/2370-HowTo-Einen-eigenen-SuperTuxKart-Server-im-LAN-aufsetzen-und-gegen-Freunde-zocken.html

First, a few packages need to be installed for the build to succeed.
```bash
sudo apt-get install build-essential cmake libbluetooth-dev \
libcurl4-openssl-dev libenet-dev libfreetype6-dev libfribidi-dev \
libgl1-mesa-dev libglew-dev libjpeg-dev libogg-dev libopenal-dev libpng-dev \
libssl-dev libvorbis-dev libxrandr-dev libx11-dev nettle-dev pkg-config zlib1g-dev git subversion
```
Next, the installation files are loaded onto the local system.
```bash
cd /opt
sudo mkdir stk-code
sudo mkdir stk-asset
git clone https://github.com/supertuxkart/stk-code stk-code
```
The last step may take some time as several 100MB have to be loaded.
```bash
svn co https://svn.code.sf.net/p/supertuxkart/code/stk-assets stk-assets
```
Now you have to wait a little bit, because depending on the CPU performance, this can take a while until it is compiled.
```bash
cd stk-code
sudo mkdir cmake_build
cd cmake_build/
sudo cmake .. -DSERVER_ONLY=ON
sudo make -j$(nproc)
```
Now run if you want to run it 
```bash
cd bin/
./supertuxkart --lan-server=test --network-console
```

Creating a service to run it 24/7
Create a systemd service unit file. In the terminal, run the following command
```bash
sudo nano /etc/systemd/system/supertuxkart-server.service
```
Paste the following content into the file:
```bash
[Unit]
Description=Supertuxkart LAN Server
After=network.target

[Service]
ExecStart=/opt/stk-code/cmake_build/bin/supertuxkart --lan-server=test --network-console
Restart=always
User=root

[Install]
WantedBy=multi-user.target
```
Now only run these commnds t oactivate it 
```bash
sudo systemctl enable supertuxkart-server.service
sudo systemctl start supertuxkart-server.service
sudo systemctl status supertuxkart-server.service
```
Done!
