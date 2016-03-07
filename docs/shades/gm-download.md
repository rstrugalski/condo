---
layout: docs
title: gm-download
group: shades
---

@{/*

gm-download
    Downloads and installs the GraphicsMagick library.

    NOTE: This is only spuported on OS X at the present time, but will be expanded to include other
    operating systems in the future.

*/}

use namespace = 'System'

use import = 'Condo.Build'

@{
    Build.Log.Header("gm-download");

    var gm_version = string.Empty;
    var gm_installed = Build.TryExecute("gm", out gm_version, "-version");

    Build.Log.Argument("version", gm_version);
    Build.Log.Header();
}

brew-install brew_install_formula='graphicsmagick' if='!gm_installed && Build.OSX'