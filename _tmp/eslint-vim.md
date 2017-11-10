---
layout: post
title:  "Quick & dirty guide for using ESLint with Vim"
# date:   2016-12-29 02:05:00 -0500
description: "How to configure the ESLint Javascript utility with the Vim editor"
tags:
- javascript
- vim
---
_Note: this assumes you have [ESLint](http://eslint.org/ "The ESLint homepage") already set up with your Javascript project — including any extra plugins needed for React or another framework._

### Installing and configuring the plugin

The easiest way to enable ESLint support in Vim is with using the [syntastic](https://github.com/scrooloose/syntastic "The syntasic plugin GitHub page") plugin. The [plugin’s README on Github](https://github.com/scrooloose/syntastic/blob/master/README.markdown "The syntasic Vim plugin README") has instructions for installing the plugin via the pathogen package manager, but for me, using [Vundle](https://github.com/VundleVim/Vundle.vim "The GitHub page for Vundle"), installation is as simple as adding this to my `.vimrc`:

```
Plugin 'scrooloose/syntastic'
```

And then running `:PluginInstall`.

Next, to configure the plugin, you’ll need to add a few lines to your `.vimrc`:
```
let g:syntastic_javascript_checkers = ['eslint']
let g:syntastic_always_populate_loc_list = 1
let g:syntastic_loc_list_height = 5
let g:syntastic_auto_loc_list = 0
let g:syntastic_check_on_open = 0
let g:syntastic_check_on_wq = 0
```

This configuration enables the ESLint syntax checker, will populate an error list in the location window (5 items high), but will not automatically open the error list. It does not check syntax on open, or at quitting, but it does check when saving.
These are my personal preferences, but there are many other options for configuration on the plugin’s README.

### Using the plugin

With Syntastic and ESLint now configured, you will now start to see signifiers in Vim showing where you have errors (according to your `.eslintrc`). Here’s an example:

![Screenshot with syntastic error signifiers in Vim gutter]({{ site.baseurl }}/assets/images/2017/01-vim-syntastic-1.png "Screenshot with syntastic error signifiers in Vim gutter")

In the left gutter you see the highlighted `>>` symbol pointing to lines with errors. If you move the cursor to that line, you will see the error displayed in the Vim command line, like this:

![Screenshot with a single reported syntastic/ESLint error]({{ site.baseurl }}/assets/images/2017/01-vim-syntastic-2.png "Screenshot with a single reported syntastic/ESLint error")

An underline is shown where errors occur as well. In the first screenshot above, you’ll see there’s an underline mark at the end of the first line with errors. That’s because we expect a semicolon there.

If you’d like to see all current errors in the current file, you can type `:Errors` in the Vim command line. The location list at the bottom of Vim will expand and look something like this:

![Screenshot with the full error list from syntastic/ESLint]({{ site.baseurl }}/assets/images/2017/01-vim-syntastic-3.png "Screenshot with the full error list from syntastic/ESLint")

That’s it for the basic usage of Syntastic and ESLint in Vim. But below are a few extras for making this combination of plugins work even better.

### Bonus tips

**Using a local ESLint executable:** If you use [nvm](https://github.com/creationix/nvm "The nvm GitHub page") to manage different Node versions per project, and don’t feel like installing ESLint globally, you can install the [syntastic-local-eslint.vim](https://github.com/mtscout6/syntastic-local-eslint.vim "syntastic-local-eslint Vim plugin page on GitHub") plugin. Now, syntastic will look for a local ESLint executable (in ./node_modules/.bin/) before looking for a global version. No extra configuration is necessary.

**Add to status bar:** If you’d like the count of errors included in your Vim status bar, you can add the following to your statusline config:

```
set statusline+=\ %#warningmsg#
set statusline+=%{exists('g:loaded_syntastic_plugin')?SyntasticStatuslineFlag():''}
set statusline+=%*
```

And the display of that will look something like this (see right side):

![Screenshot with the syntastic message in Vim status line]({{ site.baseurl }}/assets/images/2017/01-vim-syntastic-4.png "Screenshot with the syntastic message in Vim status line")

**Debugging:** If for some reason you aren’t getting the errors showing up in Vim, you can run a few commands to enable debugging messages. With a Javascript file in the buffer, type the following in the Vim command line:

```
:let g:syntastic_debug=3
:SyntasticCheck eslint
:mes
```

Upon typing `:mes`, you should be shown any debug output from syntastic with (hopefully) a message about what went wrong, like `“eslint executable not found”`.

**Getting fancy:** If you would like to get a little more expressive with your gutter error signifiers, Alex Young at [usevim](http://usevim.com/ "The usevim homepage") has a [good explanation on how to add fancier graphics](http://usevim.com/2016/03/07/linting/ "Blog post explaining how to add fancier graphics to syntastic output"). The video is also a better, and more in-depth look at getting Syntastic and ESLint working with Vim. And don’t miss out on the rest of the excellent tutorials and tips on the site.
