## Jekyll SPT (Single Page Template)

Welcome to the Jekyll Single Page Template. It is based off of the [single_page_template](https://github.com/democrats/frontend-templates/tree/master/single_page_template) with some modifications for Jekyll compatibility.

Use the area below as a readme for your site:  
## My Site

*Github Page:*  
*Admin:*  
*Prod:*  
## Setup and initial installation

If this is your first time using the Single Page Template, you may need to install some dependencies.

### Ruby

macOS users will have ruby already, but it is recommended to upgrade to a newer version using Homebrew:

* Install [Homebew](https://brew.sh/)
* `brew install ruby`

### Bundle

Bundle is a tool for managing ruby dependencies. It is installed using a ruby package called `gem`, which you should already have from installing ruby:

`gem install bundle`

## Development

Clone then copy the template into your repo to get started:

`cp -R PATH/TO/frontend-templates/jekyll_spt/ PATH/TO/YOUR-REPO/`

If you are developing a site that uses the jekyll template, just follow the directions below:

`cd <your-repo>`

Now install Jekyll using Bundle:

`bundle`

You should messages about packages being installed, ending with something like: "Bundle complete! 6 Gemfile dependencies, 33 gems now installed."

Now you should be able to start your local jekyll server:

`jekyll serve --watch`

Go to `localhost:4000` to see it in action and `localhost:4000/admin` to access the admin panel

### Configuration of your site

Once you have the template copied to your repo, you can add the meta data through the config.yml:

Open `_config.yml` file and edit the following fields to be included in your metadata:

`title`
The title of your site

`description`
The short description of your site for SEO and social share.

`meta_img`
The image card for social share. This will need to be an absolute path to the production domain.

`url`
The domain of your site.

`ga`  
The Google Analytics ID for your site

`gtm`
The Google Tag Manager ID for your site

#### Collections

[Collections](https://jekyllrb.com/docs/collections/) are custom content types. There is a commented out collections in the `_config.yml` that can can uncomment to create your own collections. These are useful if you want to display unique sets of content like person bios or events.

### HTML/Homepage

Jekyll Admin supports full fledged websites that use layouts to display content on pages. The jekyll_spt is initially set up for a single page microsite that you can modify.

Open `_layouts/index.html` to edit the structure of the homepage.

Below is a list of what each variable displays and how to edit them:
`{{ site.title }}` comes from the `title` object in `_config.yml`

`{{ page.title }}` comes from the `index.md` title

`{{ content }}` comes from the body of `index.md`

`index` is the default layout for all pages created through jekyll.

#### Includes
Jekyll allows you to inject HTML into your content with premade html snippets. There are several includes that come with this template:

*base.html*  
Base is a helpful include from [Rico Sta. Cruz](https://ricostacruz.com/til/relative-paths-in-jekyll) that helps create relative paths that work in github pages as well as local development. It's used for the internal JS and CSS files sources for the index.html.

*form.html*  
Signup forms are pretty standard for all DNC microsites, hence the form include. All you have to do is get the form url and you can add a BSD form to your microsite with relative ease: `{% include form.html form-url="//your-form-url.com" %}`

*social_share.html*  
Social share is just a pair of pre-styled Facebook and Twitter icon links that you can drop anywhere in your project. The social share has params for you to customize the tweet text and url, plus two layouts. Below is an example include:
`{% include social_share.html layout="buttons" tweet="Tweet text" url="https://democrats.org" %}`

`layout`:
* `links` (default if not specified)
* `buttons`  

`tweet`: string

`url`: url (http://example.com)

*ga-event.html*  
GA Event is a include to simply enter Google Analytics click events. Below is an example include:
`{% include ga-event.html category='Social Share' label='Twitter' %}`

`category`: string
`label`: string

### SASS

Open `_sass/_custom.scss` and start adding your custom styles. At the top of the sheet there are several preset styles, mainly colors, for you to adjust. The brand colors are displayed at the top so you know what options you have. There are also preset breakpoints and padding for the three sections: header, main, and footer.

The base styles can be found in `_sass/_base.scss`.

#### PureCSS
[PureCSS](https://purecss.io/) is the default library for this template. Some helpful tools from PureCSS are [buttons](https://purecss.io/buttons/) and [grids](https://purecss.io/grids/). Gutters have been added between the grid columns for layout purposes and can be adjusted through the `$grid-gutters` variable on `_sass/_custom.scss`.

#### Mixins

*Breakpoints*
Mixins for breakpoints can be used for quick mobile styling:
`@include bp-large`
`@include bp-medium`
`@include bp-small`

Each of the breakpoints max-widths can be adjusted through the `$size-` variables on `custom.scss`.

*Flexbox*
Use `@include flexbox` for instant flexbox with all the browser prefixes.

*Box Shadow*
Use `@include box-shadow`for an elegant shaddow on divs and the like.

*Headings*
Use `@include headings` to apply styles for heading tags (eg `<h1>`). 

### JS

Add javascript files to `_includes/js/`, then include the js file with liquid in `site_functions.js`(in the root). Ex: `{% include js/social_links.js %}`. All js files will concatenate in `js/site_functions.js` and load at the bottom of `index.html`.

There is a blank `site_functions.js` file in the `includes/js` directory for you to start your site with.

### Images

Although it's possible to add images through Jekyll Admin's Static Files, I have added an images folder that has an image minifier for page load optimization. Follow the instruction below to install dependencies and run the image minifier

1. cd into your repo
2. run `npm install`
3. run `grunt`

Grunt will then watch for new images to be added and place the minified images in the `min_images` folder.

## Deployment

Once you are satisfied with your jekyll site, you can deploy it through Jenkins. But first you will need to update the files in the `config` folder. Systems will also need to know the domain name and the preferred admin name (typically admin.domain.com).

### Config

Basically, you will need to edit two files: `ansible/group_vars/all` and `terraform/production.tfvars`. Detailed information on what to modify in each file can be found in the [config readme](config/infrastructure/README.md).

You will need to determine an admin url (app_admin_hostname) for your site and a site IP address (jekyll_ip) along with the domain.

#### Admin URL
Admin urls are typically `admin.YOURSITE.com`. For democrats.org submdomains, the url is typically `ad`

#### Site IP Address  

### Jenkins

1. Once you set up the config files, go to https://jenkins2.dnc.org

2. Select `Systems` > `Templates` > `Jekyll Site`

3. Select `Build with Parameters` to create a new deploy

4. Fill the fields with the necessary params. Each field is detailed below:

  `APP_NAME`  
  Add the repo name and the domain in parenthesis. Ex) repo-name (site.com)

  `PROJECT`  
  Leave as `None`, unless the site is on a democrats.org submdomain. If that's the case, select `sites.democrats.org`

  `APP_REPO`
  Paste the SSH git clone url (clone or download > Use SSH). Ex: `git@github.com:democrats/microsite.git`

  `APP_BRANCH`
  Leave as `Master`

5. Select `Build` and you're done.

### Github Pages

This template is configured to work with github pages. Follow the steps below inorder to publish your site to github pages:

1. Go to the settings pane of your repo (eg. https://github.com/democrats/REPO-NAME/settings)
2. Scroll to the GitHub Pages section, then select master branch under source.
3. Select save, then github should display a message:  Your site is published at https://democrats.github.io/REPO-NAME/

This github page will be your staging environment, before changes are committed through the admin portal.

### Publishing Changes/Jekyll Admin

Jekyll Admin is the default CMS for this template. When you make edits to the core files (e.g. making edits to the SCSS), you will need to trigger a jekyll rebuild with Jekyll Admin:
1. Go to the admin portal for your site (`admin.yoursite.com/admin`)
2. Make a change to any of the content (you can just add a space to the default welcome to jekyll post)
3. Select Save to trigger a Jekyll build.
4. Go to the admin site (`admin.yoursite.com`) to see your changes that are committed.

If you make any changes to the `config.yml`, you will need to build your site in jenkins in order to publish your changes.
