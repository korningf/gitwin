# Pacman

We want to install core utils and common security tools like OpenSSH and GnuPG (GPG)
We also need an  GLibC runtime and low-level libraries, like OpenSSL libgcrypt, libxml.

Gitbash has no package manager, but it is based on MSys, itself based on a Cygwin POSIX.
Now Gitbash itself is actually built in a separate Git-For-Windows Git-SDK-64 environment.
It is a full MSys POSIX with a dev toolchain and a port of the `pacman` package manager.
Thus we can just port the `pacman` packages from the windows `git-sdk-64` into Gitbash.



# Install pacman




* Install Pacman binaries

```bash
  cwd=`pwd`
  pushd /

  tar x -zvf ${cwd}/pacman-6.1.0-25-x86_64.pkg.tar.gz usr

  popd
```

* Configure Pacman repositories

```bash
  mkdir -p /etc/pacman.d/
  mkdir -p /var/lib/pacman
  mkdir -p /var/log/pacman
  mkdir -p /var/cache/pacman

  cp pacman.conf /etc/
  cp pacman.d/* /etc/pacman.d/
``` 


* Initialize Pacman keyring

```bash
  pacman-key --init
  pacman-key --add msys2-keyring.gpg
  pacman-key --add git-for-windows.gpg

  # msys2
  pacman-key --lsign-key D55E7A6D7CE9BA1587C0ACACF40D263ECA25678A
  pacman-key --lsign-key B91BCF3303284BF90CC043CA9F418C233E652008
  pacman-key --lsign-key 9DD0D4217D75A33B896159E6DA7EF2ABAEEA755C
  pacman-key --lsign-key 6E8FEAFF9644F54EED90EEA0790AE56A1D3CFDDC
  pacman-key --lsign-key 69985C5EB351011C78DF7F6D755B8182ACD22879

  # git-sdk
  pacman-key --lsign-key 91883E11E83DC29D14104DB4EDD44359093056EE
  pacman-key --lsign-key 5F944B027F7FE2091985AA2EFA11531AA0AA7F57
  pacman-key --lsign-key E8325679DFFF09668AD8D7B67115A57376871B1C
  #pacman-key --lsign-key 3B6D86A1BA7701CD0F23AED888138B9E1A9F3986
  
```


* Upgrade Pacman itself

```bash
pacman --noconfirm -Sc 
pacman --noconfirm -Syu 
pacman --overwrite '*' -Syu pacman 
```

*This will force a reinstall of the gitbash shell itself*.

*Once it completes, kill the shell and open another one.


* Upgrade Base packages

```bash
pacman --overwrite '*' -Syu base 
#pacman --overwrite '*' -Syu base-devel 

pacman --overwrite '*' -Syu tree
pacman --overwrite '*' -Syu pass

pacman --overwrite '*' -Syu cygrunsrv
pacman --overwrite '*' -Syu cygserver
```