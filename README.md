# Partner API & Supplier Portal

This repository hosts the public documentation for ShopJimmy’s Partner API and the forthcoming supplier portal. The API currently supports token-based authentication along with core workflows such as part discovery, order placement, order lifecycle management, and post-sale reconciliation.

## API Features

The Partner API presently offers the following capabilities:

- Request a JSON Web Token from `/token` for authenticated access.
- Discover inventory through `/search` and review individual listings via `/listing/{listing_id}`.
- Retrieve shipping service options with `/shippingQuote`.
- Submit new purchase orders to `/order` and initiate cancellations through `/order/{reference}/request-cancel`.
- Access historical orders with `/orders` and view detailed status updates at `/order/{reference}`.
- Review credit memos using `/credits`.
- Summarize invoices with `/invoices` and obtain full invoice detail from `/invoice/{id}`.

Each Markdown page in this repository includes representative requests and responses for the corresponding endpoint.

## Supplier Portal Roadmap

Suppliers will ultimately engage through a dedicated portal to monitor commercial performance and payments. Planned functionality includes:

- A dashboard highlighting recent sales volume and top-performing parts with tag and category filters.
- Dynamic payout projections that reflect return activity and provide per-part breakdowns.
- Return management with reason codes, supporting imagery, and tracking of good versus defective boards.
- Secure storage for supplier contracts, associated metadata, and optional annual review reminders.
- A performance score informed by return rates, consignment mix, fee handling, and defect ratios, accompanied by tools for suppliers to adjust consignment and fee settings.
- Administrative payout approval flows that will ultimately connect to automated ACH disbursements; manual confirmation will remain available until the banking integration is complete.
- A transparent deduction summary for every part on a payout.
- Protection of end-customer privacy by presenting only destination city and state (or an equivalent map view).

## Remaining Work

- Implement supplier authentication and login experiences.
- Build the supplier dashboard, including sales summaries and top-part analytics.
- Deliver interactive payout estimates with per-part insights.
- Complete return reporting, defect tracking, and supporting photo uploads.
- Finalize contract upload, storage, and metadata capture with annual review reminders.
- Develop, expose, and tune the performance scoring formula.
- Enable supplier-controlled adjustments to consignment percentages and fee allocations.
- Complete the administrative payout approval flow and integrate ACH payments when the banking provider is ready.
- Continue operating a dedicated development environment for ongoing portal testing.

---

# Just the Docs Template

This repository was bootstrapped from the *Just the Docs* Jekyll template. The template provides a minimal starting point that:

- Applies the [Just the Docs] theme via a gem-based installation.
- Supports local previewing, GitHub Pages publication, and other static hosting workflows.
- Ships with a preconfigured [GitHub Pages / Actions workflow] for automated builds and deployments.

To create a new documentation site from this template:

1. Select **Use this template** to generate a new GitHub repository.
2. Navigate to **Settings → Pages → Build and deployment → Source** and choose **GitHub Actions**.

If you prefer to keep documentation inside an existing project repository, review [Hosting your docs from an existing project repo](#hosting-your-docs-from-an-existing-project-repo).

After provisioning the new site, tailor the provided content:

## Update the Default Pages

Replace the placeholder content in the following files with project-specific information:

- `index.md` for the public landing page.
- `README.md` for repository-level guidance.

## Adjusting Theme or Jekyll Versions

Modify the relevant entries in the `Gemfile` to pin alternate versions of Jekyll or the theme.

## Adding Additional Plugins

The template enables the [`jekyll-seo-tag`] plugin by default. To add another plugin, update both the `Gemfile` and `_config.yml`. For example, to include [`jekyll-default-layout`]:

- Append the following to the `Gemfile`:

  ```ruby
  gem "jekyll-default-layout"
  ```

- Add the plugin to `_config.yml`:

  ```yaml
  plugins:
    - jekyll-default-layout
  ```

For Jekyll versions earlier than 3.5.0, use the `gems` key instead of `plugins`.

## Publishing on GitHub Pages

1.  If your repository is `YOUR-USERNAME/YOUR-SITE-NAME`, configure `_config.yml` with:

    ```yaml
    title: YOUR TITLE
    description: YOUR DESCRIPTION
    ```

2.  Execute `bundle install` to install dependencies.

3.  Run `bundle exec jekyll serve` to compile the site and preview it locally at `localhost:4000`. The generated output is stored in the `_site` directory.

## Publishing to Alternate Platforms

To deploy the site to another hosting provider, publish the contents of the `_site` directory.

## Customization Guidance

The template is intentionally lightweight; adapt the structure, styling, and content as required for your documentation project. Additional customization resources are available in the [Just the Docs documentation][Just the Docs].

## Hosting Documentation Within an Existing Repository

If you prefer to manage documentation within a project’s primary repository, copy the template files into a `docs` directory and adjust the provided GitHub Actions workflow accordingly. Clone the template locally or download the `.zip` archive to access the files.

### Copy Required Files

1.  Create a `.github/workflows` directory at the repository root if one does not already exist, and copy `pages.yml` into it so GitHub Actions can locate the workflow definition.
2.  Create a `docs` directory at the repository root and copy the remaining template files into that location.

### Update the GitHub Actions Workflow

The `pages.yml` workflow must be updated so that build and deploy steps execute within the `docs` directory.

1.  Set the default `working-directory` for the build job:

    ```yaml
    build:
      runs-on: ubuntu-latest
      defaults:
        run:
          working-directory: docs
    ```

2.  Specify the `working-directory` for the **Setup Ruby** step:

    ```yaml
    - name: Setup Ruby
      uses: ruby/setup-ruby@v1
      with:
        ruby-version: '3.1'
        bundler-cache: true
        cache-version: 0
        working-directory: '${{ github.workspace }}/docs'
    ```

3.  Configure the upload step to publish the generated site from `docs/_site/`:

    ```yaml
    - name: Upload artifact
      uses: actions/upload-pages-artifact@v1
      with:
        path: "docs/_site/"
    ```

4.  Limit workflow triggers to changes within the `docs` directory to avoid unnecessary builds:

    ```yaml
    on:
      push:
        branches:
          - "main"
        paths:
          - "docs/**"
    ```

## Licensing and Attribution

This template is distributed under the [MIT License], granting broad rights to reuse or extend the codebase. Please retain the original license file when creating derivatives. Feedback on enhancements or improvements is always welcome.

The deployment workflow derives from GitHub’s [starter workflows]; a copy of their MIT License is provided in [actions/starter-workflows].

----

[^1]: [It can take up to 10 minutes for changes to your site to publish after you push the changes to GitHub](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll#creating-your-site).

[Jekyll]: https://jekyllrb.com
[Just the Docs]: https://just-the-docs.github.io/just-the-docs/
[GitHub Pages]: https://docs.github.com/en/pages
[GitHub Pages / Actions workflow]: https://github.blog/changelog/2022-07-27-github-pages-custom-github-actions-workflows-beta/
[Bundler]: https://bundler.io
[use this template]: https://github.com/just-the-docs/just-the-docs-template/generate
[`jekyll-default-layout`]: https://github.com/benbalter/jekyll-default-layout
[`jekyll-seo-tag`]: https://jekyll.github.io/jekyll-seo-tag
[MIT License]: https://en.wikipedia.org/wiki/MIT_License
[starter workflows]: https://github.com/actions/starter-workflows/blob/main/pages/jekyll.yml
[actions/starter-workflows]: https://github.com/actions/starter-workflows/blob/main/LICENSE
