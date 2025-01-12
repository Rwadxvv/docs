---
title: Proteger las subidas con el escaneo de secretos
intro: 'Puedes utilizar el {% data variables.product.prodname_secret_scanning %} para prevenir que los secretos compatibles se suban a tu organización o repositorio si habilitas la protección contra subidas.'
product: '{% data reusables.gated-features.secret-scanning %}'
miniTocMaxHeadingLevel: 3
versions:
  feature: secret-scanning-push-protection
redirect_from:
  - /early-access/code-security/secret-scanning/protecting-pushes-with-secret-scanning
type: how_to
topics:
  - Secret scanning
  - Advanced Security
  - Alerts
  - Repositories
shortTitle: Protección contra subidas
---

{% data reusables.secret-scanning.beta %}
{% data reusables.secret-scanning.enterprise-enable-secret-scanning %}
{% data reusables.secret-scanning.push-protection-beta %}

## Acerca de la protección contra subidas para los secretos

Has ahora, el {% data variables.product.prodname_secret_scanning_GHAS %} verifica secretos _después_ de una subida y alerta a los usuarios sobre los secretos expuestos. {% data reusables.secret-scanning.push-protection-overview %}

El {% data variables.product.prodname_secret_scanning_caps %} como protección contra subidas actualmente escanea los repositorios para encontrar secretos que hayan emitido los siguientes proveedores de servicios.

{% data reusables.secret-scanning.secret-list-private-push-protection %}

## Habilitar el {% data variables.product.prodname_secret_scanning %} como una protección contra subidas

Para que puedas utilizar el {% data variables.product.prodname_secret_scanning %} como protección contra subidas, el repositorio u organización necesita tener habilitados tanto la {% data variables.product.prodname_GH_advanced_security %} como el {% data variables.product.prodname_secret_scanning %}. Para obtener más información, consulta las secciones "[Administrar los ajustes de seguridad y análisis de tu organización](/organizations/keeping-your-organization-secure/managing-security-and-analysis-settings-for-your-organization)", "[Administrar los ajustes de seguridad y análisis de tu repositorio](/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-security-and-analysis-settings-for-your-repository)" y "[Acerca de la {% data variables.product.prodname_GH_advanced_security %}](/get-started/learning-about-github/about-github-advanced-security)".

Organization owners, security managers, and repository administrators can enable push protection for {% data variables.product.prodname_secret_scanning %} via the UI and API. Para obtener más información, consulta la sección "[Repositorios](/rest/reference/repos#update-a-repository)" y expande la sección de "Propiedades del objeto `security_and_analysis`" en la documentación de la API de REST.

### Enabling {% data variables.product.prodname_secret_scanning %} as a push protection for an organization

{% data reusables.organizations.navigate-to-org %}
{% data reusables.organizations.org_settings %}
{% data reusables.organizations.security-and-analysis %}
{% data reusables.repositories.navigate-to-ghas-settings %}
{% data reusables.advanced-security.secret-scanning-push-protection-org %}

### Enabling {% data variables.product.prodname_secret_scanning %} as a push protection for a repository

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.repositories.navigate-to-code-security-and-analysis %}
{% data reusables.repositories.navigate-to-ghas-settings %}
{% data reusables.advanced-security.secret-scanning-push-protection-repo %}


## Using {% data variables.product.prodname_secret_scanning %} as a push protection from the command line

When you attempt to push a supported secret to a repository or organization with {% data variables.product.prodname_secret_scanning %} as a push protection enabled, {% data variables.product.prodname_dotcom %} will block the push. You can remove the secret from your commit or follow a provided URL to allow the push.

Up to five detected secrets will be displayed at a time on the command line. Si ya se detectó un secreto en particular en el repositorio y la alerta ya existe, {% data variables.product.prodname_dotcom %} no lo bloqueará.

![Screenshot showing that a push is blocked when a user attempts to push a secret to a repository](/assets/images/help/repository/secret-scanning-push-protection-with-link.png)

Si necesitas eliminar el secreto de tu última confirmación (es decir, `HEAD`) en la rama que se está subiendo y cualquier confirmación anterior que lo contenga, puedes eliminarlo de `HEAD` y luego combinar las confirmaciones que haya entre ellos cuando la confirmación se introdujo y la primera versión de `HEAD` para la cual se eliminó el secreto.

{% note %}

**Notas**:

* If your git configuration supports pushes to multiple branches, and not only to the default branch, your push may be blocked due to additional and unintended refs being pushed. For more information, see the [`push.default` options](https://git-scm.com/docs/git-config#Documentation/git-config.txt-pushdefault) in the Git Docs.
* If {% data variables.product.prodname_secret_scanning %} upon a push times out, {% data variables.product.prodname_dotcom %} will still run a scan after the push.

{% endnote %}

### Allowing a blocked secret to be pushed

Si {% data variables.product.prodname_dotcom %} bloquea un secreto que piensas se puede subir con seguridad, puedes permitirlo y especificar la razón por la cual se debería de permitir.

Si confirmas que un secreto es real y que pretendes corregirlo después, debes intentar remediarlo tan pronto como sea posible. For example, you might revoke the secret and remove the secret from the repository's commit history. For more information, see "[Removing sensitive data from a repository](/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)."

{% data reusables.secret-scanning.push-protection-allow-secrets-alerts %}

1. Visit the URL returned by {% data variables.product.prodname_dotcom %} when your push was blocked. ![Captura de pantalla que muestra un formulario con opciones para desbloquear la subida de un secreto](/assets/images/help/repository/secret-scanning-unblock-form.png)
{% data reusables.secret-scanning.push-protection-choose-allow-secret-options %}
1. Click **Allow me to push this secret**.
2. Reattempt the push on the command line within three hours. If you have not pushed within three hours, you will need to repeat this process.

{% ifversion secret-scanning-push-protection-web-ui %}
## Using secret scanning as a push protection from the web UI

When you use the web UI to attempt to commit a supported secret to a repository or organization with secret scanning as a push protection enabled, {% data variables.product.prodname_dotcom %} will block the commit. You will see a banner at the top of the page with information about the secret's location, and the secret will also be underlined in the file so you can easily find it.

  ![Screenshot showing commit in web ui blocked because of secret scanning push protection](/assets/images/help/repository/secret-scanning-push-protection-web-ui-commit-blocked-banner.png)

{% data variables.product.prodname_dotcom %} will only display one detected secret at a time in the web UI. Si ya se detectó un secreto en particular en el repositorio y la alerta ya existe, {% data variables.product.prodname_dotcom %} no lo bloqueará.

You can remove the secret from the file using the web UI. Una vez que elimines el secreto, el letrero en la parte superior de la página cambiará y te dirá que ahora puedes confirmar tus cambios.

  ![Captura de pantalla que muestra la confirmación en la Iu web después de que se corrigió un secreto](/assets/images/help/repository/secret-scanning-push-protection-web-ui-commit-allowed.png)

### Saltar la protección contra subidas para un secreto

Si {% data variables.product.prodname_dotcom %} bloquea un secreto que piensas se puede subir con seguridad, puedes permitirlo y especificar la razón por la cual se debería de permitir. Si confirmas que un secreto es real y que pretendes corregirlo después, debes intentar remediarlo tan pronto como sea posible.

{% data reusables.secret-scanning.push-protection-allow-secrets-alerts %}

Si confirmas que un secreto es real y que pretendes corregirlo después, debes intentar remediarlo tan pronto como sea posible.

1. En el letrero que aparece en la parte superior de la página cuando {% data variables.product.prodname_dotcom %} bloqueó tu confirmación, haz clic en **Saltar protección**.
{% data reusables.secret-scanning.push-protection-choose-allow-secret-options %}

  ![Captura de pantalla que muestra un formulario con opciones para desbloquear la subida de un secreto](/assets/images/help/repository/secret-scanning-push-protection-web-ui-allow-secret-options.png)

1. Haz clic en **Permitir secreto**.

{% endif %}
