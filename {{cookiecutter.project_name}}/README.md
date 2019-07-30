# {{ cookiecutter.project_name }}

{{ cookiecutter.description }}

### Development environment
1. Install pipenv and pre-commit
```
pip install pipenv pre-commit
```

2. Clone the repository
```
git clone git@github.com:hmrc/{{ cookiecutter.project_name }}.git
cd {{ cookiecutter.project_name }}
pre-commit install && pre-commit autoupdate
pipenv install && pipenv install tox
```

3. To run linters and tests
```
pipenv run tox
```

4. To run formatter
```
tox -e black
```

### Adding dependencies

- Runtime dependencies should be pinned in `setup.py` in `install_requires`.
- Testing dependencies should be added to `tox.ini` under `testenv.deps` .

### Setting up CI with Jenkins
{%- if cookiecutter.type == "lambda" %}
Create a PR (via a fork if you lack permissions) for this file: 

https://github.com/hmrc/infrastructure-jenkins-pipelines/blob/master/jobs/lambda/seed.groovy

It would cause [Infrastructure's Jenkins](https://jenkins.tools.management.tax.service.gov.uk/)
to pick up the Jenkinsfile and generate a pipeline for your lambda. 

Once the lambda is packaged and uploaded to S3, it can be accessed by terraform.
{%- elif cookiecutter.type == "package" %}
Create a PR for this file:

https://github.com/hmrc/build-jobs/blob/master/jobs/live/platsec.groovy

and add CI jobs, for example for a repo named `request-ssh-access`:
```groovy
// request-ssh-access ci
new PythonLibraryToxJobBuilder("$TEAM", 'request-ssh-access')
        .test()
        .release()
        .build this as DslFactory

// request-ssh-access pull-request
new PythonLibraryToxPullRequestBuilder("$TEAM", 'request-ssh-access')
        .test()
        .build this as DslFactory
```
{% endif %}
### License

This code is open source software licensed under the [Apache 2.0 License]("http://www.apache.org/licenses/LICENSE-2.0.html").