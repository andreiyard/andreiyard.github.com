# Project Overview

This is a personal blog created with the [Hugo](https://gohugo.io/) static site generator. The site is configured to be multilingual, supporting English, Russian, and Serbian. The visual theme is based on [PaperMod](https://github.com/adityatelange/hugo-PaperMod).

The content is organized into posts and is written in Markdown. The blog covers topics such as network automation, cloud architecture, and infrastructure reliability.

The site is automatically built and deployed to GitHub Pages using a GitHub Actions workflow.

# Building and Running

## Running Locally

To run the blog locally for development, you need to have Hugo installed. You can then use the following command:

```bash
hugo server
```

This will start a local server, and you can view the site at `http://localhost:1313/`. The site will automatically reload when you make changes to the content or templates.

## Building for Production

The production build is handled by the GitHub Actions workflow. The command used to build the site is:

```bash
hugo --gc --minify --baseURL "https://andreiyard.github.com/"
```

This command generates the static files in the `public` directory.

# Development Conventions

## Adding New Posts

To create a new post, use the following command:

```bash
hugo new --kind post content/en/posts/your-post-name.md
```

This will create a new Markdown file in the `content/en/posts` directory with the appropriate front matter. You can then edit this file to add your content.

## Multilingual Content

The site is structured to support multiple languages. Content for each language is stored in a separate directory under `content`:

-   `content/en` for English
-   `content/ru` for Russian
-   `content/sr` for Serbian

When adding a new post, you can create a version for each language by placing the translated Markdown file in the corresponding directory.

## Theme Customization

The site uses the PaperMod theme. The theme files are located in the `themes/PaperMod` directory. To customize the theme, you can override the templates in the `layouts` directory. For example, to change the header, you can create a file at `layouts/partials/header.html`.
