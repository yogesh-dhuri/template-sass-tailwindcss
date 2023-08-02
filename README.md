# template-sass-tailwindcss

In short, according to their website, Tailwind CSS is a

utility-first CSS framework packed with classes like flex, pt-4, text-center and rotate-90 that can be composed to build any design, directly in your markup.


Rapidly build modern websites without ever leaving your HTML.

I know that I am completely implying the opposite of Tailwinds headline (see above) with this tutorial. But keep in mind that I do not want to judge the framework. I simply want to share another option of how to use it.

This small tutorial is just a personal recommendation of how to use this framework without getting into messy HTML code. Hence, getting into CSS definitely is required.

The one thing you should always remember is:

The best retirement arrangement for your code is to write it clean and readable.

Why do you tend to write messy HTML code when using Tailwind CSS?
As stated earlier, Tailwind is a utility-first framework that creates CSS classes describing what they are doing, e.g. text-center . In general, this is a good idea because you do not need to look what your CSS is doing while writing your HTML code. The bad thing is, that you tend to bloat your HTML with CSS classes that easily get over the recommended 80 characters per line and makes your code almost unreadable. Even more, if you want to use the responsive capabilities of this framework.

Setup
For this small tutorial I assume that you installed SASS and Tailwind CSS already in your project. You additionally need a local dev server, e.g. use npm install http-server and npx http-server to have a basic one.

I set up a template repository at Github. Just clone it and run npm install and you’re all set.

Example
To demonstrate the setup, lets take a look at the example that is already present on Tailwinds website at the time of writing:

<figure class="md:flex bg-gray-100 rounded-xl p-8 md:p-0">
  <img class="w-32 h-32 md:w-48 md:h-auto md:rounded-none rounded-full mx-auto" src="/sarah-dayan.jpg" alt="" width="384" height="512">
  <div class="pt-6 md:p-8 text-center md:text-left space-y-4">
    <blockquote>
      <p class="text-lg font-semibold">
        “Tailwind CSS is the only framework that I've seen scale
        on large teams. It’s easy to customize, adapts to any design,
        and the build size is tiny.”
      </p>
    </blockquote>
    <figcaption class="font-medium">
      <div class="text-indigo-600">
        Sarah Dayan
      </div>
      <div class="text-gray-500">
        Staff Engineer, Algolia
      </div>
    </figcaption>
  </div>
</figure>

Actually this example had to be adjusted a little bit, because some of the classes are not available by default, e.g. text-cyan-600 does not exist (so we use Indigo). In addition to that, the example itself does not look like the one that is “representing” it on their website, it looks a bit different. The given image has been replaced by this one from flaticon.com.

Visual representation of the example that is given on the TailwindCSS website.
Screenshot by author, Icon from flaticon.com
This example already has so much classes that you get more than 80 characters in a line, so it is perfect for our case.

Approach
The approach uses the file paths of the example repository mentioned earlier. If you clone this repository, you get the complete process already and you can start development from there.

Build process

To be able to use Tailwind with SASS, it is required that is being build before the SASS compiler runs. Therefore a few scripts are required to be set in the package.json :

// some config before
"scripts": {
    "build-tailwind": "npx tailwindcss-cli@latest build -o build/tailwind.css",
    "concat-css": "concat -o build/main.concat.scss build/tailwind.css scss/main.scss",
    "compile-sass": "node-sass build/main.concat.scss public_html/css/main.css",
    "purge-css": "npx purgecss --config ./purgecss.config.js",
    "build": "npm run build-tailwind && npm run concat-css && npm run compile-sass",
    "build-prod": "npm run build-tailwind && npm run concat-css && npm run compile-sass && npm run purge-css"
  },
// some config afterwards

In there, the scripts do the following:

build-tailwind compiles the tailwind framework
concat-css concats tailwind css and your main.scss, so the SASS compiler does recognize all CSS classes that tailwind produces
compile-sass runs the node-sass compiler on your scss/main.scss
purge-css compares the resulting CSS file to your HTML and removes unused declarations
build runs the first three steps mentioned above, used for development
build-prod runs all four steps that are required to build your final CSS ready for production
Usually Tailwind purges all unused CSS classes when NODE_ENV=production is set. Because we modified the build process and moved all classes into a SCSS file it is required that we manually run PurgeCSS after generating the complete CSS file. That is what purge-css script does.

scss/main.scss

Because our goal is to simplify our HTML, this is our entrypoint for every CSS class that we use. The exact implementation depends on your requirements. For demonstration purpose, I chose to move every class from our HTML code into this file, assuming that every figure should look the same on our final webpage.

The final scss/main.scss file looks like the following:

figure {
    @extend .bg-gray-100;
    @extend .rounded-xl;
    @extend .p-8;
    @extend .md\:flex;
    @extend .md\:p-0;
    img {
        @extend .w-32;
        @extend .h-32;
        @extend .rounded-full;
        @extend .mx-auto;
        @extend .md\:w-48;
        @extend .md\:h-auto;
        @extend .md\:rounded-none;
    }
    & > div {
        @extend .pt-6;
        @extend .text-center;
        @extend .space-y-4;
        @extend .md\:p-8;
        @extend .md\:text-left;
        blockquote p {
            @extend .text-lg;
            @extend .font-semibold;
        }
        figcaption {
            @extend .font-medium;
        div.name {
                @extend .text-indigo-600;
            }
        div.work-role {
                @extend .text-gray-500;
            }
        }
    }
}
public_html/index.html

The resulting HTML file looks a lot cleaner now:

<figure>
  <img src="img/175062.png" alt="" width="384" height="512">
  <div>
    <blockquote>
      <p>
        “Tailwind CSS is the only framework that I've seen scale
        on large teams. It’s easy to customize, adapts to any design,
        and the build size is tiny.”
      </p>
    </blockquote>
    <figcaption>
      <div class="name">
        Sarah Dayan
      </div>
      <div class="work-role">
        Staff Engineer, Algolia
      </div>
    </figcaption>
  </div>
</figure>

Conclusion
In this short tutorial I wanted to show you an option how you can use TailwindCSS that scales without bloating up your HTML code. This enables you to develop in a more modular approach and adds a lot of flexibility for you.

I hope you enjoyed reading! I am happy to answer any questions in the comments!
