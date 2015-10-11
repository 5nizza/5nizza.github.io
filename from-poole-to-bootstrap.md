How to go from poole to twitter-bootstrap:

- clone twitter-bootstrap saas version from 
  https://github.com/twbs/bootstrap-sass

- copy the content of bootstrap's folder `assets` into your assets folder

- modify your `_config.yml` -- change `saas_dir`:
  sass_dir:          assets/stylesheets

- copy `bootstrap-saas/templates/project/styles.saas` into your root directory
  (replacing your `styles.scss`)

- copy `bootstrap-saas/templates/project/_bootstrap-variables.saas` into 
  `assets/stylesheets/`

- modify `styles.saas`: add dummy YAML front matter
  (empty one, to force jekyll to process it)

- modify `_layouts/default.html` and add into the end:
`
    <!-- Bootstrap core JavaScript
    ================================================== -->
    <!-- Placed at the end of the document so the pages load faster -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
    <script src="{{ site.baseurl }}/assets/javascripts/bootstrap.min.js"></script>
`


DONE
