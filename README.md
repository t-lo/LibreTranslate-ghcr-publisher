# LibreTranslate publisher (with models)

This project provides automated Docker images of [LibreTranslate](https://github.com/LibreTranslate/LibreTranslate) that include all language models.

## Benefits of a container image with models included

- Faster container start-up time:
  The container with models included starts faster as it avoids downloading approximately 14 GB of data during start-up.
  This improves responsiveness and prevents liveness probe timeouts, avoiding unnecessary container restarts.
- Reduced bandwidth usage:
  By including the models in the container, each restart doesn't require re-downloading the language models, resulting in significant bandwidth savings (around 14 GB per restart) in container orchestration environments like Kubernetes or Docker Swarm.
- Offline usability:
  The self-contained image allows you to use LibreTranslate in private networks without internet connectivity.

## Drawbacks

- Larger container image size: The resulting container image size is 16 GB (~10GB compressed) due to the inclusion of language models.

## GitHub Workflows

- [`check-new-tag.yaml`](.github/workflows/check-new-tag.yaml) runs once per hour and checks for new tags in the [upstream repo](https://github.com/LibreTranslate/LibreTranslate).
  If upstream has a tag larger than what's available from https://github.com/t-lo/LibreTranslate/pkgs/container/libretranslate a build is triggeredi for that tag.
  The new build is tagged "latest".
  This action can be triggered manually.
- [`publish-ghcr.yml`](.github/workflows/publish-ghcr.yml) builds a LibreTranslate container image from the [upstream repo](https://github.com/LibreTranslate/LibreTranslate).
  The image is published to ghcr.io.
  Also, an email will be sent to notify one or more recipients of the new image.
  Optionally takes a version to build (tag; defaults to latest main), and a bool whether the image shall be tagged "latest".

### Secrets

Apart from `GITHUB_TOKEN`, secrets are needed only for the "new release available" email.

- `GITHUB_TOKEN`: repo token with write access to the libretranslate ghcr package for image publishing
- `SMTP_SERVER`: SMTP server to use for sending "new image available" email.
   **Leave empty / don't set to disable email report**.
- `SMTP_USER`: Username for logging in to SMTP server.
- `SMTP_PASSWORD`: Password for logging in to SMTP server. 
- `EMAIL_TO`: Recipient of the Good News
- `EMAIL_FROM`: Sender of the email. Usually `SMTP_USER@SMTP_SERVER`.

## Notes

For more details and usage instructions, please refer to the [LibreTranslate repository](https://github.com/LibreTranslate/LibreTranslate).
