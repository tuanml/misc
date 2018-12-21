How to remote desktop directly to real X displays
=================================================

### Install x11vnc
```bash
sudo apt install x11vnc
```

### Start x11vnc
```bash
x11vnc -nowf -display :0
```

### Start x11vnc at startup
```bash
# Start x11vnc at startup
x11vnc -bg -nowf -nevershared -forever -display :0
```
