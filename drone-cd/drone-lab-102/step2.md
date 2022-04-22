### Code Validation

So we are learning about CI still at this stage, and part of any pipeline should include some type of automated validation of what you are doing so  that you know it's not going to be junk once it is made into an artifact.

The following is the drone pipeline `step`, a pipeline can have multiple steps and a configuration file can have multiple pipelines.  Copy the code below and place it into the steps level of the `class` pipeline in the file `/class/.drone.yml` using the Katacoda editor (top right).

<pre class="file" data-target="clipboard">
- name: code-validation
  image: philipharries/html_tidy
  commands:
    - tidy -eq *.htm*
  when:
    event: [ pull_request ]
</pre>

**HINT**: The `when` clause can be implemented to define when it is important for the step to execute.  Automated testing is a reassurance of a first glance to any reviewers.

### Testing Code

Right, you have the code above in-place, let's test your `index.html` file using drone to see if you have proper html syntax!

`cd /class \
&& drone exec --trusted \
              --repo class \
              --branch master \
              --pipeline class \
              --event pull_request \
&& echo success || echo failed with $?`{{execute CLIENT}}

**NOTICE**: This might take a few minutes if the host still has to download the `html_tidy` docker image before it gives feedback.

**HINT**: Pull requests / PR's are a fantastic way to ensure you have a hardened step for testing / review prior to a merge activity.
