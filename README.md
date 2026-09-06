# Surfing remote configuration

This repository publishes a sanitized Mihomo configuration template for the
Surfing Android module. The real proxy subscription block remains on each
phone and is merged into the downloaded template before validation.

Update `config.yaml` on the `main` branch to publish a new configuration.
Connected phones check for changes every six hours and keep a local backup
before restarting Surfing.

Do not replace `__LOCAL_SUBSCRIPTION_URL__` with a real subscription URL.
