# Tasks - Building a website

## Prerequisites / Dependencies / Assumptions

1. You have a running Debian Linux docker container named `debian_linux`
2. You are either SSH'd into the container or have `docker exec` into the container
3. The container mounts a local `.\srv` directory to the container's `/srv` directory
  * `/srv/web` exists and is empty (or content can be overwritten)
4. SSH Keys and GPG Keys have been setup in GitHub

## Setting up a website

1. `cd /srv` - move to our project directory
2. Please create a simple three page website, with all files saved into the `./web` directory
  * The index page will have a header, footer, left navigation, with each component having a different thematic style, store this file into `./web/index.html`
  * The about us page will header a header, footer, left navigation, with each component having a different thematic style, store this file into `./web/about.html`
  * The content page will header a header, footer, left navigation, with each component having a different thematic style, store this file into `./web/content.html`
  * Each page should share an overall CSS theme but have some unique styles
  * The navigation on each page should reference the `index.html`, `about.html`, and `content.html` pages
  * The content page should have placeholders for images in the body of the page
    * Replace the image placeholders with actual images saved to `./web/images/<imagename[].png>`
3. Ensure that you Git `add`, `commit`, and `push`, all files to your GitHub repository
