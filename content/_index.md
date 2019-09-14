+++
title = "Let's Get JAMming"
description = "An Introduction to Static Site Generators"
outputs = ["Reveal"]
[reveal_hugo]
custom_theme = "reveal-hugo/themes/robot-lung.css"
margin = 0.2
highlight_theme = "color-brewer"
transition = "slide"
transition_speed = "fast"
[reveal_hugo.templates.hotpink]
class = "hotpink"
background = "#FF4081"
+++

![jamming](images/jamming.jpg)

# Let's Get JAMming

An Introduction to Static Site Generators

----

Dennis Stepp
<br>
![profile-pic](images/about/profile-circle.png)

Product Owner
<br>
![profile-pic](images/about/lirio.png)

----

## JAMStack?

"A modern web development architecture based on client-side **JavaScript**, reusable **APIs**, and prebuilt **Markup**" 

![Mathias Biilmann](images/jamstack/mathias-biilmann.png)
- Mathias Biilmann, CEO at ![Netlify](images/jamstack/netlify.png)

----

## Why should I care?

Barrier to entry is super low.

Large value, small investment.

Simpler infrastructure, less dependencies.

Tons of resources to get you to market quickly.

----

## You said Generators... so there is more than one?

Jekyll, Hugo, Gatsby, Hexo, GitBook, VuePress, Docusaurus, Pelican, MkDocs, Zola, Hakyll, Wyam...

----

## Is this actually used in the real world?

2012: Obama's presidential campaign raised $250 Million through a site built on Jekyll.

2013: Healthcare.gov switched to a static site using Jekyll.

2018: Let's Encrypt website was release after being ported from Jekyll to Hugo.

2018: Linode moved their documentation to Hugo taking their build time down from minutes to seconds.

----

## I don't have a lot of time to build a whole site.

1) Install hugo.

2) Create new Hugo site. `hugo new site minimal-blog`

3) Init a git repo. `git init`

4) Pick a theme and add it `git submodule add https://github.com/calintat/minimal.git themes/minimal`

5) Setup your config in `config.toml`. 

----

Cheat mode: Copy and paste from the example site from the submodule and make edits for your site.

```
baseURL = "http://example.com/"
languageCode = "en-us"
title = "Minimal"
theme = "minimal"
disqusShortname = "username" # delete this to disable disqus comments
googleAnalytics = ""

[params]
    author = "Calin Tataru"
    description = "Personal blog theme powered by Hugo"
    githubUsername = "#"
    accent = "red"
    showBorder = true
    backgroundColor = "white"
    font = "Raleway" # should match the name on Google Fonts!
    highlight = true
    highlightStyle = "solarized-dark"
    highlightLanguages = ["go", "haskell", "kotlin", "scala", "swift"]

[[menu.main]]
    url = "/"
    name = "Home"
    weight = 1

[[menu.main]]
    url = "/post/"
    name = "Posts"
    weight = 2

[[menu.main]]
    url = "/project/"
    name = "Projects"
    weight = 3

# Social icons to be shown on the right-hand side of the navigation bar
# The "name" field should match the name of the icon to be used
# The list of available icons can be found at http://fontawesome.io/icons/

[[menu.icon]]
    url = "mailto:me@example.com"
    name = "envelope-o"
    weight = 1

[[menu.icon]]
    url = "https://github.com/username/"
    name = "github"
    weight = 2

[[menu.icon]]
    url = "https://twitter.com/username/"
    name = "twitter"
    weight = 3

[[menu.icon]]
    url = "https://www.linkedin.com/in/username/"
    name = "linkedin"
    weight = 4

[[menu.icon]]
    url = "https://www.stackoverflow.com/username/"
    name = "stack-overflow"
    weight = 5
```

----

6) Add content. `hugo new posts/my-first-post.md`.

7) See your content. `hugo server`.

8) Ready to build run `hugo`.

----

![minimal-blog](images/jamstack/minimal-blog.png)

----

## That's great you made a blog.  I don't need a blog.

How about some project documentation instead? Well, it's just as easy.

----

Let's pick a different theme.

![whisper-doc](images/jamstack/whisper-doc.png)

----

## Oh, cool. A site that was out of date before it was even published.

You're not wrong. So how about a product page instead.

----

Let's pick yet a different theme.

![whisper-doc](images/jamstack/infinity-product-page.png)

----

## So I can create a blog, project documentation, and product landing page...

`wyam new -r Blog`

----

## Create a product page in markdown

```
Url: /
Name: Epiphone MM50E Profesional Electric Mandonlin In Natural
Description: The Epiphone MM50 Mandolin offers a professional solution to live mandolin playing. With a new and highly versatile pickup system, this mandolin is optimised for both its acoustic and electric tone. The Shadow pickup system is fully adjustable and can be moved to better suit your playing style, or to simply alter your amplified tone. Never before has a mandolin sounded so good in a live environment!
Image: epihone-MM50E.png
Price: 599.00
Sku: MM50E
Title: Epiphone MM50E Profesional Electric Mandonlin In Natural
---
The Epiphone MM50 Mandolin offers a professional solution to live mandolin playing. With a new and highly versatile pickup system, this mandolin is optimised for both its acoustic and electric tone. The Shadow pickup system is fully adjustable and can be moved to better suit your playing style, or to simply alter your amplified tone. Never before has a mandolin sounded so good in a live environment!
```

----

## Create Another Product

```
Url: /
Name: Martin Guitars D-28JP
Description: More than a million musicians are already trusting us!, 115 musicians at your service, Best price guarantee, 400 000 items in stock
Image: martin-D28JP
Price: 6178.00
Sku: D-28JP
Title: Martin Guitars D-28JP
---
More than a million musicians are already trusting us!, 115 musicians at your service, Best price guarantee, 400 000 items in stock
```

----

## Adding product

```
Pipelines.Add("Products",
     ReadFiles("products/*.md"),
     // Read all markdown files in the "products" directory
     FrontMatter(Yaml()),
     // Load any frontmatter and parse it as YAML markup
     Markdown(),
     // Render the markdown content
     Meta("Product", @doc.Content),
     // Move the markdown content to metadata
     Meta("ProductFile", string.Format(@"products\{0}.html", ((string)@doc["Title"]).ToLower().Replace(' ', '-'))),
     // Use the product title as the filename and save it in metadata so we can use it in links later
     Merge(
         ReadFiles("products/products.cshtml")
         // Load the Razor product page template
     ),
     Razor(),
     // Compile and render the page template
     WriteFiles((string)@doc["ProductFile"])  
     // Write the product file
 );
```

----

## Displaying a list of Products

```
<div class="row">
  @foreach(var doc in Documents["Products"])
    {
    <a href="@doc["ProductFile"]">
      <div class="col-lg-4 col-md-6 mb-4">
        <div class="card h-100">
          <a href="#"><img src="@doc["Image"]" width="150px" class="thumbnail"/></a>
          <div class="card-body">
            <h4 class="card-title">
              <a href="#"><p>@doc["Title"]</p></a>
            </h4>
            <h5>$@doc["Price"]</h5>
            <p class="card-text">@doc["Description"]</p>
          </div>
          <div class="card-footer">
            <small class="text-muted">&#9733; &#9733; &#9733; &#9733; &#9734;</small>
          </div>
        </div>
      </div>
    </a>
  }
</div><!-- /.row -->
```

----

## Displaying a single product

```
<h2>@Metadata["Name"]</h2>
<div class="product-details">
    <img src="@Metadata["Image"]" width="150px" class="thumbnail"/>
    <div class="product-description">
        <p>
            @Html.Raw(Metadata["Product"])
        </p>
        <button class="snipcart-add-item"
                data-item-name="@Metadata["Name"]"
                data-item-description="@Metadata["Description"]"
                data-item-id="@Metadata["Sku"]"
                data-item-image="@Metadata["Image"]"
                data-item-price="@Metadata["Price"]">
            Buy it for @Metadata["Price"] $
        </button>
    </div>
</div>
```
 
 ----

## After Half a Morning Without Coffee

![wyam-ecommerce](images/jamstack/wyam-ecommerce.png)

----

## Seems pretty technical could I give control over to my content creators?

Don't want to have to deal with the work to update these pages? Give them a static CMS.

----

![netlify-cms-page](images/jamstack/netlify-cms-page.png)

----

![netlify-cms-admin](images/jamstack/netlify-cms-admin.png)

----

## So I have to dump this on a web server to deploy it?

Deploy it on your dedicated resources, to AWS/Azure/Google, or a CDN. 

![Netlify](images/jamstack/netlify.png)

Global CDN, Continuous Deployment, HTTPS Support

----

## Additional Resources

![Netlify](images/jamstack/jamstack-radio.png)

https://jamstack.wtf/

https://dennis-stepp.com/

Twitter: @destepp

----

You have a story.
<br>
Go forth and **share it**.