JCDS
====

Jamf Cloud Distribution System aka CCM.

The base API URL is ``https://ccm.jamfcloud.com``

the current r/w bucket token may only be retrieved by creating a session in the system and reading the `data-upload-token`
attribute of the div with class `chunked-uploader`. This appears when you are uploading a new package.

The uploader element exists within an iframe pointing to `legacy\packages.html?id=-1&o=c`

This element also contains an attribute `data-base-url` of which the last URI segment is the bucket ID for the instance.

The bucket ID should theoretically not change for the life of the instance.


.. http:post:: /api/file/v1/(str: bucket_id)/(str: filename)/part?chunk=0&chunks=1

   Upload a package named `filename` to the tenant identified by `bucket_id`

   **Example request**:

   .. sourcecode:: http

       POST /api/file/v1/bucketid/jamf.pkg/part?chunk=0&chunks=1 HTTP/1.1
       Host: ccm.jamfcloud.com
       Content-Type: multipart/form-data
       X-Auth-Token: ccmauthtoken

   **Example response**:

   .. sourcecode:: http

        HTTP/1.1 201 Created
        Content-Type: application/json

        {
            "id":"random hex string",
            "filename":"jamf.pkg",
            "size":0,
            "version":1,
            "status":"PENDING",
            "md5":"",
            "created":"2017-11-15T05:19:18.000Z",
            "lastModified":"2017-11-15T05:19:18.000Z"
        }

   :query chunk: the current chunk, zero indexed
   :query chunks: the total chunk count
   :reqheader Content-Type: multipart/form-data
   :reqheader X-Auth-Token: The JCDS read/write authorization token. Cannot be determined from API.
   :resheader Content-Type: application/json
   :statuscode 201: package (chunk) uploaded successfully


.. http:get:: /api/file/v1/(str: bucket_id)

   Get all files

.. http:get:: /api/file/v1/(str: bucket_id)/(str: filename)

   Get file info

   **Example response**:

   .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
            "id":"random hex string",
            "filename":"jamf.pkg",
            "size":13047,
            "version":1,
            "status":"READY",
            "md5":"99aac006dfeb1f3ebc0854528c801eeb",
            "created":"2017-11-15T06:06:35.000Z",
            "lastModified":"2017-11-15T06:06:35.000Z"
        }

   :reqheader X-Auth-Token: The JCDS read/write or r/o authorization token. Cannot be determined from API.
   :resheader Content-Type: application/json
   :statuscode 200:

.. http:get:: /api/file/v1/(str: bucket_id)/(str: filename)/(str: version)

   Get file version info

.. http:delete:: /api/file/v1/(str: bucket_id)/(str: filename)

   Delete a file

.. http:get:: /api/token/v1/(str: bucket_id)/(str: token_type)?type=token_type

   Validate the current token

.. http:post:: /api/token/v1/(str: bucket_id)?activation=(str: activation_code)

   Create new set of tokens. I seem to get 403 even with basic auth

.. http:post:: /api/token/v1/(str: bucket_id)/(str: renewal_token)

   Get a new set of tokens from a renewal token

.. http:delete:: /api/token/v1/(str: bucket_id)/(str: renewal_token)

   Revoke all tokens associated with a renewal token

.. http:post:: /api/bucket/v1/(str: endpoint_id)?activation=(str: activation_code)

   Init a new bucket.

   The endpoint ID is determined when the instance is allocated. The end user has no visibility of the endpoint ID.

.. http:get:: /api/usage/v1/(str: bucket_id)?startDate=(date)&endDate=(date)

   Get CCM usage

.. http:get:: /api/activation/v1/(str: activation_code)

   Figure out whether this activation code is enabled for CCM/JCDS

