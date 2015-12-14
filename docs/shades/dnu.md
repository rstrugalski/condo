---
layout: docs
title: dnu
group: shades
---

@{/*

dnu
    Executes a dnu package manager command.

dnu_args=''
    Required. The arguments to pass to the dnu command line tool.

dnu_options='' (Environment Variable: DNU_OPTIONS)
    Additional options to include when executuing the dnu command line tool.

dnu_path='$(working_path)'
    The path in which to execute the dnu command line tool.

base_path='$(CurrentDirectory)'
    The base path in which to execute the dnu command line tool.

working_path='$(base_path)'
    The working path in which to execute the dnu command line tool.

dnu_wait='true'
    A value indicating whether or not to wait for exit.

dnu_quiet='$(Build.Log.Quiet)'
    A value indicating whether or not to avoid printing output.

*/}

use namespace = 'System'
use namespace = 'System.IO'

use import = 'Condo.Build'

dnvm-locate once='dnvm-locate'

default base_path           = '${ Directory.GetCurrentDirectory() }'
default working_path        = '${ base_path }'
default dnu_path            = '${ working_path }'
default dnvm_path           = '${ Build.GetPath("dnvm").Path }'

default dnu_args            = ''
default dnu_options         = '${ Build.Get("DNU_OPTIONS") }'
default dnu_wait            = '${ true }'
default dnu_quiet           = '${ Build.Log.Quiet }'
default dnu_runtime         = ''

var dnu_exec_cmd            = 'dnu'

@{
    Build.Log.Header("dnu");

    if (string.IsNullOrEmpty(dnu_args))
    {
        throw new ArgumentException("dnu: arguments must be specified when executing the dnu command line tool.", "dnu_args");
    }

    dnu_args = dnu_args.Trim();

    if (!string.IsNullOrEmpty(dnu_options))
    {
        dnu_options = dnu_options.Trim();
    }

    Build.Log.Argument("arguments", dnu_args);
    Build.Log.Argument("options", dnu_options);
    Build.Log.Argument("path", dnu_path);
    Build.Log.Argument("wait", dnu_wait);
    Build.Log.Argument("quiet", dnu_quiet);
    Build.Log.Header();

    if (!string.IsNullOrEmpty(dnu_runtime))
    {
        dnu_args = (dnu_args + " \"" + dnu_path + "\"").Trim();

        if (Build.Unix)
        {
            dnu_args = string.Format(@"bash -c ""source '{0}' && dnvm use {1} && {2} {3} {4}""", dnvm_path, dnu_runtime, dnu_exec_cmd, dnu_args, dnu_options);
            dnu_exec_cmd = "/usr/bin/env";
        }
        else
        {
            dnu_args = string.Format(@"use {0} && {1} {2} {3}", dnu_runtime, dnu_exec_cmd, dnu_args, dnu_options);
            dnu_exec_cmd = dnvm_path;
        }

        dnu_options = string.Empty;
    }
}

exec exec_cmd='${ dnu_exec_cmd }' exec_args='${ dnu_args } ${ dnu_options }' exec_path='${ dnu_path }' exec_wait='${ dnu_wait }' exec_quiet='${ dnu_quiet }'