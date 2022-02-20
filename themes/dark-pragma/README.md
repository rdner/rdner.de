# Dark Pragma

A nice purple theme for a blog.

## How to use

You need the following files in your blog sources to make the theme work properly:

### Your main logo

- `./static/images/logo.svg`

### Favicon stuff

- `./static/apple-touch-icon.png`
- `./static/favicon-16x16.svg`
- `./static/favicon-32x32.svg`
- `./static/favicon-120x120.svg`
- `./static/favicon.ico`
- `./static/site.webmanifest`
- `./static/safari-pinned-tab.svg`

The theme generates the following set of related HTML headers:

```html
<link rel="apple-touch-icon" sizes="180x180" href="{{ "/apple-touch-icon.png" | relURL }}">
<link rel="icon" type="image/svg+xml" sizes="16x16" href="{{ "/favicon-16x16.svg" | relURL }}">
<link rel="icon" type="image/svg+xml" sizes="32x32" href="{{ "/favicon-32x32.svg" | relURL }}">
<link rel="icon" type="image/svg+xml" sizes="120x120" href="{{ "/favicon-120x120.svg" | relURL }}">
<link rel="manifest" href="{{ "/site.webmanifest" | relURL }}">
<link rel="mask-icon" href="{{ "/safari-pinned-tab.svg" | relURL }}" color="#5a2673">
<meta name="apple-mobile-web-app-title" content="{{ .Site.Title }}">
<meta name="application-name" content="{{ .Site.Title }}">
<meta name="msapplication-TileColor" content="#5a2673">
<meta name="theme-color" content="#5a2673">
```

Highly recommended to use https://realfavicongenerator.net to generate the whole package of files which covers all platforms and browsers and will include some additional files that are supposed to be placed in these locations:

- `./static/android-chrome-192x192.png`
- `./static/android-chrome-512x512.png`
- `./static/browserconfig.xml`
- `./static/mstile-70x70.png`
- `./static/mstile-144x144.png`
- `./static/mstile-150x150.png`
- `./static/mstile-310x150.png`
- `./static/mstile-310x310.png`

### Configuration

This is a configuration file example that can be used with the theme:

```toml
baseURL = "https://website.local/" # absolute URL of the deployed web-site
languageCode = "en-us"
title = "Some fancy blog" # the main title of the web-site
theme = "dark-pragma" # this theme
relativeURLs = true # recommended to use relative URLs if possible
sectionPagesMenu = "main"
enableRobotsTXT = true
pygmentsCodefences = true

# recommended markup parameters
[markup]
  defaultMarkdownHandler = "blackFriday"
 [markup.blackFriday]
    hrefTargetBlank = true
    nofollowLinks = true
    noreferrerLinks = true

# theme-specific parameters
[params]
  UserPicture = "https://url-to-your-avatar" # your userpic that will be used for the small author description in posts
  AuthorName = "John Doe" # your name
  Description = "Some more detailed description"
  Github = "johndoe" # this will show a GitHub icon/link in bottom right
  Reddit = "johndoe" # this will show a Reddit icon/link in bottom right
  Twitter = "johndoe" # this will show a Twitter icon/link in bottom right
  Email = "john@doe.local" # this email goes to the footer of the web-site as your contact
  License = "Any HTML you want" # this will be placed as raw HTML in the footer after words "licensed under", so feel free to insert links and small images with inline styles.

# this is an example content structure that you might want to have.
# the section builds the main navigation panel in the header of the web-site, aligned to the right. Many root level items can cause layout problems on small screens
[menu]
  [[menu.main]]
    identifier = "posts"
    name = "Posts"
    title = "Blog posts"
    url = "/posts/"
    weight = -120
  [[menu.main]]
    identifier = "about"
    name = "About"
    title = "About the author"
    url = "/about/"
    weight = -110
```

### File structure

In order to generate your static web-site with Hugo you need to maintain the following file structure of your blog sources:

```
./content
  _index.md
  about/
    index.md
  posts/
    post1/
      index.md
    post2/
      index.md
./themes/
  dark-pragma
./static
  images/
    logo.svg
  apple-touch-icon.png
  favicon-16x16.png
  favicon-32x32.png
  favicon.ico
  site.webmanifest
  safari-pinned-tab.svg
  android-chrome-192x192.png
  android-chrome-512x512.png
  browserconfig.xml
  mstile-70x70.png
  mstile-144x144.png
  mstile-150x150.png
  mstile-310x150.png
  mstile-310x310.png
```
