# Custom Post Types

## Description

In this module, you'll learn about custom post types, what they are, when you should use them and how to create your owns to enhance your WordPress site.

## Objectives

At the end of this lesson, you will be able to:

*   Define custom post types in WordPress,
*   Identify when and how should custom post types be created,
*   Create your own simple custom post type using functions.php file of your theme.

##  Prerequisite Skills

You will be better equipped to work through this lesson if you have experience in and familiarity with:

*   WordPress [child themes](https://make.wordpress.org/training/handbook/theme-school/child-themes/),
*   [functions.php file](https://make.wordpress.org/training/handbook/theme-school/what-to-include-in-functions-php/)
*   [post vs pages](https://make.wordpress.org/training/handbook/user-lessons/pages-vs-posts/) difference

## Assets

*   Installation of WordPress
*   [Twenty Sixteen](https://wordpress.org/themes/twentysixteen/) theme

## Screening Questions

1.  Are you familiar with the concept of a child theme in WordPress?
2.  Do you have a self-hosted WordPress website?

## Teacher Notes

*   **Time Estimate:** 30 minutes
*   The recommended way to approach the scenarios would be to demonstrate and explain the process first and then ask students to repeat the actions using their own devices, while you’re available for questions and troubleshooting if something doesn’t work out.
*   It is easiest for students to work on a locally installed copy of WordPress. Set some time aside before class to assist students with installing WordPress locally if they need it or, if possible, send them out instructions before the class so they would come prepared. For more information on how to install WordPress locally, please visit our [Teacher Resources](https://make.wordpress.org/training/teacher-resources/) page.
*   The preferred answer to the screening questions is “yes.” Participants who reply “no” to question #1 might require a bit of explanation, and if they answer “no” to question # 2 they may be grouped with other students to work in pairs on the task.
*   You may print out the Hands-On Walkthrough part to use it as handouts or send it out as a .pdf file to keep it green and preserve the links used throughout the document.

## Hands-on Walkthrough

### Introduction to custom post types

#### What are custom post types?

Custom post types were introduced since [WordPress 3.0](http://codex.wordpress.org/Version_3.0). They are new post types that you can configure yourself. Since WordPress evolved from being a blogging platform into a robust content management system, the term _post_ stuck: so there are default post types in your WordPress installation such as post and page. However, a post can relate to any kind of content and you can create your own post types with different custom fields and characteristics and call them whatever you want. For example, if you create a website which hosts your professional portfolio you may want to create a new custom “Portfolio Item” post type.

#### How do you know when you need a custom post type?

Post types serve to distinguish different WordPress content types. Below are some signs indicating you should probably consider creating a custom post type for a particular content type you’re posting:

*   It isn't required to be a part of chronological series of entities.
*   It needs to be displayed differently from posts and/or pages.
*   It doesn’t look and feel like a post or a page.
*   You need some additional fields and characteristics.

#### How should you approach creating custom post types?

Custom post types are really requested quite often, so there are several ways you can choose from when you need one:

1.  Modifying child theme: t<span style="font-weight: 400">he easy way to register your new custom post is via your child theme’s function.php. However, what if you decide to change themes? While your data is still in the database, you can’t access it when the custom post is not registered in the new theme. Your configuration will actually be tied to your theme, breaking the “portability” WordPress principle,  which is about being able to access your data regardless of the theme in use.</span>
2.  Creating a plugin: this lets us avoid the drawback described earlier. If you create your own plugin out of your custom post, it will be portable between themes.
3.  <span style="font-weight: 400">And, of course, there are plenty of ready premium and free plugins which allow you to create a custom post type. Among most popular are [Custom Post Type UI](https://wordpress.org/plugins/custom-post-type-ui/), [Toolset Types](https://wordpress.org/plugins/types/), and [Custom Post Type Maker](https://wordpress.org/plugins/custom-post-type-maker/).</span>

For the training purposes, you will be using method #1 - modifying the theme's function.php. And of course in real life, you would be modifying your own child theme at this point, but now to make things easier, you will be editing [WordPress TwentySixteen Theme](https://wordpress.org/themes/twentysixteen/).

### Developing a Custom Post Type

#### Scenario

You want to develop a theme for an online portfolio website. So you want to have a new post type “Portfolio item”, which should be public, editable from the admin dashboard, and have some custom labels in the menu.

#### Registering a custom post type: The Basics

1\. Make sure you've activated [WordPress TwentySixteen Theme](https://wordpress.org/themes/twentysixteen/). </br>
2\. Open its function.php file in Sublime/gedit/Notepad/any other text editor you're comfortable using. The theme's function.php file's path should be: <your root WP folder>/wp-content/themes/twentysixteen. </br>
3\. Add the following piece of code to the very end of file.

<pre>//adding our own portfolio item
function my_custom_portfolio_item() {
$args = array();
register_post_type( 'portfolio_item', $args );
}

add_action( 'init', 'my_custom_portfolio_item' );</pre>

The first line is just a comment to let your future self and possibly others know what the modifications are for. Further on, you'll describe the new post type's settings (actually, lack of any custom settings for now) and register it. Finally, add_action directive is meant to add a described function to WordPress via the _init_ hook which runs after WordPress itself has finished loading, but before any headers are sent. [![custom_post_base](https://make.wordpress.org/training/files/2014/10/cutom_post_base.png)](https://make.wordpress.org/training/files/2014/10/cutom_post_base.png) </br>
4\. Make sure you save the file. </br>
5\. Now, check out your site's dashboard. In spite of the fact that you registered the new post you won't see any changes. That's because you haven't configured your post to be public and visible in the dashboard explicitly, and it's not by default.</br>

#### Registering a custom post type: adding arguments

1\. To fine-tune the new post type to your actual needs, you'll explore some of the most frequently-used options and add them to the $args array. This is how the previously added post should look like now.

<pre>//adding our own portfolio item
function my_custom_portfolio_item() {
  $labels = array(
    'name'               => _x( 'Portfolio items', 'post type general name' ),
    'singular_name'      => _x( 'Portfolio item', 'post type singular name' ),
    'menu_name'          => 'Portfolio'
  );
    $args = array(
    'labels'        => $labels,
    'description'   => 'Holds our custom portfolio items',
    'public'        => true,
    'menu_position' => 5,
    'supports'      => array( 'title', 'editor', 'thumbnail', 'excerpt', 'comments' ),
    'has_archive'   => true,
  );
  register_post_type( 'portfolio_item', $args ); } add_action( 'init', 'my_custom_portfolio_item' );</pre>

This is what you added:

*   **labels** is an array defining the different labels that a new custom post type can have.
*   **description** is a short explanation of the purpose behind the new type, i.e. what it is supposed to contain.
*   **public** controls how the post is visible to authors and readers. It can be further tinkered customizing visibility, searchability and queriability of the post.
*   **menu_position** is the position in the menu list.
*   **supports** lists the default WordPress controls that are required to be set up for the edit screen for this custom post type. The default settings are to show the title field and editor, so here you may want to add support for comments, revisions, etc.
*   **has_archive** determines whether or not you want a specific post type archive to be created for you.

[tip]For a full list of adjustable settings feel free to check out the [arguments section](https://codex.wordpress.org/Function_Reference/register_post_type#Arguments) in the Codex. [/tip] 2\. Save the functions.php file. 3\. Now, refresh your site's dashboard page. You should see a menu entry for the newly created custom post type, with options to add new portfolio items and view them in both the admin section and at the website as they are published. [![view_new_post_type](/images/view_new_post_type.png)](/images/view_new_post_type.png) [tip]Feel free to read the [dashboard icons lesson](https://make.wordpress.org/training/handbook/theme-school/dashboard-icons/) to learn how to customize the icon added for your new post type.[/tip]

### Summary

A custom post type is a regular post with a different post_type value in the database which is designed to hold a type of content different from standard posts and pages. You should think about creating a custom post type when you want your new content type to be different in looks, meaning, and content from the post types you already have registered on your site. Ideally, you should create a plugin when you need custom post types, but you also can modify your theme’s functions.php file. The basic modification resembles the following piece of code:

<pre>function my_custom_post_type() {
  $args = array();
  register_post_type( 'custom_post', $args ); 
}
add_action( 'init', 'my_custom_post_type' );</pre>

Arguments should be modified when you need your post to be public, displayed in the admin dashboard and/or further customized.

## Exercises

Using lesson's materials and [arguments section](https://codex.wordpress.org/Function_Reference/register_post_type#Arguments) in the Codex create a new post type "Movie Review" with the following characteristics:

*   public
*   displayed in the admin dashboard under "Comments"
*   supporting comments
*   no archive
*   [additional task] using the video icon from Dashicons

## Quiz

A short quiz for students to evaluate their retention of the material presented.

**What best describes the situation when you need to create a custom post type?**

1.  You need to create a portfolio site
2.  You want your items to be ordered chronologically
3.  Your new content type is different in looks, meaning, and content

**Answer:** 3.Your new content type is different in looks, meaning, and content 

<hr>

**Which of the following is the main benefit of creating your custom post type as a new plugin?**

1.  Portability
2.  Code validation
3.  Accessibility
4.  Being widget-ready

**Answer:** 1. Portability
