# Build and Host a Static Website
\label{cha:static_website}

Everybody has to start somewhere...so let's start with a static website!

## Static vs Dynamic Websites

Static means the pages delivered to the client are the same for everyone.  Dynamic is the opposite and each client may receive different content.  In general, web applications are dynamic because users are authenticated against a database and receive content based on their account, privileges, and/or roles.  A website advertising the hours for a local hardware store could be static in that every one visiting that website sees the same information.  Static web servers give everyone the same html file.  Dynamic servers involve logic, database connections, and provide changing content by building the html file 'on the fly'...aka dynamically.

## Our First Gig

As an example, this chapter will focus on building a website for a technology consulting service, AgilePhd. **Note: This exercise assumes that you are familiar with creating GitHub repositories.  If not, try working through the [GitHub Hello World](https://guides.github.com/activities/hello-world/) tutorial.**  

The finished product will look something like this:
![agilephd](images/agilephd.png)

...but you usually start with something like this:
![agilephd](images/agilephd_start.png)

Generated with this minimal code:
\begin{codelisting}
\label{code:basic_html}
\codecaption{index.html}
```html
<html>
<body>
  Coming soon!
</body>
</html>
```
\end{codelisting}

Inside your Projects folder, create a subfolder called 'agilephd' and copy the above code into a file called index.html.

We are going to use a service called [GitHub Pages](https://pages.github.com/) so let's create a git repository for this code.  You get one freely hosted website per GitHub account. Head over to [GitHub](https://github.com/) and create a new repository named username.github.io, where username is your username on GitHub.

```
$ git init
$ git add .
$ git commit -m 'initial commit'
$ git remote add origin https://github.com/username/username.github.io.git
    (where username is your username)
$ git push -u origin master
```

Goto your new GitHub repository and visit the settings page.  Scroll down to the 'GitHub Pages' section and publish the page.
![agilephd githubpages](images/agilephd_publish.png)

Now you should be able to view your 'coming soon!' page at username.github.io


## Plagiarize

In technology, plagiarism is not only tolerated but it is encouraged and celebrated.  Open source communities endorse this practice by publicly posting code.  All that is asked in return is that you share your enhancements with the community creating a win-win atmosphere.

Don't spend time reinventing the wheel.  Find a solution that works and borrow heavily.  That is exactly where we are going to start with our static website.  

## Zurb Foundation

HTML is the language of the web.  Web-servers know how to send it and browsers know how to interpret it.  HTML is great.  HTML is awesome...but writing it can be a bit mundane and slow.  Did I mention you should borrow from others?  No, it is not stealing and it is not wrong.  We all do it and as a community, we all 'borrow' from each other.  Let's start by borrowing from a company that has made it easy to find their code.

Zurb is a design firm that creates awesome websites for clients.  They are also the creators of Foundation, a responsive front-end framework.  Zurb's Foundation or Zurb Foundation or just Foundation is a javascript library that helps your web pages look better.  Foundation, and other responsive frameworks such as Bootstrap, make it easy to design beautiful responsive websites, apps and emails that look amazing on any browser or any device.

Check out [Zurb](https://foundation.zurb.com/) to learn more about the framework (but don't spend too much time as it is not important for this course).  

Zurb provides several [templates](https://get.foundation/templates.html) of their framework.  Sign up to download all the templates or you can scroll down to view demos.
![foundation templates](images/foundation_templates.png)


For the consulting firm gig, let's start with the 'agency' template.
![agency template](images/agency_template.png)

For the cost of signing up with Zurb (which was free), we now have some robust HTML code to build upon.

\begin{codelisting}
\label{code:foundation_template}
\codecaption{index.html}
```html
<!doctype html>
<html class="no-js" lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Foundation | Welcome</title>
    <link rel="stylesheet"
      href="https://dhbhdrzi4tiry.cloudfront.net/cdn/sites/foundation.min.css">
  </head>
  <body>

    <!-- Start Top Bar -->
    <div class="top-bar">
      <div class="top-bar-left">
        <ul class="menu">
          <li class="menu-text">Yeti Agency</li>
          <li><a href="#">One</a></li>
          <li><a href="#">Two</a></li>
        </ul>
      </div>
      <div class="top-bar-right">
        <ul class="menu">
          <li><a href="#">Three</a></li>
          <li><a href="#">Four</a></li>
          <li><a href="#">Five</a></li>
          <li><a href="#">Six</a></li>
        </ul>
      </div>
    </div>
    <!-- End Top Bar -->

    <div class="callout large">
      <div class="row column text-center">
        <h1>Changing the World Through Design</h1>
        <p class="lead">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
          Nullam in dui mauris.
        </p>
        <a href="#" class="button large">Learn More</a>
        <a href="#" class="button large hollow">Learn Less</a>
      </div>
    </div>

    <div class="row">
      <div class="medium-6 columns medium-push-6">
        <img class="thumbnail" src="https://placehold.it/750x350">
      </div>
      <div class="medium-6 columns medium-pull-6">
        <h2>Our Agency, our selves.</h2>
        <p>
          Vivamus luctus urna sed urna ultricies ac tempor dui sagittis.
          In condimentum facilisis porta. Sed nec diam eu diam mattis viverra.
          Nulla fringilla, orci ac euismod semper, magna diam porttitor mauris,
          quis sollicitudin sapien justo in libero. Vestibulum mollis mauris enim.
          Morbi euismod magna ac lorem rutrum elementum. Donec viverra auctor.
        </p>
      </div>
    </div>

    <div class="row">
      <div class="medium-4 columns">
        <h3>Photoshop</h3>
        <p>Vivamus luctus urna sed urna ultricies ac tempor dui sagittis.
        In condimentum facilisis porta. Sed nec diam eu diam mattis viverra.
        Nulla fringilla, orci ac euismod semper, magna.</p>
      </div>
      <div class="medium-4 columns">
        <h3>Javascript</h3>
        <p>Vivamus luctus urna sed urna ultricies ac tempor dui sagittis.
        In condimentum facilisis porta. Sed nec diam eu diam mattis viverra.
        Nulla fringilla, orci ac euismod semper, magna.</p>
      </div>
      <div class="medium-4 columns">
        <h3>Marketing</h3>
        <p>Vivamus luctus urna sed urna ultricies ac tempor dui sagittis.
        In condimentum facilisis porta. Sed nec diam eu diam mattis viverra.
        Nulla fringilla, orci ac euismod semper, magna.</p>
      </div>
    </div>

    <hr>

    <div class="row column">
      <ul class="vertical medium-horizontal menu expanded text-center">
        <li><a href="#"><div class="stat">28</div><span>Websites</span></a></li>
        <li><a href="#"><div class="stat">43</div><span>Apps</span></a></li>
        <li><a href="#"><div class="stat">95</div><span>Ads</span></a></li>
        <li><a href="#"><div class="stat">59</div><span>Cakes</span></a></li>
        <li><a href="#"><div class="stat">18</div><span>Logos</span></a></li>
      </ul>
    </div>

    <hr>

    <div class="row column">
      <h3>Our Recent Work</h3>
    </div>

    <div class="row medium-up-3 large-up-4">
      <div class="column">
        <img class="thumbnail" src="https://placehold.it/550x550">
      </div>
      <div class="column">
        <img class="thumbnail" src="https://placehold.it/550x550">
      </div>
      <div class="column">
        <img class="thumbnail" src="https://placehold.it/550x550">
      </div>
      <div class="column">
        <img class="thumbnail" src="https://placehold.it/550x550">
      </div>
      <div class="column">
        <img class="thumbnail" src="https://placehold.it/550x550">
      </div>
      <div class="column">
        <img class="thumbnail" src="https://placehold.it/550x550">
      </div>
      <div class="column">
        <img class="thumbnail" src="https://placehold.it/550x550">
      </div>
      <div class="column">
        <img class="thumbnail" src="https://placehold.it/550x550">
      </div>
      <div class="column">
        <img class="thumbnail" src="https://placehold.it/550x550">
      </div>
      <div class="column">
        <img class="thumbnail" src="https://placehold.it/550x550">
      </div>
      <div class="column">
        <img class="thumbnail" src="https://placehold.it/550x550">
      </div>
      <div class="column">
        <img class="thumbnail" src="https://placehold.it/550x550">
      </div>
    </div>

    <hr>

    <div class="row column">
      <ul class="menu">
        <li><a href="#">One</a></li>
        <li><a href="#">Two</a></li>
        <li><a href="#">Three</a></li>
        <li><a href="#">Four</a></li>
      </ul>
    </div>

    <script src="https://code.jquery.com/jquery-2.1.4.min.js"></script>
    <script src="https://dhbhdrzi4tiry.cloudfront.net/cdn/sites/foundation.js">
    </script>
    <script>
      $(document).foundation();
    </script>
  </body>
</html>
```
\end{codelisting}

Copy / paste the above code into your index.html file and push to github.

```
$ git add .
$ git commit -m "updating page with a template from Zurb Foundation"
$ git push
```
Now you should be able to view the Zurb Foundation template page at username.github.io

Time to modify the template for the AgilePhD consulting firm.

\begin{codelisting}
\label{code:modified_template}
\codecaption{index.html}
```html
<!doctype html>
<html class="no-js" lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>AgilePhD | Welcome</title>
    <link rel="stylesheet"
      href="https://dhbhdrzi4tiry.cloudfront.net/cdn/sites/foundation.min.css">
    <link rel="stylesheet" href="https://s3.amazonaws.com/agilephd/styles/main.css">
    <link href="https://fonts.googleapis.com/css?family=Montserrat" rel="stylesheet">
  </head>
  <body>

    <!-- Start Top Bar -->
    <div class="top-bar">
      <div class="top-bar-left">
        <ul class="menu">
          <li class="menu-text agilephd">AgilePhD</li>
        </ul>
      </div>
      <div class="top-bar-right">
        <ul class="menu">
          <li><a data-open="contactModal">Contact</a></li>
        </ul>
      </div>
    </div>
    <!-- End Top Bar -->

    <div class="callout large scrum">
      <div class="row column text-center bottom">
        <h1 class="scrum_h1 bottom">Organizational Change Through Agile Methods</h1>
      </div>
    </div>

    <div class="row">
      <div class="medium-6 columns medium-push-6">
        <img class="thumbnail pm_image"
          src="https://s3.amazonaws.com/agilephd/project_management.jpg">
      </div>
      <div class="medium-6 columns medium-pull-6">
        <h2>Agile Project Management</h2>
        <p>
          As an experienced agile project manager, AgilePhD is the niche provider
          clients seek out when they are looking to implement business-enhancing,
          agile PMO services that improve project and portfolio performance.
        </p>

      </div>
    </div>

    <div class="row">
      <div class="medium-4 columns">
        <h3>Project Management</h3>
        <p>
          Speed is what companies want as they adopt Agile. Traditional
          plan-driven methodologies like Waterfall make a lot of up front
          assumptions and leave no flexibility to adapt to the changing
          needs of the customer.
        </p>
      </div>
      <div class="medium-4 columns">
        <h3>Software Development</h3>
        <p>
          It's all about that MVP (minimally viable product) and getting
          it in front of your customers.  I love building technology products.  
        </p>
      </div>
      <div class="medium-4 columns">
        <h3>Cloud Services</h3>
        <p>
          As an early adopter several cloud technologies, AgilePhD can
          help you develop a scalable solutions allowing you to focus
          on your core business.
        </p>
      </div>
    </div>

    <hr>

    <div class="row column">
      <ul class="vertical medium-horizontal menu expanded text-center">
        <li>
          <a href="#"><div class="stat">600+</div><span>Students Trained</span></a>
        </li>
        <li>
          <a href="#"><div class="stat">20+</div><span>Web App Projects</span></a>
        </li>
        <li>
          <a href="#"><div class="stat">12+</div><span>Mobile App Projects</span></a>
        </li>
      </ul>
    </div>

    <hr>

    <div class="row column">
      <h3>Recent Web Apps</h3>
    </div>

    <div class="row medium-up-3 ">
      <div class="column">
        <img class="thumbnail"
          src="https://s3.amazonaws.com/agilephd/damagelist.png">
      </div>
      <div class="column">
        <img class="thumbnail"
          src="https://s3.amazonaws.com/agilephd/health_engagements.png">
      </div>
      <div class="column">
        <img class="thumbnail"
          src="https://s3.amazonaws.com/agilephd/animalminder_2.jpg">
      </div>
      <div class="column">
        <img class="thumbnail"
          src="https://s3.amazonaws.com/agilephd/sabot_solutions.png">
      </div>
      <div class="column">
        <img class="thumbnail"
          src="https://s3.amazonaws.com/agilephd/sicklewell.png">
      </div>
      <div class="column">
        <img class="thumbnail"
          src="https://s3.amazonaws.com/agilephd/rails_girls.png">
      </div>

    </div>

    <hr>

    <div class="row column">
      <h3>Recent Mobile Apps</h3>
    </div>

    <div class="row small-up-2 medium-up-5 ">
      <div class="column">
        <img class="thumbnail"
          src="https://s3.amazonaws.com/agilephd/damagelist_ios_3.jpg">
      </div>
      <div class="column">
        <img class="thumbnail"
          src="https://s3.amazonaws.com/agilephd/btr_crime.jpeg">
      </div>
      <div class="column">
        <img class="thumbnail"
          src="https://s3.amazonaws.com/agilephd/seattle_crime.jpeg">
      </div>
      <div class="column">
        <img class="thumbnail"
          src="https://s3.amazonaws.com/agilephd/sfo_crime.jpeg">
      </div>
      <div class="column">
        <img class="thumbnail"
          src="https://s3.amazonaws.com/agilephd/sicklewell_ios.png">
      </div>

    </div>

    <hr>

    <div class="row column">
      <ul class="menu">

        <li>
          <a href="https://www.linkedin.com/in/jdavisphd/" class="" target="_blank">
            <img src="https://s3.amazonaws.com/agilephd/webicon-linkedin-m.png">
          </a>
        </li>
        <li>
          <a href="https://github.com/cavalryjim" class="" target="_blank">
            <img src="https://s3.amazonaws.com/agilephd/webicon-github-m.png">
          </a>
        </li>
      </ul>
    </div>

    <div class="reveal" id="contactModal" data-reveal>
      <h4>Contact information</h4>
      <ul class="no-bullet">
        <li>Dr. James Davis, PhD, PMP, CSP, CSM</li>
        <li>
          <a href="mailto:james@agilephd.com?Subject=AgilePhD" target="_top">
            james@agilephd.com
          </a>
        </li>
        <li><a href="tel:+12258920230">+1.225.892.0230</a></li>
      </ul>
      <button class="close-button" data-close aria-label="Close modal" type="button">
        <span aria-hidden="true">&times;</span>
      </button>
    </div>

    <script src="https://code.jquery.com/jquery-2.1.4.min.js"></script>
    <script src="https://dhbhdrzi4tiry.cloudfront.net/cdn/sites/foundation.js"></script>
    <script>
      $(document).foundation();
    </script>
  </body>
</html>
```
\end{codelisting}

Update the index.html file with the new code (from above) and push to github.

```
$ git add .
$ git commit -m "customizing index.html for AgilePhd"
$ git push
```

When viewed in a browser, username.github.io should look something like this:
![agilephd local](images/agilephd_local.png)

## Hosting Webpages

Now that we have a decent static webpage, it is time to host it.

We are living in the glory days of technology.  While there are unlimited options for hosting websites, we are going to focus on options that are 1) free and 2) worthy your time & effort.[^effort_footnote]  

\begin{aside}
\label{aside:polytex_markdown}
\heading{Hosting Services}

\noindent Word Press, Wix, Squarespace, along with most Domain Registration Services, have wizard-style website creation abilities.  While this may work for some situations, I've found these 'wizard' solutions to be very heavy and overly constraining.  As the technical lead for your venture, developing your own webpages gives you flexibility with hosting options and features.

While laws vary from state to state, content, such as a website, is owned by the person or entity that created it. This is more reason to for you to develop your own websites and/or applications. If you do happen to utilize outside services, read the contract and consult an attorney as needed.
\end{aside}

If you didn't already think of yourself as a web developer, now you can.  You have created an HTML page and published it at username.github.io (where 'username' is  your github username).  While this may be sufficient for some projects or personal pages, you usually want your page accessible through a custom URL.  For this gig, I want to use 'agilephd.com'.

There are dozens, if not hundreds, of domain registration firms.  I've used several and at the moment, [GoDaddy](https://www.godaddy.com) is my favorite.  If you don't already have one, create an account with GoDaddy.

GoDaddy allows users to search for domain names.  If your desired domain name isn't available, GoDaddy offers suggestions.  I was lucky and registered 'agilephd.com' years ago.  

\begin{aside}
\label{aside:polytex_markdown}
\heading{Domain Names}

\noindent What's in a domain name?  Names can be meaningful or meaningless.  IBM.com and LSU.edu
are very meaningful names.  I imagine the discussion for lsu.edu started with
"We are Louisiana State University and must have lsu.edu".  

To me, other domain name, such as heroku.com, are a less meaningful in that the
founders likely started with the question "What domain names are available?
Heroku.com is available and sounds cool...let's get it."
\end{aside}

After finding the appropriate domain name, you register it with GoDaddy.  During the registration process, domain registration firms are usually very persistent about up-selling additional services to you.  These services may include hiding your registration name, SSL, coming soon pages, templates, and web development services.  You don't need any of those and can opt out.

Once you have purchased a domain name, you must keep it registered with a registration service to maintain ownership.  

## The Connection

The last step is to select the DNS servers for your newly registered domain such that it 'points' to the location of your content.  You could configure GoDaddy to use GitHub's DNS servers but I use a service called CloudFlare.

### Configure CloudFlare

![cloudflare](images/cloudflare_main.png)

Create an account with CloudFlare and add your website to your account.

![cloudflare](images/cloudflare_addsite.png)

Like many other SaaS solutions, CloudFlare utilizes the freemium model.  At the free level, you get basic routing and CloudFlare's robust protection service.  In the past, we had to buy expensive hardware solutions from companies like Cisco and configure complicated firewall servers.  CloudFlare keeps your application safer (there is no such thing as completely safe) from threats such as DDOS attacks and other evilness in cyber world.

\begin{aside}
\label{aside:polytex_markdown}
\heading{Nameservers}

\noindent To use Cloudflare, you need to change your domain's authoritative DNS servers, which
are also referred to as just 'nameservers'. This change would be made in your
hosting service account (GoDaddy).

As a reference, these are an example of Cloudflare nameservers you may be assigned.
```
isla.ns.cloudflare.com
ken.ns.cloudflare.com
```

\end{aside}

In my GoDaddy account, I change the DNS settings to the nameservers provided by CloudFlare.

![GoDaddy DNS](images/godaddy_dns.png)

Back at CloudFlare page for your website, select the DNS page.  Similar to the image below, I will add CNAME and MX records routing traffic through the Cloudflare system.

* CNAME agilephd.com is an alias of cavalryjim.github.io
* CNAME www is an alias of agilephd.com
* MX agilephd.com	mail handled by mx2.zoho.com
* MX agilephd.com	mail handled by mx.zoho.com

![cloudflare](images/cloudflare_dns.png)

\begin{aside}
\label{aside:polytex_markdown}
\heading{DNS Record Types}

\noindent A Canonical Name or 'CNAME' record is a type of DNS record that maps an alias name to a true or canonical domain name.  An 'A' record returns a 32-bit IPv4 address such as '192.0.2.23'.  An 'MX' record is a mail exchange record that is used for mapping email traffic.

```
NAME                    TYPE   VALUE
--------------------------------------------------
bar.example.com         CNAME  foo.example.com
foo.example.com         A      192.0.2.23
example.com             MX     mx.mail.com
```
\end{aside}

DNS changes can take a few minutes to a few hours to propagate through the 'Internet'.  This delay is caused by Internet Service Providers and other entities caching DNS settings.  **Mental note: Don't make DNS changes before an important demo of your product.**

### Update GitHub
Lastly, you need to let GitHub know that you are using a custom domain name.
![GitHub Custom Domain](images/github-custom-domain.png)

At this point, you are ready to call your client (or co-workers) and tell them the new website is done and has been deployed.  Good job!


[^effort_footnote]: If you are in a startup organization, titles really don't matter and nothing should be considered 'beneath' you.  You and your co-founders should be willing to do everything and anything to see the venture move forward.
