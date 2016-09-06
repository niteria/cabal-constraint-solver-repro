This is a repro for what could be bug in cabal 1.24.0.0 constraint solver.

To repro GHC 7.10.2 or GHC 7.10.3 is required and cabal 1.24.0.0.

What happens is:
```
Found no modified add-source deps.
Reading available packages...
/usr/bin/pkg-config --list-all
/usr/bin/pkg-config --modversion libdrm_nouveau form libdrm_amdgpu xcb-xfixes xf86dgaproto damageproto xcb-sync xcb-xselinux xcb-xtest x11-xcb xf86driproto libsepol libpcre renderproto dmxproto xcb-xvmc krb5-gssapi x11 xcb-present xcb-shape xau formw gl openssl compositeproto glproto xproto panelw icu menu xcmiscproto xcb-render xcb-dri2 panel xcb-dri3 libxslt kadm-client xcb mit-krb5-gssapi libssl xcb-xevie libexslt xext libdrm_nouveau2 xcb-composite xf86bigfontproto resourceproto xcb-dpms xmlsec1 libcrypto xpyb videoproto pthread-stubs zlib gssrpc fontutil xproxymngproto tic xdamage xcb-xinerama kdb xf86miscproto com_err libxml-2.0 xcb-res scrnsaverproto menuw ncursesw dri3proto xcb-xv fixesproto tinfo dri inputproto xcb-xf86dri xcb-damage libpcrecpp xcb-record xcb-xprint libselinux libffi xxf86vm shared-mime-info kbproto dri2proto xextproto bigreqsproto kadm-server fontsproto evieproto xfixes krb5 presentproto mit-krb5 emacs libdrm_intel xmlsec1-openssl glu xcb-glx recordproto xf86vidmodeproto xcb-shm ncurses xcb-randr libdrm_radeon xcb-screensaver xcb-xkb libdrm xineramaproto randrproto
Choosing modular solver.
Resolving dependencies...
cabal: Could not resolve dependencies:
trying: text-1.2.1.3:+integer-simple
next goal: integer-simple (dependency of text-1.2.1.3:+integer-simple)
rejecting: integer-simple-0.1.1.1 (only already installed instances can be
used)
Dependency tree exhaustively searched.
```

What's expected (from what @hvr told me) is that cabal should disable the flag
automatically to try to satisfy the constraints.

Changing `text-1.2.1.3/text.cabal` to have:
```
flag integer-simple
  description: Use the simple integer library instead of GMP
  default: False
  manual: False

```
fixes the issue.
