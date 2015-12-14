---
layout: docs
title: bower
group: shades
redirect_from: "/shades/"
slug: bower
---

Executes a bower package manager command.

## Contents

* Will be replaced with the table of contents
{:toc}

## Supported Operating Systems

{% icon fa-apple fa-3x %} {% icon fa-windows fa-3x %} {% icon fa-linux fa-3x %}

## Arguments

The `bower` shade accepts the following arguments:

<div class="table-responsive">
    <table class="table table-bordered table-striped">
    <thead>
        <tr>
            <th style="width:100px;">Name</th>
            <th style="width:50px;">Type</th>
            <th style="width:50px;">Default</th>
            <th style="width:25px;">Required</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>args</td>
            <td>string</td>
            <td><code>null</code></td>
            <td><strong>Yes</strong></td>
            <td>The arguments to pass to the bower command line tool.</td>
        </tr>
        <tr>
            <td>options</td>
            <td>string</td>
            <td><code>${env:BOWER_OPTIONS}</code></td>
            <td>No</td>
            <td>Additional options to use when executing the bower command line tool</td>
        </tr>
        <tr>
            <td>path</td>
            <td>string</td>
            <td><code>${global:working_path}</code></td>
            <td>No</td>
            <td>The base path in which to execute bower.</td>
        </tr>
        <tr>
            <td>wait</td>
            <td>boolean</td>
            <td><code>true</code></td>
            <td>No</td>
            <td>A value indicating whether or not to wait for bower to exit before continuing.</td>
        </tr>
        <tr>
            <td>quiet</td>
            <td>boolean</td>
            <td><code>${global:quiet}</code></td>
            <td>No</td>
            <td>A value indicating whether or not to suppress standard output when executing the bower command line tool.</td>
        </tr>
    </tbody>
    <tfooter>
        <tr>
            <td colspan="5">All arguments are prefixed by <code>bower_</code>.</td>
        </tr>
    </tfooter>
    </table>
</div>

## Global Arguments

The following global arguments are used by `bower`:

<div class="table-responsive">
    <table class="table table-bordered table-striped">
    <thead>
        <tr>
            <th style="width:100px;">Name</th>
            <th style="width:50px;">Type</th>
            <th style="width:50px;">Default</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>base_path</td>
            <td>string</td>
            <td><code>$PWD</code></td>
            <td>The base path in which Condo was executed.</td>
        </tr>
        <tr>
            <td>working_path</td>
            <td>string</td>
            <td><code>${global:base_path}</code></td>
            <td>The working path in which Condo should execute shell commands.</td>
        </tr>
        <tr>
            <td>quiet</td>
            <td>boolean</td>
            <td><code>false</code></td>
            <td>A value indicating whether or not to suppress output when executing Condo.</td>
        </tr>
    </tbody>
    </table>
</div>

## Examples

### Bower

{% highlight sh %}
bower bower_args='install'
{% endhighlight %}

Note: Use the `bower-install` shade instead of calling `bower` directly. This shade exists primarily to support more specialized shades.

### Bower with Options

{% highlight sh %}
bower bower_args='install bootstrap' bower_options='--save-dev'
{% endhighlight %}

This would not be a likely scenario for a repeatable process, but its still possible.

### Bower Asynchronously

{% highlight sh %}
bower bower_args='install bootstrap' bower_quiet='${ true }' bower_wait='${ false }'
{% endhighlight %}

Any processes created with a `false` wait argument are monitored. The default goals will wait for running processes that have not yet exited during the
`#wait` stage of the default lifecycle. You can also use the `Build` utilities to wait for processes to complete at any time:

{% highlight sh %}
- Build.Wait();
{% endhighlight %}

A shade will be introduced in the future that will wrap this utility method.

## See Also

* [bower-install]({{site.baseurl}}/shades/bower-install)
* [bower-download]({{site.baseurl}}/shades/bower-download)