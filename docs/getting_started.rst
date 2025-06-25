Getting started
===============
If you want to protect the forms of your web application using reCAPTCHA,
you'll need to go through the following steps:

- create the needed reCAPTCHA keys
- install the django-recaptcha package
- update Django's settings
- add a field to the form(s) that you want to protect

This guide will demonstrate these steps for Google's reCAPTCHA v2 Checkbox.
If you follow this guide, switching to a different version of reCAPTCHA afterwards should be trivial in most cases.


Creating reCAPTCHA keys
-----------------------
In order to generate reCAPTCHA keys, you need to be logged into your Google account and go to `Google's reCAPTCHA admin console <https://www.google.com/recaptcha/admin/create>`_.
Choose values for the following, and click the submit button:

- a descriptive label to identify the keys
- a reCAPTCHA type
- one or multiple domains and subdomains
- a Google Cloud project

The generated keys can only be used with the type of reCAPTCHA that you selected.
For example, keys for reCAPTCHA v2 Checkbox cannot be used with reCAPTCHA v2 Invisible or reCAPTCHA v3.

You must list every domain and subdomain that serves forms protected using the generated keys.
Usually, domain names like `example.com` or `www.example.com` are listed for keys that are used in production.
Keys used for local development typically have values such as `127.0.0.1` and `localhost`.

After the keys have been created, they're tied to the selected Google Cloud project, and can be managed through either the `legacy admin console <https://www.google.com/recaptcha/admin/>`_, or `Google Cloud's admin console <https://console.cloud.google.com/security/recaptcha>`_.

.. warning::

  You should keep your secret reCAPTCHA keys private at all times to prevent abuse.

Test keys
*********
As discussed in `their FAQ <https://developers.google.com/recaptcha/docs/faq#id-like-to-run-automated-tests-with-recaptcha.-what-should-i-do>`_, Google provides test keys for local development and testing.
However, these keys can only be used with reCAPTCHA v2 Checkbox and reCAPTCHA v2 Invisible:

- site key: ``6LeIxAcTAAAAAJcZVRqyHh71UMIEGNQ_MXjiZKhI``
- secret key: ``6LeIxAcTAAAAAGG-vFI1TnRWxMZNFuojJ4WifJWe``

You'll need to create your own test keys for local development and testing with reCAPTCHA v3.


Installation
------------

PyPI package
************
All release versions of django-recaptcha are published on the `Python Package Index <https://pypi.org/project/django-recaptcha/>`_.
So, you can list its name as a project dependency, or install it using your preferred Python package installer.

.. code-block:: sh

  $ python3 -m pip install -U django-recaptcha


Installation from source
************************
If you want to use the most recent version of django-recaptcha, then you can install django-recaptcha directly from its Git repository.

.. code-block:: sh

  $ python3 -m pip install -U git+https://github.com/django-recaptcha/django-recaptcha.git@main


Settings
--------
You need to make two changes to Django's settings before protecting forms with django-recaptcha.

First, you need to add ``"django-recaptcha"`` to the list of installed apps:

.. code-block:: python3

  INSTALLED_APPS = [
      ...,
      "django_recaptcha",
      ...
  ]

Second, you need to set the values of ``RECAPTCHA_PUBLIC_KEY`` and ``RECAPTCHA_PRIVATE_KEY``.
You can use the test keys provided by Google for now:

.. code-block:: python3

  RECAPTCHA_PUBLIC_KEY = "6LeIxAcTAAAAAJcZVRqyHh71UMIEGNQ_MXjiZKhI"
  RECAPTCHA_PRIVATE_KEY = "6LeIxAcTAAAAAGG-vFI1TnRWxMZNFuojJ4WifJWe"


Protecting forms
----------------
As the final step, you need to add a ``RecaptchaField`` with a ``RecaptchaV2Checkbox`` widget to every form that you want to protect:

.. code-block:: python3

  from django import forms
  from django_recaptcha.fields import ReCaptchaField
  from django_recaptcha.widgets import ReCaptchaV2Checkbox

  class MyForm(forms.Form):
      name = forms.CharField(max_length=100)
      captcha = RecaptchaField(widget=ReCaptchaV2Checkbox())


Next steps
----------
If you followed all of the steps in this guide, then now the forms of your web application should be protected by reCAPTCHA v2 Checkbox.
All that's left to do, is replacing the test keys with your own keys when you deploy to production.

If you want to use a different version of reCAPTCHA, all you need to do is:

1. create a new set of keys for the correct version of reCAPTCHA
2. replace the old keys with your new keys in Django's settings
3. change every RecaptchaField instance's widget to the corresponding widget
