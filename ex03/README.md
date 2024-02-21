# Writing Images with Dockerfile

In this exercise, you'll learn how to:
* Write your first Dockerfile
* Build your first Docker image

## Better configuration using Dockerfile

In Docker ecosystem, to build your specific images for your application, you cannot use `docker commit` all the time. There are caveats and sometimes does not satisfy your needs for application. Therefore, it's better to use Dockerfile to create your customized image to be used in repeatedly.

In this exercise, let's try running a HTTP server with an HTML page.

1. To create a Dockerfile, just make a new one in the root of any of your projects,
   
   ```sh
   project-root $ touch Dockerfile
   ```

2. Open the `Dockerfile` with your text editor, and we'll start by specifying what image to be used; like a recipe, we can always change and optimize them in any way we want.
   
   Add some headers into the `Dockerfile`,
   * `FROM` - a directive to specify base image.

   ```docker
   FROM httpd:latest
   ```

3. We can also run commands during the creation of containers, for example, let's install a better editor for editing any web pages inside of the container.

   The `RUN` directive allows us to do so.

   ```docker
   RUN apt-get update && apt-get install -y vim
   ```

4. Now, we can copy some of our project's files into the container (using `docker commit` would be too difficult to do so) by specifying `COPY` directive:
   
   ```docker
   # COPY <src> <dest>
   COPY ./public-html/ /usr/local/apache2/htdocs/
   ```

   Then we, can save and close the file.

5. After that, we are ready to build and run our new image.
   
   ```sh
   # docker build -t <image-name> .
   docker build -t 'gdscku/web-example' .
   ```

6. Try running the container with opening ports to the container,
   
   To open ports when running the container we can use, `-p <host port>:<container port>`

   ```sh
   docker run -d -p --rm 8080:80 gdscku/web-example
   ```

   Then, you can open the site on `http://localhost:8080`

### Facts

* To use `docker run` without specifying any commands, an `ENTRYPOINT` must be specified in thier Dockerfile as well.

* There are more directives on Dockerfile that you can use, however you'll need to learn more: https://docs.docker.com/engine/reference/builder/