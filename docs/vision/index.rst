######
Vision
######

The Google Cloud `Vision`_ (`Vision API docs`_) API enables developers to
understand the content of an image by encapsulating powerful machine
learning models in an easy to use REST API. It quickly classifies images
into thousands of categories (e.g., "sailboat", "lion", "Eiffel Tower"),
detects individual objects and faces within images, and finds and reads
printed words contained within images. You can build metadata on your
image catalog, moderate offensive content, or enable new marketing
scenarios through image sentiment analysis. Analyze images uploaded
in the request or integrate with your image storage on Google Cloud
Storage.

.. _Vision: https://cloud.google.com/vision/
.. _Vision API docs: https://cloud.google.com/vision/reference/rest/


********************************
Authentication and Configuration
********************************

- For an overview of authentication in ``google-cloud-python``,
  see :doc:`/core/auth`.

- In addition to any authentication configuration, you should also set the
  :envvar:`GOOGLE_CLOUD_PROJECT` environment variable for the project you'd
  like to interact with. If the :envvar:`GOOGLE_CLOUD_PROJECT` environment
  variable is not present, the project ID from JSON file credentials is used.

  If you are using Google App Engine or Google Compute Engine
  this will be detected automatically.

- After configuring your environment, create a
  :class:`~google.cloud.vision.client.Client`.

.. code-block:: python

     >>> from google.cloud import vision
     >>> client = vision.ImageAnnotatorClient()

or pass in ``credentials`` and ``project`` explicitly.

.. code-block:: python

     >>> from google.cloud import vision
     >>> client = vision.Client(project='my-project', credentials=creds)


*****************
Annotate an Image
*****************

You can call the :meth:`annotate_image` method directly:

.. code-block:: python

    >>> from google.cloud import vision
    >>> client = vision.ImageAnnotatorClient()
    >>> response = client.annotate_image({
    ...   'image': {'source': {'image_uri': 'gs://my-test-bucket/image.jpg'}},
    ...   'features': [{'type': vision.enums.Feature.Type.FACE_DETECTOIN}],
    ... })
    >>> len(response.annotations)
    2
    >>> for face in response.annotations[0].faces:
    ...     print(face.joy)
    Likelihood.VERY_LIKELY
    Likelihood.VERY_LIKELY
    Likelihood.VERY_LIKELY
    >>> for logo in response.annotations[0].logos:
    ...     print(logo.description)
    'google'
    'github'


************************
Single-feature Shortcuts
************************

If you are only requesting a single feature, you may find it easier to ask
for it using our direct methods:

.. code-block:: python

    >>> from google.cloud import vision
    >>> client = vision.ImageAnnotatorClient()
    >>> response = client.face_detection({
    ...   'source': {'image_uri': 'gs://my-test-bucket/image.jpg'},
    ... })
    >>> len(response.annotations)
    1
    >>> for face in resposne.annotations[0].faces:
    ...     print(face.joy)
    Likelihood.VERY_LIKELY
    Likelihood.VERY_LIKELY
    Likelihood.VERY_LIKELY


****************
No results found
****************

If no results for the detection performed can be extracted from the image, then
an empty list is returned. This behavior is similiar with all detection types.


Example with :meth:`~google.cloud.vision.ImageAnnotatorClient.logo_detection`:

.. code-block:: python

    >>> from google.cloud import vision
    >>> client = vision.ImageAnnotatorClient()
    >>> with open('./image.jpg', 'rb') as image_file:
    ...     content = image_file.read()
    >>> response = client.logo_detection({
    ...     'content': content,
    ... })
    >>> len(response.annotations)
    0

*************
API Reference
*************

.. toctree::
  :maxdepth: 2

  gapic/api
  gapic/types
