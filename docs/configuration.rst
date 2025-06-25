Configuration
=============
django-recaptcha uses several settings that can be used to alter its behavior.


reCAPTCHA keys
--------------
The settings ``RECAPTCHA_PUBLIC_KEY`` and ``RECAPTCHA_PRIVATE_KEY`` are used to set the global values of public and private keys used for reCAPTCHA.
If you do not add these settings, django-recaptcha will use Google's test keys for reCAPTCHA v2.

.. code-block::

  RECAPTCHA_PUBLIC_KEY = "6LeIxAcTAAAAAJcZVRqyHh71UMIEGNQ_MXjiZKhI"
  RECAPTCHA_PRIVATE_KEY = "6LeIxAcTAAAAAGG-vFI1TnRWxMZNFuojJ4WifJWe"


.. note::

  You can set the public and private keys for individual ``RecaptchaField`` instances at instantiation.



reCAPTCHA v3 scores
-------------------
The setting ``RECAPTCHA_REQUIRED_SCORE`` is used to set the score used for reCAPTCHA v3.
If you do not add this setting, django-recaptcha will assume ``0.00`` as the default required score.

.. code-block::

  RECAPTCHA_REQUIRED_SCORE = 0.85


.. note::

  You can set the required score for individual ``RecaptchaField`` instances through their widgets.


Using proxies
-------------
If you require a proxy for the server-side verification of reCAPTCHA tokens, set ``RECAPTCHA_PROXY`` to a dictionary of proxies.
If you do not add this setting, django-recaptcha will not use any proxies.

.. code-block:: python3

  RECAPTCHA_PROXY = {'http': 'http://127.0.0.1:8000', 'https': 'https://127.0.0.1:8000'}


Using different domains
-----------------------
As per `Google's official reCAPTCHA FAQ <https://developers.google.com/recaptcha/docs/faq#can-i-use-recaptcha-globally>`_, the domain ``www.recaptcha.net`` can be used instead of ``www.google.com`` whenever the latter is not accessible.
To do this, set ``RECAPTCHA_DOMAIN`` to ``"www.recaptcha.net"``.
If you do not add this setting, django-recaptcha will use ``"www.google.com"``.

.. code-block::

  RECAPTCHA_DOMAIN = "www.recaptcha.net"

This changes the domain of the API used for the server-side verification of reCAPTCHA tokens, as well as the client-side URL used to load reCAPTCHA, and the client-side APIs.
