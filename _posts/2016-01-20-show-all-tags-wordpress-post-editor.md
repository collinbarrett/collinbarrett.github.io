---
id: 924
title: 'Showing All Tags in the WordPress Post Editor'
date: '2016-01-20T08:52:09-06:00'
author: 'Collin M. Barrett'
excerpt: 'How to show all tags in the WordPress post editor when clicking the "Choose from the most used tags" link in
the sidebar.'
layout: post-wp-import
guid: '/?p=924'
permalink: /show-all-tags-wordpress-post-editor
redirect-from: /show-all-tags-wordpress-post-editor/
image: /assets/img/showAllTagsWordPressPostEditor_collinmbarrett.jpg
categories:
- Code
tags:
- PHP
- SEO
- WordPress
---

When editing a post in the WordPress post editor, there is a link in the sidebar “tags” section that reads “Choose from
the most used tags.” To improve my blog’s navigability and SEO, I want to use a significant amount (within reason) of
tags, and I want to do my best to ensure that I apply them to all possible posts. I was finding this difficult to do
without being able to show all of the existing tags in the WordPress post editor.

There were one or two very old plugins that seemed like they might solve the issue as a byproduct of their primary
function, but I wanted a lightweight patch to gain this functionality (without editing core, obviously). The solution to
follow was pieced together from the StackExchange post linked below, and it is working flawlessly. Just add the code
snippet to the functions.php in your child theme, and you should be good to go. Note that the link in the editor will
continue to read “Choose from the most used tags,” but WordPress will display all of the tags rather than the 45 most
popular.

```
<pre class="brush: php; title: ; notranslate" title="">
function wp_showAllTagsEditor ( $args ) {
if ( defined( 'DOING_AJAX' ) &amp;&amp; DOING_AJAX &amp;&amp; isset( $_POST['action'] ) &amp;&amp; $_POST['action'] === 'get-tagcloud' ) {
unset( $args['number'] );
$args['hide_empty'] = 0;
}
return $args;
}
add_filter( 'get_terms_args', 'wp_showAllTagsEditor' );
```

*via [StackExchange](https://wordpress.stackexchange.com/questions/174593/show-all-post-tags-on-post-edit-screen-sidebox/198778)*