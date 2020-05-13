OpenHub used Github Actions for CI workflow. Actions are as per recorded at `.github/workflows`.

## Available Actions
  * `build.yml` - run when code is push to `master`. This will automatically pack, create and deploy `openhub-master.zip` to designated S3 bucket set at Secret. 
  * `release.yml` - run when code is tag for release. This will automatically pack, create and deploy `openhub-latest.zip` and `openhub-vx.y.zip` to designated S3 bucket set at Secret.

## Secret
Makes sure you had set these secrets before running any actions at github project settings page:
  * `AWS_S3_BUCKET`
  * `AWS_ACCESS_KEY_ID`
  * `AWS_SECRET_ACCESS_KEY`