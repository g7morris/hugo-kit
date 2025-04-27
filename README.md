# hugo-kit

Run a local Hugo site for development in Docker and Docker-compose.

## Resources

* Hugo - https://gohugo.io/
* Hugo Quick start - https://gohugo.io/getting-started/quick-start/
* This project uses the [Hugomods](https://docker.hugomods.com/) docker [image](https://hub.docker.com/r/hugomods/hugo/tags)

---

## 1. Scaffold

* Clone this project
  * `git clone git@github.com:g7morris/hugo-kit.git`
* `cp sample.env .env`
* Edit the .env file and set your site name.
  * This should match the domain name you plan to use, or the name of your private GitHub repository — _often they're the same._

* Run the following command to scaffold a new Hugo site into the project directory as a subfolder.

    ```bash
    docker-compose -f docker-compose.init.yml up
    ```

* While Hugo just created the subfolder structures, files and content: e.g. `hugo.toml`, `archetypes/`, `content/`, etc. **no theme is installed yet**.

---

## 2. Add a theme

* **Assumptions**
  * In this setup the [Minimo theme](https://github.com/MunifTanjim/minimo) will be used. _Change the theme as needed._
    * The Minimo theme will be added as a git submodule

* Run a one-off Docker Compose container with bash
  * `docker-compose run --rm hugo sh`
    * Drops one inside the container shell.
    * Working directory will already be `/site` as mounted.

* Within the container
  * Add the Minimo theme as a Git submodule and within the `themes/` directory
  ```bash
    git init       # if not already initialized
    git config --global --add safe.directory /site
    git submodule add https://github.com/MunifTanjim/minimo.git themes/minimo
  ```

* (Optional) — For a faster setup, copy (and overwrite) the theme’s example `config.toml` to the site's default `hugo.toml`.
  * `cp themes/minimo/exampleSite/config.toml hugo.toml`
  * Then edit the top lines of hugo.toml to match your local development environment:
  ```bash
    baseURL = "http://localhost:1313"
    languageCode = "en-us"
    title = "Your Site Title"
  ```

* To stage, commit and push changes to the Hugo site
  ```bash
    git branch -M main
    git add .
    git commit -m "Add Minimo theme and basic Hugo site scaffold"
    git remote add origin git@github.com:user/usersite.com.git  # add your private repo remote (once)
    git push -u origin main
  ```

* Shutdown
  * `exit`

---

## 3. For development

```bash
docker-compose up -d
```

* View the site at http://localhost:1313