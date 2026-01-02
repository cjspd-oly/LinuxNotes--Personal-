
### Commands
```bash
sudo dnf install qt6-qtbase-devel qt6-qtsvg-devel qt6-qttools-devel qt6-qtwayland-devel

cd ~/tmp
git clone https://github.com/flameshot-org/flameshot.git
cd flameshot

mkdir build
cd build

cmake -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=$HOME/.local \
      ..

make -j$(nproc)

cd ~/tmp/flameshot/build
make install
```

- This installs to: `~/.local/bin/flameshot`
### Run it
```bash
export PATH="$HOME/.local/bin:$PATH"
flameshot gui
```

### Uninstall later
```bash
rm -rf ~/.local/bin/flameshot ~/.local/share/flameshot ~/.local/share/applications/org.flameshot.Flameshot.desktop
```

## Install Latest
```bash
sudo dnf install flameshot
```